直接引用一个大佬的分析：类似0e462097431906509019562988736854 这种形式 0e\[0-9\].\*的字符串会被PHP解析器默认解析为 numerical strings类型。

在"=="表达式进行字符串比较时，首先会判断类型，如果属于numerical strings 则先强转转换为数字。

0e462097431906509019562988736854 =0x10【462097431906509019562988736854】次方 =0

0e83040045199349405802421990339 =0x10【83040045199349405802421990339】次方 =0

这个漏洞比较的两段md5都应该是 0e\[0-9\].\*

自己写了一段脚本帮助理解：

```php
<?php
        $_str1='!1793422703!';
        $_str2='240610708';
        $_str3='QNKCDZO';
        echo '字符串一md5：'.md5($_str1)."\n";
        echo '字符串二md5：'.md5($_str2)."\n";
        echo '字符串三md5：'.md5($_str3)."\n";
        echo "------------------------------------------------\n";
        echo "强制类型转换str1的md5：";
        echo intval(md5($_str1));
        echo "\n强制类型转换str2的md5：";
        echo intval(md5($_str2));
        echo "\n强制类型转换str3的md5：";
        echo intval(md5($_str3));
        echo md5($_str1)==md5($_str2)?"\nstr1=str2\n":"\nstr1!=str2\n";
        echo md5($_str1)==md5($_str3)?"str1=str3\n":"str1!=str3\n";
        echo md5($_str2)==md5($_str3)?"str2=str3\n":"str2!=str3\n";
?>
```

执行结果如下：

![title](https://i.loli.net/2019/04/26/5cc26420c004b.png)

在附一个hackinglab的 **ctf**：

 md5真的能碰撞嘛?分值: 350md5真的能碰撞嘛?其实有时候我们不需要进行碰撞得到完全一致的MD5  
[通关地址](http://lab1.xseclab.com/pentest5_6a204bd89f3c8348afd5c77c717a097a/)

源码

```php
<?php
$flag=FLAG;
if(isset($_POST["password"])){
	$password=$_POST['password'];
    $rootadmin="!1793422703!";
    if($password==$rootadmin){die("Please do not attack admin account!");}
    
    if(md5($password)==md5($rootadmin)){
        echo $flag;
    }else{
        die("Password Error!");
    }
}
?>
```

思路：post一个240610708或者QNKCDZO