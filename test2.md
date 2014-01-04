Requests and Responses
======================

[出处: http://blog.csdn.net/zouyee/article/details/16966185](http://blog.csdn.net/zouyee/article/details/16966185)

翻译如下:

教程 2: Requests and Responses
==============================

从这个角度我们将真正开始覆盖其他框架的核心。让我们介绍几个基本构建块。

**1. Request Object  ——Request对象**

rest framework 引入了一个继承自HttpRequest的Request对象，该对象提供了对请求的更灵活解析。request对象的核心部分是request.data属性，类似于request.post, 但在使用WEB API时，request.data更有效。

~~~~ {.prettyprint .lang-py code_snippet_id="82801" snippet_file_name="blog_20131126_1_5506918" name="code" style="white-space: pre-wrap; word-wrap: normal; color: rgb(51, 51, 51); font-size: 14px; padding: 8px; font-family: Monaco, Menlo, Consolas, 'Courier New', monospace; margin-top: 0px; margin-bottom: 20px; line-height: 20px; word-break: break-all; background-color: rgb(247, 247, 249); border: 1px solid rgb(225, 225, 232); overflow: auto;"}
request.POST  # Only handles form data.  Only works for 'POST' method.
request.DATA  # Handles arbitrary data.  Works any HTTP request with content.
~~~~

**2. Response Object ——Response对象**

rest framework引入了一个Response 对象，它继承自TemplateResponse对象。它获得未渲染的内容并通过内容协商content negotiation 来决定正确的content type返回给client。

~~~~ {.prettyprint .lang-py code_snippet_id="82801" snippet_file_name="blog_20131126_2_1951105" name="code" style="white-space: pre-wrap; word-wrap: normal; color: rgb(51, 51, 51); font-size: 14px; padding: 8px; font-family: Monaco, Menlo, Consolas, 'Courier New', monospace; margin-top: 0px; margin-bottom: 20px; line-height: 20px; word-break: break-all; background-color: rgb(247, 247, 249); border: 1px solid rgb(225, 225, 232); overflow: auto;"}
return Response(data)  # Renders to content type as requested by the client.
~~~~

**3. Status Codes**

在views当中使用数字化的HTTP状态码，会使你的代码不宜阅读，且不容易发现代码中的错误。rest framework为每个状态码提供了更明确的标识。例如HTTP\_400\_BAD\_REQUEST。相比于使用数字，在整个views中使用这类标识符将更好。

**4. 封装API views**

在编写API views时，REST Framework提供了两种wrappers：

1).   @api\_viwe 装饰器 ——函数级别

2). APIView 类——类级别

这两种封装器提供了许多功能，例如，确保在view当中能够接收到Request实例；往Response中增加内容以便内容协商content negotiation 机制能够执行。

封装器也提供一些行为，例如在适当的时候返回405 Methord Not Allowed响应；在访问多类型的输入request.DATA时，处理任何的ParseError异常。

**5. 汇总**

我们开始用这些新的组件来写一些views。

我们不在需要JESONResponse 类（在前一篇中创建），将它删除。删除后我们开始稍微重构下我们的view

~~~~ {.prettyprint .lang-py code_snippet_id="82801" snippet_file_name="blog_20131126_3_3968992" name="code" style="white-space: pre-wrap; word-wrap: normal; color: rgb(51, 51, 51); font-size: 14px; padding: 8px; font-family: Monaco, Menlo, Consolas, 'Courier New', monospace; margin-top: 0px; margin-bottom: 20px; line-height: 20px; word-break: break-all; background-color: rgb(247, 247, 249); border: 1px solid rgb(225, 225, 232); overflow: auto;"}
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer
@api_view(['GET', 'POST'])
def snippet_list(request):
    """
    List all snippets, or create a new snippet.
    """
    if request.method == 'GET':
        snippets = Snippet.objects.all()
        serializer = SnippetSerializer(snippets)
        return Response(serializer.data)
    elif request.method == 'POST':
        serializer = SnippetSerializer(data=request.DATA)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        else:
            return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
~~~~

上面的代码是对我们之前代码的改进。看上去更简洁，也更类似于django的forms api形式。我们也采用了状态码，使返回值更加明确。

下面是对单个snippet操作的view更新：

