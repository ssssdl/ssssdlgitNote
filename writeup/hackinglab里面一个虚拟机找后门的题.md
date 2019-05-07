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

## 查看端口占用情况，包括要查看防火墙规则
查看单个端口占用情况,注意这个lsof可能有的系统不自带
```
lsof -i tcp:80
```
列出所有端口占用情况
```
netstat -ntlp
```
另外还要查看防火墙的相关设置`/etc/sysconfig/iptables`和`iptables status`

## 查看用户和组的相关状态
`groups`查看当前组中的成员，如果加上用户就是查看这个用户所在组的成员。

然后检查以下文件状态是否正常
```
/etc/group
/etc/shadow
/etc/passwd
```
然后用户和组的操作的命令主要有
```
useradd		# 增加用户
usermod		# 修改用户
userdel 	# 删除用户
gpasswd 	# 将用户加入指定组中
passwd		# 修改用户密码
groupadd	# 添加组
groupmod	# 修改组
groupdel	# 删除组
```
其他相关的 
```
w或者who	# 查看当前系统用户登陆情况
whoami		# 查看当前登陆用户的用户信息
id		# 查看自己或者指定用户的组等信息
last或者lastb	# 查看登陆成功和不成功的日志信息
```

## 查看apache加载的模块
```
apachectl -l			# 查看编译时就编译在apache中的模块
apachectl -t -D DUMP_MODULES	# 查看apache加载的模块
# 这个也可以在apache的配置文件中查看
```