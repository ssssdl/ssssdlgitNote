# 这个是很久之前看到的两篇博客,通过修改配置文件配网

- [CentOS 6 网络设置修改 指定IP地址 DNS 网关（实测 笔记）](http://www.cnblogs.com/vicowong/archive/2011/04/23/2025545.html)
- [](https://blog.csdn.net/wendelee/article/details/17339835)


> 这种可能比较通用一些，但是我觉得后来学的红帽系linux的nmcli和nmgui比较灵活


# 一些红帽比赛的课程培训资料

## nmcli
全称：network manager command line interface
> 有两套管理系统:网卡和配置文件,可以预先设置多套配置文件，然后将需要的配置文件直接up起来就可以了，网卡不能做太多的配置主要是做一些连接断开的操作，然后可以查看一些网卡的信息和状态
```
nmcli 可以补全
nmcli connection //针对配置文件的操作
nmcli device  //针对网卡的操作
-----------------------------------------
nmcli device connect （网卡名称） 表示链接一个网卡
nmcli device disconnect （网卡名称） 断开一个网卡的链接
nmcli device show （网卡名称） 表示查看一个网卡的硬件信息
nmcli device status 查看网卡状态 ,列出设备名称（DEVICE）、网卡类型（TYPE）、网卡状态（STATE）、当前连接网卡的配置文件（CONNECTION）

nmcli connection add 表示添加一个配置文件
nmcli connection delete 表示删除一个文件
nmcli connection modify 表示修改一个配置文件
nmcli connection show 查看配置文件
nmcli connection up 表示使配置文件生效
nmcli connection down 表示使配置文件失效
```
