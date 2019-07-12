感觉以前也没怎么在意，然后技术摸底就被问起来了，仔细探究一下发现一些大型漏洞里面也都能发现XXE的影子，从XML注入到任意文件读取，到代码或者命令执行，

## 简介
XXE全称是——XML External Entity,也就是XML外部实体注入攻击.漏洞是在对不安全的外部实体数据进行处理时引发的安全问题。一个比较经典的例子 
![注入一个XML节点，使添加了一个管理员账户](https://i.loli.net/2019/07/12/5d280db66707453328.png)

## 基础知识
- XML是一种标记语言，被无数软件项目采用


相关连接
https://xz.aliyun.com/t/3357
https://www.jianshu.com/p/77f2181587a4
https://blog.csdn.net/xiaoyi52/article/details/82660471
https://www.freebuf.com/articles/web/177979.html
