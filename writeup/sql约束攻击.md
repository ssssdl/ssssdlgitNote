## 0x00 CTF

**skctf**：[http://118.89.219.210:49163/](http://118.89.219.210:49163/)

**解题思路**：注册用户名admin+若干空格，登录即可getflag

## 0x01 SQL约束攻击

这个攻击主要是实现的原理是：在数据库中`select * from user where username='admin';`和`select * from user where username='admin     ';`的查询结果是一样的

**参考**：[https://blog.csdn.net/qq\_32400847/article/details/54137747](https://blog.csdn.net/qq_32400847/article/details/54137747)
