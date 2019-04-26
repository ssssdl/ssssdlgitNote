---
题目地址：（HITCON2017）题目并不是你想要的http://117.50.3.97:8004
---
 
直接点开源码，读一下
```
<?php 
    $sandbox = "sandbox/" . md5("orange" . $_SERVER["REMOTE_ADDR"]);   //定义一个路径
    @mkdir($sandbox); 
    @chdir($sandbox); 

    $data = shell_exec("GET " . escapeshellarg($_GET["url"])); 
    $info = pathinfo($_GET["filename"]); 
    $dir  = str_replace(".", "", basename($info["dirname"])); 
    @mkdir($dir); 
    @chdir($dir); 
    @file_put_contents(basename($info["basename"]), $data); 
    highlight_file(__FILE__); 
```

