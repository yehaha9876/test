[Mongoose简单学习笔记](/violet_day/article/details/16871703)
============================================================

### 安装Mongoose

~~~~ {.javascript code_snippet_id="76792" snippet_file_name="blog_20131121_1_7932286" name="code"}
$ npm install mongoose
~~~~

### 连接数据库

~~~~ {.javascript code_snippet_id="76792" snippet_file_name="blog_20131121_2_1280357" name="code"}
var mongoose = require('mongoose');
var Schema = mongoose.Schema;
mongoose.connect('mongodb://localhost/test');
~~~~

### 定义你的schema

这里的schema相当于模型，但是mongoose里面除了schma外还有一个model的概念。可以先暂时这样理解：我们首先定义schema，然后再由schema生成model，这个时候的model才拥有了与数据库交互的方法。

我们先定义一个博文的schema，如下。

~~~~ {.javascript code_snippet_id="76792" snippet_file_name="blog_20131121_3_9470212" name="code"}
var blogSchema = new Schema({
  title:  String,
  author: String,
  body:   String,
  comments: [{ body: String, date: Date }],
  date: { type: Date, default: Date.now },
  hidden: Boolean,
  meta: {
    votes: Number,
    favs:  Number
  }
});
~~~~

我们传递给mongoose的Schema构造器的对象结构中，key代表我们定义的属性名称，value则是这项属性的类型。目前mongoose允许的几种类型有：

-   String
-   Number
-   Date
-   Buffer
-   Boolean
-   Mixed
-   ObjectId
-   Array

这些类型中，除了Mixed、ObjectId是Schema.Types的属性外，其它都是javascript自有的属性。

定义完schema之后，我们由它生成model。

~~~~ {.javascript code_snippet_id="76792" snippet_file_name="blog_20131121_4_4836170" name="code"}
var Blog = mongoose.model('Blog', blogSchema);
~~~~

之后当我们需要初始化一条博文的记录的时候，我们就可以这样初始化：

~~~~ {.javascript code_snippet_id="76792" snippet_file_name="blog_20131121_5_4454486" name="code"}
var blog = new Blog({
    title: 'this is my blog title',
    author: 'me',
    body: 'the body of my blog. can you see that?'        
});
~~~~

### 数据库操作

定义完模型之后，我们就可以进行记录的增删查改了。mongoose的增删查改十分简单，以我们上一节的Blog模型为例，当我们初始化了一条博文的记录的时候，并没有将它保存进数据库中。这个时候，我们只要调用blog实例的save方法就可以将记录插入数据库中了。

~~~~ {.javascript code_snippet_id="76792" snippet_file_name="blog_20131121_6_9579679" name="code"}
blog.save();
~~~~

还可以给save方法传递一个错误回调函数，如果保存过程中发生错误将会调用该函数。

另外一种保存记录的方法是使用我们之前定义好的Blog对象，调用它的create方法：

~~~~ {.javascript code_snippet_id="76792" snippet_file_name="blog_20131121_7_8849331" name="code"}
Blog.create({ 
    title: 'another blog title', 
    author: 'still me', 
    body: 'the blog body again!' 
}, function (err, small) {
  if (err) return handleError(err);
  // saved!
});
~~~~

数据库中有了记录，下一步可能就是需要做查询了。例如，查询作者“me”的文章。一种方法是

~~~~ {.javascript code_snippet_id="76792" snippet_file_name="blog_20131121_8_8467647" name="code"}
Blog.find({ author: 'me' }).exec(callback);
~~~~

另一种方法是：

~~~~ {.javascript code_snippet_id="76792" snippet_file_name="blog_20131121_9_1674014" name="code"}
Blog.find({ author: 'me'}, callback);
~~~~

基本上所有涉及到查询的模型方法都有这样两种形式的查询方式，一种是不往查询方法中传递回调函数，这时查询方法不会立即执行查询，而是返回一个query对象，用户可以再在query对象上修改查询条件，直到执行exec(callback)方法；而第二种是往查询方法中传递回调函数，这时查询方法立即执行。

推荐使用前者，因为这样方便指定复杂的条件以及用于链式调用。例如我可以找出这样一篇博文：

~~~~ {.javascript code_snippet_id="76792" snippet_file_name="blog_20131121_10_4880380" name="code"}
Blog
.find({ author: 'me' })
.where('title').equals('this is title')
.where('meta.votes').gt(17).lt(66)
.limit(10)
.sort('-date')
.select('title author body')
.exec(callback);
~~~~

最后就是更新了。在mongoose里，模型的update方法是只做更新，并不会返回对象。如果需要获取要更新的对象，要使用模型的findOneAndUpdate方法。

~~~~ {.javascript code_snippet_id="76792" snippet_file_name="blog_20131121_11_5022084" name="code"}
Blog.update({ author: 'me' }, { title: 'new title' }, { multi: true }, function (err, numberAffected, raw) {
  if (err) return handleError(err);
  console.log('The number of updated documents was %d', numberAffected);
  console.log('The raw response from Mongo was ', raw);
});
~~~~


