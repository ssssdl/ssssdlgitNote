# 关于题目
> 去年暑假做的题，今天又翻了出来，给了一个导出来的虚拟机，然后找这个虚拟机上面的后门。一开始自己各种尝试，最后经过大佬的提醒发现在apache加载的so模块中。是一个so的后门，然后直接strings了一下这个模块，直接就出flag了

# 说一些做这个题的时候用的一些思路和方法

## 启动项查询
`chkconfig`这个命令可以查询启动项，也可以配置一些启动项，
```
例子：
关闭防火墙自启 各启动级别全部关闭
chkconfig iptables  off 
关闭防火墙level 3级别的启动
chkconfig iptables --level 3 off
```
查看一些配置文件启动项 `/etc/init.d`下的一些
还有`/etc/rc.local /etc/rc.sysinit /etc/inittab /etc/profile`等一些文件都又类似配置自启动的功能