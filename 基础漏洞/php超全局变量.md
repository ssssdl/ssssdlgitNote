# 关于php超全局变量

## 0x01 问题来源

ctf：[http://120.24.86.145:8004/index1.php](http://120.24.86.145:8004/index1.php)

 flag In the variable !

 `<?php    
  
error_reporting(0);  
include "flag1.php";  
highlight_file(__file__);  
if(isset($_GET['args'])){  
    $args = $_GET['args'];  
    if(!preg_match("/^\w+$/",$args)){  
        die("args error!");  
    }  
    eval("var_dump($$args);");  
}  
?>`  

payload：[http://120.24.86.145:8004/index1.php?args=GLOBALS](http://120.24.86.145:8004/index1.php?args=GLOBALS)

## 0x02 原理解析

这段代码的意思是输入变量名，得变量内容，var\_dump是说这个变量可以是数组或者就是一个变量，\w是匹配所有符号（除$），加上了$就是匹配了所有符号，而且有var\_dump代码执行行不通，$GLOBALS这个超全局变量作用是引用全局作用域中的可用的全局变量，把他们放在一个数组中，刚好把flag1.php里面的变量全部打印出来。

## 0x03 扩展

php超全局变量还有很多



* $GLOBALS
* $\_SERVER   //针对web服务打印一些请求信息
* $\_REQUEST   //和get、post一样接收一些请求参数
* $\_POST
* $\_GET
* $\_FILES  //应该是一些文件操作
* $\_ENV  // 只是被动的接受服务器端的环境变量并把它们转换为数组元素，你可以尝试直接输出它。 
* $\_COOKIE  //剩下俩不说了
* $\_SESSION

