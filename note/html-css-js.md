# 1. 异步提交带文件上传的表单

[参考](http://blog.sina.com.cn/s/blog_573a052b0100nt0u.html)

一般的表单都是通过ajax方式提交，所以碰到带文件上传的表单就比较麻烦。基本原理就是在页面增加一个隐藏iframe，然后通过ajax提交除文件之外的表单数据，在表单数据提交成功之后的回调函数中，通过form单独提交文件，而这个提交文件的form的target就指向前述隐藏的iframe。

```html
<html>   
<body>   
<form action="/upload.php" id="up_file" encType="multipart/form-data" method="post" target="hidden_frame" >   
    <input type="file" id="file" name="file" style="width:450">   
    <input type="submit" value="上传文件"><span id="msg"></span>   
    <br>   
    <font color="red">支持JPG,JPEG,GIF,BMP,SWF,RMVB,RM,AVI文件的上传</font>                 
</form>   
<iframe name='hidden_frame' id="hidden_frame" style='display:none'></iframe>   
</body>   
</html>
```

# 2. 实现动态的 icon

```html
<link rel="shortcut icon" href="data:image/png;base64,base64-content" type="image/png">
```

通过异步改变 href 里面的值即可实现

