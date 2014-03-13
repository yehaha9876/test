#1. 换行符

first line
second line

fouth line
libhab 这里只要一个空格

####强制换行

End a line with two or more spaces will create a hard linebreak, called `<br />` in HTML. ( Control + Return )
Above line ended with 2 spaces.
以两个或两个以上以上空格结尾


# 2. 链接自动生成

http://www.baidu.com


# 3. 代码高亮

一个tab或者四个空格：

    var Mou = exactlyTheAppIwant

```javascript
var s = "JavaScript syntax highlighting";
alert(s);

```


```python
def function():
#indenting works just fine in the fenced code block
s = "Python syntax highlighting"
print s
```


```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

```less
@nice-blue: #5B83AD;
@light-blue: @nice-blue + #111;
#header { color: @light-blue; }
```


```
No language indicated, so no syntax highlighting.
s = "There is no highlighting for this."
But let's throw in a tag.
```


# 4. Emoji（表情）

Sometimes you want to be :cool: and add some :sparkles: to your :speech_balloon:. Well we have a :gift: for you:
:exclamation: You can use emoji anywhere GFM is supported. :sunglasses:


# 5. 特殊符合开始


    @foo : for team members
    #123 : for issues
    !123 : for merge requests
    $123 : for snippets
    1234567 : for commits
    [file](path/to/file) : for file references



# 6. 标准markdown

Headers

# H1
## H2
### H3
#### H4
##### H5
###### H6

Alternatively, for H1 and H2, an underline-ish style:

Alt-H1
======

Alt-H2
------


###### ..Ab_c-d. e [anchor](url) ![alt text](url)..
链接为#ab_c-d-e-anchor
# 7. 字体

Emphasis, aka italics, with *asterisks* or _underscores_.

粗体
Strong emphasis, aka bold, with **asterisks** or __underscores__.

粗，粗斜体
Combined emphasis with **asterisks and _underscores_**.

删除线
Strikethrough uses two tildes. ~~Scratch this.~~


# 8. list
1. First ordered list item
2. Another item
* Unordered sub-list.
1. Actual numbers don't matter, just that it's a number
1. Ordered sub-list
4. And another item.


Some text that should be aligned with the above item.


* Unordered list can use asterisks
- Or minuses
+ Or pluses



# 9. 链接
* links:

[I'm an inline-style link](https://www.google.com)

[I'm a reference-style link][Arbitrary case-insensitive reference text]

相对地址
[../LICENSE](../LICENSE)

[aa啊/LICENSE](aa啊/LICENSE)

[/LICENSE/aa啊](/LICENSE/aa啊)


[You can use numbers for reference-style link definitions][1]

Or leave it empty and use the [link text itself][]

Some text to show that the reference links can follow later.

[arbitrary case-insensitive reference text]: https://www.mozilla.org
[1]: http://slashdot.org

[link text itself]: http://www.reddit.com

* email:

An email <example@example.com> link.


# 10. 图片
Inline-style:
![alt text](assets/logo-white.png)


Reference-style:
![alt text1][logo]
[logo]: assets/logo-white.png

# 11. 横线
Three or more...

---

Hyphens

***

Asterisks

___

Underscores


# 12. 表格

| header 1 | header 2 |
| -------- | -------- |
| cell 1 | cell 2 |
| cell 3 | cell 4 |



First Header | Second Header | Third Header
------------ | ------------- | ------------
Content Cell | Content Cell  | Content Cell
Content Cell | Content Cell  | Content Cell

If you wish, you can add a leading and tailing pipe to each line of the table:

| First Header | Second Header | Third Header |
| ------------ | ------------- | ------------ |
| Content Cell | Content Cell  | Content Cell |
| Content Cell | Content Cell  | Content Cell |

Specify alignement for each column by adding colons to separator lines:

First Header | Second Header | Third Header
:----------- | :-----------: | -----------:
Left         | Center        | Right
Left         | Center        | Right

aaa|bbb|ccc
------------|------------|------------
Content Cell | Content Cell  | Content Cell
Content Cell | Content Cell  | Content Cell
