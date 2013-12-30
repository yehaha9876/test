[kindeditor 文本编辑器](/dingxingmei/article/details/17301205)
==============================================================

网上资源 ： http://kindeditor.net/demo.php

参数设置：http://kindeditor.net/docs/option.html\#allowimageupload

  \<link href="../../Scripts/kindeditor/themes/default/default.css" rel="stylesheet"
             type="text/css" /\>
         \<script src="../../Scripts/kindeditor/lang/zh\_CN.js" type="text/javascript"\>\</script\>
         \<script src="../../Scripts/kindeditor/kindeditor.js" type="text/javascript"\>\</script\>

简洁版的：

 var editor;
           KindEditor.ready(function (K) {
               editor = K.create('textarea[name="content"]', {
                   resizeType: 1,
                   allowPreviewEmoticons: false,
                   allowImageUpload: false,
                   items: [
                         'fontname', 'fontsize', '|', 'forecolor', 'hilitecolor', 'bold', 'italic', 'underline',
                         'removeformat', '|', 'justifyleft', 'justifycenter', 'justifyright', 'insertorderedlist',
                         'insertunorderedlist', '|', 'emoticons', 'image', 'link']
               });
           });

默认版：

 var editor;
           KindEditor.ready(function (K) {
               editor = K.create('textarea[name="content"]', {
                  allowFileManager:true
               });
           });

 \<textarea id="kindeditor" name="content"\>
    \</textarea\>

