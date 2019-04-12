## 真是已经成了老年人什么都想记一下
```
修改sshd_config文件，命令为：
vi /etc/ssh/sshd_config

将#PasswordAuthentication no的注释去掉，并且将NO修改为YES //kali中默认是yes



将PermitRootLogin without-password修改为PermitRootLogin yes



然后保存退出vi编辑器。



二、启动SSH服务

命令为：/etc/init.d/ssh start 

或者service ssh start


/etc/init.d/ssh status
或者
service ssh status


update-rc.d ssh enable  //系统自动启动SSH服务

update-rc.d ssh disabled // 关闭系统自动启动SSH服务


#ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
```