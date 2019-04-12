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



查看SSH服务状态是否正常运行，命令为：

/etc/init.d/ssh status

或者

service ssh status



注明：这两种启动ssh方式都是临时性的，如果机器重启就需要重新输入上面命令才可以开启ssh，如果需要ssh服务下次开机自动启动，则需要使用以下命令启动ssh服务，命令为：

update-rc.d ssh enable  //系统自动启动SSH服务

update-rc.d ssh disabled // 关闭系统自动启动SSH服务



三、如果以上两个步骤都操作完了还是登陆不了kali linux的ssh，则需要生成两个秘钥

那么要先生成两个密钥：

#ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key

#ssh-keygen -t dsa -f /etc/ssh/ssh_host_rsa_key
```