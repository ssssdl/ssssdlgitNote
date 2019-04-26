# 关于linux提权

## 0X001 查看当前用户，及系统信息

### 查看用户

`$ whoami //查看用户，id 这个命令也可以`

### 查看linux内核版本\(后面主要用到这个\)

`$ cat /proc/version`

`$ uname -a`

### 查看linux系统版本

`$ lsb_release -a  # 即可列出所有版本信息,理论上适用所有linux发行版，但今天centos7不知道为啥用不了`

`$  cat /etc/*-release，这种方法只适合Redhat系的Linux：`

`$ cat /etc/issue，此命令也适用于所有的Linux发行版。`

## 0X002 提权

### 参考：

 [Linux提权的4种方式](https://www.anquanke.com/post/id/85002)

 [实战Linux下三种不同方式的提权技巧](https://www.anquanke.com/post/id/84466)

###  **利用Linux内核漏洞提权**

* 获取到低权限SHELL后我们通常做下面几件事 \(大佬这里写的挺好记下来\)

  1.检测操作系统的发行版本

  2.查看内核版本

  3.检测当前用户权限

  4.列举Suid文件

  5.查看已经安装的包，程序，运行的服务，过期版本的有可能有漏洞

切换目录到tmp下，发现这里不容易被发现，

`$ touch exploit.c`

`$ vi exploit.c`

根据刚才的系统版本到这里下载或者复制对应的exp，\(通过README.md查找对应的\)

[https://github.com/SecWiki/linux-kernel-exploits](https://github.com/SecWiki/linux-kernel-exploits)

然后编译运行就可以了，\(但是我还没成功\)
