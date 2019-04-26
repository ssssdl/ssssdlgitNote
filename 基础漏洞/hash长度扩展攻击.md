### 利用环境

采用类似这种：`md5($salt.$message)`加密方式，并且已知一组明文和密文，已知$salt长度（这个是可以利用脚本穷举实现的），此时就可以利用这个漏洞绕过类似 `$hsh !== md5($salt . $message)`的判断控制$message变量内有自己想要的值

再附上一个利用这个漏洞的hackinglab ctf：

> 有签名限制的读取任意文件
>
> 我们一直认为,只要消息签名了,salt不泄露且无法猜解到,即便是算法使用公开的加密算法,那么黑客也无法篡改信息.可是真的是这样嘛?  
> [通关地址](http://lab1.xseclab.com/decrypt1_53a52adb49c55c8daa5c8ee0ff59befe/md5_le.php)  
> Tips: MD5 Length Extension Attack!  
> Tips: 除已经告知的/etc/hosts文件外,若能读取到任意系统文件即可获取Flag.  
> Info: 增加密钥长度为32位

### 核心原理

我觉得核心原理就是一句话： 明文长度大于64byte，那么散列算法会先计算第一个64byte，然后再计算第二个64byte 这样第二个64byte就可以放一些自己想要的数据

### 漏洞利用

大致思路：就是先利用已知的salt长度构造salt+message在内存中的状态（可以利用工具winhex新建一个64byte的空间），前面是salt（随便写 位数对就可以 接着是message），在第57位上改成50 ，然后删去前面的salt ，在后面加上自己想要的信息就可以了，  不手工的话还可以利用 [ hash\_extender ](http://link.zhihu.com/?target=https%3A//github.com/iagox86/hash_extender)或是 [HashPump](http://link.zhihu.com/?target=https%3A//github.com/bwall/HashPump)，还有就是提交的`$hsh`应该是你处理后的字符串的md5加密 ，还有00通常用%00代替

讲真还是推荐工具 ，一下子就解决了各种计算和加密过程

附上wp上关于这两个工具使用

```text
chen@vmware:~/桌面/hash_extender$ ./hash_extender -f md5 -l 32 -d '/etc/hosts' -s 'f3d366138601b5afefbd4fc15731692e' -a '' --out-data-format=html
Type: md5
Secret length: 32
New signature: 1b17d9594eb404c97c5090b11660ac63
New string: %2fetc%2fhosts%80%00%00%00%00%00%00%00%00%00%00%00%00%00P%01%00%00%00%00%00%00

# HashPump
╭─root@kali  ~/Desktop/HashPump  ‹master*› 
╰─$ hashpump -s f3d366138601b5afefbd4fc15731692e -d /etc/hosts -k 32 -a /etc/hosts 
75221e2e5cd1cd1c7694cbd386571ffe
/etc/hosts\x80\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00P\x01\x00\x00\x00\x00\x00\x00/etc/hosts
```

还有一个分析的挺透彻的大佬的文章

[http://www.mottoin.com/85772.html](http://www.mottoin.com/85772.html)

