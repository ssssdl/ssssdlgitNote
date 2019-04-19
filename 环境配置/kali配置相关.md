# 安装vmware-tool
```
# 查看光盘驱动器的名字
ls -l /dev | grep cdrom
# 如果光盘驱动器名字为cdrom，挂载命令如下
mount /dev/cdrom /mnt/
# 拷贝安装文件到根目录
cp VMwareTools-10.3.2-9925305.tar.gz ~
# 解压
tar -zxvf VMwareTools-10.3.2-9925305.tar.gz 
# 切换目录
cd vmware-tools-distrib/
# 安装
./vmware-install.pl 
# 然后根据需要配置（下一步）就行了
```

# 更新
```
# 换源
vi /etc/apt/sources.list


apt-get update
apt-get dist-upgrade
# 
```