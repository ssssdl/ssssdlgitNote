- 使用http用户名密码语法，就是@，形如admin@123.123.123.123
- 多级域名
- 使用十六进制0xC0.0xA8.0.1等进制转换，
- IP缩写10.0.0.1可以写成10.1（未验证）
- 利用302跳转
	> 附上简易跳转脚本
```
	
<?php
$ip = $_GET['ip'];
$port = $_GET['port'];
$scheme = $_GET['s'];
$data = $_GET['data'];
header("Location: $scheme://$ip:$port/$data");
?>
```
- url短网址，但是有一些短网址不支持使用ip生成，[tinyurl支持](https://tinyurl.com/)，
- 使用http://xip.io,可以将192.168.0.1.xip.io解析到192.168.0.1，然后可以配合url短网址绕过
- 利用其他相关协议
- DNS Rebinding
	- 先准备一个域名，绑定一个非内网的IP，设置TTL为0，这样服务器请求的时候会每次都解析一下这个域名。
	- 然后将这个域名的url提交给服务器存在ssrf的点，这时服务器会第一次进行解析域名，判断IP不是内网IP，然后通过验证。
	- 服务器通过验证后会对URL中的域名第二次解析(TTL为0)，然后这时修改掉域名指定的IP为内网IP，然后就会返回请求内网资源的结果。
	- 然后这个的一些问题：DNS缓存，有的DNS服务器即使TTL设置为0也会缓存。

- 类似的一个绕过方法，ip双重绑定绕过：
	> php获取IP的时候使用gethoostname或者dns_get_record这两个函数，如果一个域名绑定了两个IP,dns_get_record就会返回两个，gethostname就会随机返回一个，但是curl访问域名的时候会尝试访问每一个，如果外网的端口关闭，就会返回内网端口的开放情况。




# 相关连接
- [CSDN DNS Rebinding技术绕过SSRF/代理IP限制](https://blog.csdn.net/u011721501/article/details/54667714)
- [FreeBuf SSRF漏洞中绕过IP限制的几种方法总结](https://www.freebuf.com/articles/web/135342.html)
- [CSDN 【漏洞学习——SSRF】360某处ssrf漏洞可探测内网信息](https://blog.csdn.net/Fly_hps/article/details/84392954)