---
其实就是：auxiliary/scanner/smb/smb_ms17_010
---

# 关于msf批量扫描ms17\_010

## 0x00 使用

采用辅助模块中的ms17\_010扫描模块

```text
auxiliary/scanner/smb/smb_ms17_010
```

要配置以一下线程和要扫描的ip或者IP段

这里的IP段可以是这这样

set RHOSTS 10.123.0-128.0-254  

扫描起来不是很快但比nmap边扫边自己去找快很多

## 0x01附：msf乱码解决方案

如果是中文乱码就将命令行窗口编码改成gb 2312，或者gbk，就可以了。
