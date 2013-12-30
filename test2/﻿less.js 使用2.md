[less.js 使用](http://www.cnblogs.com/breakdown/archive/2012/12/08/2796494.html)
================================================================================

关于less.js 一直认为是个钻了牛角尖的东西，被迫在项目利用了一段时间发现我错了。所以总结下

less文件demon

~~~~ {.brush:css;gutter:false;}
#header {
    .red;
    .a_line;
    a{ color: #000000;}
    span {
        font-size: 14px;
        color: #0F0;
    }
    .logo {
        width: 300px;
        &:hover {
            text-decoration: none
        }
    }
}


.red (){
    color: red;
    .co{ border: 1px solid red;}
}

.a_line(){
a:focus { outline:0;}
a{blr:~"expression(this.onFocus=this.close())";} // 只支持IE，过多使用效率低 
 a{blr:~"expression(this.onFocus=this.blur())";} // 只支持IE，过多使用效率低 
 a:focus { -moz-outline-style: none; } // IE不支持 
 a{ text-decoration: none}
}
~~~~

 less的优点

1.清晰的css逻辑结构，使处于\#header选择器的css样式全部写在\#header样式块中。

2.样式变量，less官网称为混合模式

其中.red (){ color: red; .co{ border: 1px solid red;} }与.red { color: red; .co{ border: 1px solid red;} }的写法，与效果是有本质区别的。

.red (){ color: red; .co{ border: 1px solid red;} } 相当建立了个样式变量，不会再css中显示，不会直接起作用，类似于未实例化。 只有在.class{}中调用才会起作用。调用的效果为.class{color: red; .co{ border: 1px solid red;}}这就防止了废弃不被使用css样式

.red {color: red; .co{ border: 1px solid red;}}是一个已经起作用的类，在.class{}中调用效果为 .class{color: red; .red{.co{ border: 1px solid red;}}}。

 

less官网自带winless一个将less自动生成css的工具，会自动监控指定的less文件，只要文件被更改就会自动更新生成新的被压缩的css文件。
