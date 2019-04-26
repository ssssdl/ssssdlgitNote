# strcmp返回值不确定

就是如果srtcmp如果比较的两个值如果不是字符串就会产生不确定的返回值，

附上大佬的实验代码：

![title](https://i.loli.net/2019/04/26/5cc28a81a0c15.png)

这两个漏洞参考：[https://www.waitalone.cn/php-sec-bugs.html](https://www.waitalone.cn/php-sec-bugs.html)

再加一个关于这两个的bugku的题：[http://118.89.219.210:49162/](http://118.89.219.210:49162/?v1=240610708&v2=QNKCDZO&v3[]=)

payload（不止一种）：[http://118.89.219.210:49162/?v1=240610708&v2=QNKCDZO&v3\[\]=](http://118.89.219.210:49162/?v1=240610708&v2=QNKCDZO&v3[]=)