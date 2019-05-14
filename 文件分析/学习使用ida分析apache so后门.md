# 基础知识

以sub开头加地址的是没有函数表也不是debug的

# 先分析一个简单的
地址：[Apache-HTTP-Server-Module-Backdoor](https://github.com/WangYihang/Apache-HTTP-Server-Module-Backdoor)

> 这个是使用了一个popen的函数执行了系统命令，配合源码，感觉比较容易看懂

![title](https://i.loli.net/2019/05/14/5cda26a7ac7a028583.png)


# 再来分析这个

首先，根据上面的思路，发现没有白底加粗的自定义的函数，