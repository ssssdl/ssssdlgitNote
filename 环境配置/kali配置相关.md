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
# 常见国内更新源
     #中科大
     deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
     deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
 
     #阿里云（常用）
     deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
     deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
 
     #清华大学
     deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
     deb-src https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
 
     #浙大
     deb http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free
     deb-src http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free
 
     #东软大学（没有release文件）
     deb http://mirrors.neusoft.edu.cn/kali kali-rolling/main non-free contrib
     deb-src http://mirrors.neusoft.edu.cn/kali kali-rolling/main non-free contrib

# 安装更新（有可能需要--fix-missing参数）
apt-get clean //清除缓存索引
apt-get update //更新索引文件
apt-get dist-upgrade //更新
# 其他命令
apt-get upgrade -y //更新实际的软件包文件
apt-get dist-upgrade -y //根据依赖关系更新
apt-get install kali-linux-all //安装所有kali工具包
```

