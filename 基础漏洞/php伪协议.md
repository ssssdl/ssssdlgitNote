---
最常用的伪协议：php://filter/convert.base64-encode/resource= ...
---

# 关于伪协议

## 0x01 发现问题

来源于[http://hackinglab.cn](http://hackinglab.cn)上的一道脚本题

 `<?php    
    header("Content-type: text/html; charset=utf-8");  
    if (isset($_GET['view-source'])) {   
        show_source(__FILE__);   
        exit();   
    }   
  
    include('flag.php');   
  
    $smile = 1;    
  
   if (!isset ($_GET['^_^'])) $smile = 0;    
    if (preg_match ('/\./', $_GET['^_^'])) $smile = 0;    
    if (preg_match ('/%/', $_GET['^_^'])) $smile = 0;    
    if (preg_match ('/[0-9]/', $_GET['^_^'])) $smile = 0;    
    if (preg_match ('/http/', $_GET['^_^']) ) $smile = 0;    
    if (preg_match ('/https/', $_GET['^_^']) ) $smile = 0;    
    if (preg_match ('/ftp/', $_GET['^_^'])) $smile = 0;    
    if (preg_match ('/telnet/', $_GET['^_^'])) $smile = 0;    
    if (preg_match ('/_/', $_SERVER['QUERY_STRING'])) $smile = 0;   
    if ($smile) {   
        if (@file_exists ($_GET['^_^'])) $smile = 0;    
    }    
    if ($smile) {   
        $smile = @file_get_contents ($_GET['^_^']);    
        if ($smile === "(●'◡'●)") die($flag);    
    }    
?>    
<!doctype html>   
<html lang="en">   
<head>   
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">   
<title>Show me your smile :)</title>   
<link rel="stylesheet" href="style.css">   
</head>   
<body>   
<br><br><br><br><br><br><br>   
<div class="loginform cf">   
    <form name="login" action="index.php" method="POST" accept-charset="utf-8">   
        <ul>   
            <li>   
                <label for="SMILE">请使用微笑过关<a href="?view-source">源代码</a></label>   
                <input type="text" name="T_T" placeholder="where is your smile" required>   
            </li>   
            <li><input type="submit" value="Show"> </li>   
        </ul>   
    </form>   
</div>   
<div style="text-align:center;clear:both">   
</div>   
</body>   
</html>` 

因为`file_get_contents`看起来像是php伪协议。但是过滤了.和数字 ，这样base64就用不了，而且这根本是不能是读取一个存在于他服务器上的文件，这个文件既不能存在又要从这个读出`(●'◡'●)，查了很多资料，最后发现是我了解php伪协议太浅薄了`

payload：[http://lab1.xseclab.com/base13\_ead1b12e47ec7cc5390303831b779d47/index.php?^.^=data:text/plain,\(%E2%97%8F%27%E2%97%A1%27%E2%97%8F\)](http://lab1.xseclab.com/base13_ead1b12e47ec7cc5390303831b779d47/index.php?^.^=data:text/plain,%28%E2%97%8F%27%E2%97%A1%27%E2%97%8F%29)

### 0x02 关于其他的PHP伪协议

这个看了上面那个解题思路来源于write，学到的东西来源于这篇文章 http://www.4o4notfound.org/index.php/archives/31/  伪协议常用的还有一下四种php://、data://、zlib://、phar://

### php://

除了这个以前经常用的`php://filter/convert.base64-encode/resource=`读取一些源代码，还有一种php://input ，

#### php://filter

一般用于文件包含漏洞，可以配合base64实现任意代码读取，比如php://filter/read=convert.base64-encode/resource=1.php  但其实这个也可以还有一个作用就是执行一些本地的代码，比如php://filter//resource=1.php这个就会解析1.php里的代码，如果解析不了就会直接打印出来，而利用base64编码正是利用了解析不了的这个问题实现了源代码读取。

#### php://input

则可以配合post代码，实现任意代码执行和木马植入，同样是解析的话就会当作代码执行，解析不了的话就会直接打印出来。

### data://

常用的有两个

#### data:text/plain,xxxxxxx

如果是代码就执行，如果不是就返回字符串xxxxxxx

#### data:text/plain;base64,xxxxxxx

xxxxxx base64解码后的内容，如果是可以执行的代码就执行，不是的话就直接返回字符串。

###  zlib://

这个应该是一种文件上传姿势吧 

#### [zip://archive.zip\#dir/file.txt](zip://archive.zip#dir/file.txt) 



### phar://

 这种和zip伪协议差不多，用法有一点点区别一个是“\#”,一个是“/”  


#### phar://archive.zip/dir/file.txt

#### 

最后放上大佬的应用场景总结

 php://input和data://可以用来绕过一些判断语句  
data://+include可以远程命令执行  
php://filter可以用来对磁盘读写，ctf中任意文件读取用的比较多，还有就是三个白帽挑战中的绕过死亡die  
zip://和phar://主要用于配合php://filter读源码审计上传拿getshell