~~~~ {.prettyprint .lang-py code_snippet_id="82801" snippet_file_name="blog_20131126_4_9159442" name="code" style="white-space: pre-wrap; word-wrap: normal; color: rgb(51, 51, 51); font-size: 14px; padding: 8px; font-family: Monaco, Menlo, Consolas, 'Courier New', monospace; margin-top: 0px; margin-bottom: 20px; line-height: 20px; word-break: break-all; background-color: rgb(247, 247, 249); border: 1px solid rgb(225, 225, 232); overflow: auto;"}
@api_view(['GET', 'PUT', 'DELETE'])
def snippet_detail(request, pk):
    """
    Retrieve, update or delete a snippet instance.
    """              
    try:
        snippet = Snippet.objects.get(pk=pk)
    except Snippet.DoesNotExist:
        return Response(status=status.HTTP_404_NOT_FOUND)
    if request.method == 'GET':
        serializer = SnippetSerializer(snippet)
        return Response(serializer.data)
    elif request.method == 'PUT':
        serializer = SnippetSerializer(snippet, data=request.DATA)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        else:
            return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
    elif request.method == 'DELETE':
        snippet.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
~~~~

注意，我们并没有明确的要求requests或者responses给出content type。request.DATA可以处理输入的json请求，也可以输入yaml和其他格式。类似的在response返回数据时，REST Framework返回正确的content type给client。

**6. 给URLs增加可选的格式后缀**

利用在response时不需要指定content type这一事实，我们在API端增加格式的后缀。使用格式后缀，可以明确的指出使用某种格式，意味着我们的API可以处理类似[http://example.com/api/items/4.json](http://example.com/api/items/4.json).的URL。

增加format参数在views中，如：

~~~~ {.prettyprint .lang-py code_snippet_id="82801" snippet_file_name="blog_20131126_5_9541126" name="code" style="white-space: pre-wrap; word-wrap: normal; color: rgb(51, 51, 51); font-size: 14px; padding: 8px; font-family: Monaco, Menlo, Consolas, 'Courier New', monospace; margin-top: 0px; margin-bottom: 20px; line-height: 20px; word-break: break-all; background-color: rgb(247, 247, 249); border: 1px solid rgb(225, 225, 232); overflow: auto;"}
def snippet_list(request, format=None):
~~~~

and

~~~~ {.prettyprint .lang-py code_snippet_id="82801" snippet_file_name="blog_20131126_6_6083589" name="code" style="white-space: pre-wrap; word-wrap: normal; color: rgb(51, 51, 51); font-size: 14px; padding: 8px; font-family: Monaco, Menlo, Consolas, 'Courier New', monospace; margin-top: 0px; margin-bottom: 20px; line-height: 20px; word-break: break-all; background-color: rgb(247, 247, 249); border: 1px solid rgb(225, 225, 232); overflow: auto;"}
def snippet_detail(request, pk, format=None):
~~~~

现在稍微改动urls.py文件，在现有的URLs中添加一个格式后缀pattterns (format\_suffix\_patterns):

~~~~ {.prettyprint .lang-py code_snippet_id="82801" snippet_file_name="blog_20131126_7_5276011" name="code" style="white-space: pre-wrap; word-wrap: normal; color: rgb(51, 51, 51); font-size: 14px; padding: 8px; font-family: Monaco, Menlo, Consolas, 'Courier New', monospace; margin-top: 0px; margin-bottom: 20px; line-height: 20px; word-break: break-all; background-color: rgb(247, 247, 249); border: 1px solid rgb(225, 225, 232); overflow: auto;"}
from django.conf.urls import patterns, url
from rest_framework.urlpatterns import format_suffix_patterns
urlpatterns = patterns('snippets.views',
    url(r'^snippets/$', 'snippet_list'),
    url(r'^snippets/(?P<pk>[0-9]+)$', 'snippet_detail'),
)
urlpatterns = format_suffix_patterns(urlpatterns)
~~~~

这些额外的url patterns并不是必须的。

### Browsability

Because the API chooses the content type of the response based on the client request, it will, by default, return an HTML-formatted representation of the resource when that resource is requested by a web browser. This allows for the API to return a fully web-browsable HTML representation.

Having a web-browsable API is a huge usability win, and makes developing and using your API much easier. It also dramatically lowers the barrier-to-entry for other developers wanting to inspect and work with your API.

See the [browsable api](http://django-rest-framework.org/topics/browsable-api) topic for more information about the browsable API feature and how to customize it.

What's next?
------------

In [tutorial part 3](http://django-rest-framework.org/tutorial/3-class-based-views), we'll start using class based views, and see how generic views reduce the amount of code we need to write.
