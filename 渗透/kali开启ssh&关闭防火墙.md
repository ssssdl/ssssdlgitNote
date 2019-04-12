## 真是已经成了老年人什么都想记一下
# 打开ssh
```
修改sshd_config文件，命令为：
vi /etc/ssh/sshd_config

#PasswordAuthentication no的注释去掉，并且将NO修改为YES //kali中默认是yes
PermitRootLogin without-password修改为PermitRootLogin yes

开启服务
/etc/init.d/ssh start 
或者service ssh start

查看服务状态
/etc/init.d/ssh status
或者service ssh status


update-rc.d ssh enable  //系统自动启动SSH服务

update-rc.d ssh disabled // 关闭系统自动启动SSH服务


#ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
```

# 关闭防火墙
```
kali关闭iptables
安装
apt-get install ufw
关闭
ufw disable # To disable the firewall
开启
ufw enable # To enable the firewall
```