# ew代理工具使用

## - -help 
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
 -g connport set the connect port.#设
 -h help show the help text, By adding the -s parameter,
 you can also see the more detailed help.
 -a about show the about pages
 -v version show the version. 
 -t usectime set the milliseconds for timeout. The default 
 value is 1000 
 ......
```