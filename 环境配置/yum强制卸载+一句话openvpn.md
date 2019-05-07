# 记录一次用yum卸载的经历，不是关于openvpn
## 0x00 问题来源
> 前一阵子闲着没事想弄一波openvpn体验一下，然后make安了一个，因为有点事就耽搁了，没配置就放那了，后来就给忘了，过了挺久的，又想安装一个，然后忘了安装过这回事，直接yum又安了一个，还没报错，然后配置的时候发现一堆乱七八糟的东西，然后就想着删删，然后
```
[root@ssssdl ~]# yum remove openvpn
Loaded plugins: fastestmirror
Setting up Remove Process
Resolving Dependencies
--> Running transaction check
---> Package openvpn.x86_64 0:2.4.6-1.el6 will be erased
--> Finished Dependency Resolution

Dependencies Resolved

===============================================================================================================
 Package                  Arch                    Version                         Repository              Size
===============================================================================================================
Removing:
 openvpn                  x86_64                  2.4.6-1.el6                     @epel                  1.1 M

Transaction Summary
===============================================================================================================
Remove        1 Package(s)

Installed size: 1.1 M
Is this ok [y/N]: y
Downloading Packages:
Running rpm_check_debug
Running Transaction Test
Transaction Test Succeeded
Running Transaction
openvpn: unrecognized service
error reading information on service openvpn: No such file or directory
error: %preun(openvpn-2.4.6-1.el6.x86_64) scriptlet failed, exit status 1
Error in PREUN scriptlet in rpm package openvpn
openvpn-2.4.6-1.el6.x86_64 was supposed to be removed but is not!
  Verifying  : openvpn-2.4.6-1.el6.x86_64                                                                  1/1 

Failed:
  openvpn.x86_64 0:2.4.6-1.el6                                                                                 

Complete!

```
> 这。。。。卸载不了了，删了不该删的东西了。。。。。为了方便删的时候还是rm -rf 。。。。。直接安装显示Nothing to do也安装不上，用也不能用  

## 0x01 解决
> `rpm -e --noscripts openvpn` 然后再重装就好了

# 正文开始
## 0x02 关于配置openvpn实现校园网ipv6免流（惊不惊喜，意不意外）

> 这里配置了好两次感觉，一次脚本，一次正常yum安装所有组件，修改配置文件生成密钥，放到本机……

### yum安装
> 这里是使用这位大佬的[教程](https://www.radebit.com/web/article/128.html),然后值得注意的是里面有一些双引号被转义了，不要照抄照搬，然后客户端的证书文件名一定要和.open文件里面的文件名相同，配置完之后一定要记得写防火墙转发规则！！

### 脚本安装
`wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh&bash openvpn-install.sh`
> 脚本安装完只是一个简单的梯子，但是我们要的是ipv6的梯子，接下来编辑/etc/openvpn/server.conf 
```
proto udp => proto udp6
```
> 还没完.open文件也要修改，这里ip也要改成ipv6的