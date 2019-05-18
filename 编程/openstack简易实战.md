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

# 修改/etc/libvirt/libvirtd.conf文件，添加或修改
unix_sock_group = "libvirt"

# 创建文件/etc/polkit-1/localauthority/50-local.d/50-org.libvirtd-group-access.pkla 内容如下

[libvirtd group Management Access]
Identity=unix-group:libvirt
Action=org.libvirt.unix.manage
ResultAny=yes
ResultInactive=yes
ResultActive=yes

# 将需要的用户（apache）添加到这个用户组里面
usermod -a -G libvirtd apache

# 重启服务
service libvirtd restart

# 修改/etc/libvirt/qemu.conf
vnc_listen="0.0.0.0"
``` 
差不多就是这些，各种权限问题一定要注意

# 0x02 简易代码
```
	<?php  
	    $logfile = 'test.log';  
	  
	    unlink($logfile);  
	    if(!libvirt_logfile_set($logfile))  
	        die('Cannot set the log file!!');  
	      
	    # 建立连接      
	    $conn = libvirt_connect('qemu:///system',false);  
	      
	    # 获取虚拟机名称列表  
	    $doms = libvirt_list_domains($conn);  
	    echo '获取节点名称：<br>';  
	    foreach($doms as $domname){  
	        echo $domname."<br>";  
	    }  
	  
	    # 获取节点id  
	    echo '获取节点id：<br>';  
	    $ids = libvirt_list_active_domain_ids($conn);  
	    foreach($ids as $id){  
	        echo $id.'<br>';  
	    }  
	  
	    # 关闭节点,get传要关闭的name,部分虚拟机可能因为没有acpid而不起作用  
	    if(isset($_GET['sdname'])){  
	        # echo 'GET sdid';  
	        $id = libvirt_domain_lookup_by_name($conn,strval($_GET['sdname']));  
	        libvirt_domain_shutdown($id);  
	        # 需要延时获取，虚拟机关闭需要时间  
	        #if(!libvirt_domain_is_active($id))  
	        #    echo $_GET['sdname'].'关闭成功';  
	    }  
	      
	    #强制关闭节点  
	    if(isset($_GET['dsname'])){  
	        $id = libvirt_domain_lookup_by_name($conn,strval($_GET['dsname']));  
	        libvirt_domain_destroy($id);  
	        if(!libvirt_domain_is_active($id))  
	            echo $_GET['dsname']."关闭成功";  
	    }  
	    #还有很多类似的操做，可以参照https://libvirt.org/php/api-reference.html  
	    # ......  
	  
	    # 关于在浏览器打开vnc界面  
	    if(isset($_GET['vncid'])){  
	        # 可以在xml定义域的时候获取vnc端口，这里假设已经获取到了  
	        $VncPort = 5900;  
	        # 这里其实涉及到获取kvm的ip  
	        exec('./noVNC/utils/launch.sh --vnc 127.0.0.1:'.$VncPort);  
	        # 需要给novnc一个启动的时间，不然会报错  
	        sleep(10);  
	        # 这里要获取kvm和本地的ip  
	        header('Location: http://192.168.72.136:6080/vnc.html?host=192.168.72.136&port=6080');  
	    }  
	    $fp = fopen($logfile,'r');  
	    $str = fread($fd,filesize($logfile));  
	    fclose($fp);  
	  
	    echo $str;  
	?>  

```
# 0x03 相关连接
- [libvirt-php官方文档](https://libvirt.org/php/api-reference.html#libvirt_image_create)
- [CentOS安装noVNC，以Web方式交付VNC远程连接]()