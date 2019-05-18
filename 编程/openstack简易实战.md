# 0x00
这个感觉有空再搭建一个,好好总结一下，写一个博客，这一波操做太有意思了。整整一下午，一边摸索框架路子，一边写代码测试

# 0x01 环境搭建
主要侧重与kvm的安装
```
安装kvm
lsmod | grep kvm			# 查看是否已经开启了kvm模块，有内容输出代表开启了
systemctl stop firewalld.service 	# 关闭防火墙
# 关闭selinux 这两步主要是防止出一些其他的错误

# 更新yum源
yum install -y qemu-kvm qemu-img qemu-kvm-tools 				#安装qemu相关组件
yum install -y virt-install libvirt libvirt-client virt-viewer virt-manager 	#安装libvirt相关组件
yum install -y bridge-utils		#安装网络桥接组件
yum install -y tigervnc tigervnc-server #安装vnc
yum install –y gcc-c++ gcc glibc  	#安装gcc
yum install -y libvirt-devel libvirt-glib-devel	#安装libvirt相关开发库
yum install -y php-libvirt.x86_64	#安装libvirtphp相关开发库


#安装lamp相关组件

#安装 php-fpm （可能用的到）
yum install -y php-fpm

#创建软连接
ln -s /usr/libexec/qemu-kvm /usr/bin/qemu-kvm 

#设置各种服务开机自启，还有http等
systemctl enable libvirtd
systemctl start libvirtd 

#下载配置 noVnc
git clone git://github.com/kanaka/noVNC
cd ./noVNC/utils/ 
openssl req -new -x509 -days 365 -nodes -out self.pem -keyout self.pem

#将apache用户加入libvirt组
groupadd libvirt
- 配置这个用户组可以管理libvirt 
> 修改`/etc/libvirt/libvirtdconf`文件，添加或修改`unix_sock_group = "libvirt"`,创建文件`/etc/polkit-1/localauthority/50-local.d/50-org.libvirtd-group-access.pkla`
内容如下
```
[libvirtd group Management Access]
Identity=unix-group:libvirt
Action=org.libvirt.unix.manage
ResultAny=yes
ResultInactive=yes
ResultActive=yes
```
- 将需要的用户（apache）添加到这个用户组里面
```
sudo usermod -a -G libvirtd apache
```
- 重启服务
```
service libvirtd restart
```
``` 
