---
主要工具 EWSA+pydictor
---

## 0x00 CTF

hackinglab：

 邂逅对门的妹纸分值: 150小明想要认识对门的漂亮妹纸，但又不好意思直接去敲门，但是小明知道妹纸今年\(2014年\)上大三（提交wifi密码的md5 32位小写）  
[Wifi-Crack](http://hacklist-cdn.stor.sinaapp.com/misc/wifi-crack.cap)

思路：根据信息推出生日大致的范围，用pydictor+cap包爆破密码



![](.gitbook/assets/image%20%281%29.png)

## 0x01 工具介绍

说一下pydictor

源码地址 [https://github.com/LandGrey/pydictor](https://github.com/LandGrey/pydictor)

生日在19900101到20141231时间段的八位字典生成  

`python pydictor.py -plug birthday 19900101 20141231 --len 8 8`

更多用法参考readme和[https://landgrey.github.io/pydictor/docs/doc/usage.html](https://landgrey.github.io/pydictor/docs/doc/usage.html)
