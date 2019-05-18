首先这是一个apache没有操做libvirt权限的报错，我想搭建一个使用web环境管理kvm虚拟机的服务，语言用的是php，本地执行没有问题，但是web访问执行就会报错
大致代码如下
```

<?php
    $logfile = 'test.log';

    unlink($logfile);
    if(!libvirt_logfile_set($logfile))
        die('Cannot set the log file!!');
        
    $conn = libvirt_connect('qemu:///system',false);
    # 获取虚拟机名称列表
    $doms = libvirt_list_domains($conn);
    print_r($doms);
    $ids = libvirt_list_active_domain_ids($conn);
    print_r($ids);
    
    $fp = fopen($logfile,'r');
    $str = fread($fd,filesize($logfile));
    fclose($fp);

    echo $str;
?>

```

报错如下

```
[Sat May 18 16:11:53.486282 2019] [:error] [pid 5218] [client 192.168.72.1:20782] PHP Warning:  libvirt_connect(): authentication unavailable: no polkit agent available to authenticate action 'org.libvirt.unix.manage' in /var/www/html/libvirt/list.php on line 8
[2019-05-18 16:11:53 libvirt-php/core ]: libvirt_connect: Cannot establish connection to qemu:///system

[Sat May 18 16:11:53.486788 2019] [:error] [pid 5218] [client 192.168.72.1:20782] PHP Warning:  libvirt_list_domains() expects parameter 1 to be resource, boolean given in /var/www/html/libvirt/list.php on line 10
[Sat May 18 16:11:53.486857 2019] [:error] [pid 5218] [client 192.168.72.1:20782] PHP Warning:  libvirt_list_domains(): Invalid arguments in /var/www/html/libvirt/list.php on line 10
[Sat May 18 16:11:53.486914 2019] [:error] [pid 5218] [client 192.168.72.1:20782] PHP Warning:  libvirt_list_active_domain_ids() expects parameter 1 to be resource, boolean given in /var/www/html/libvirt/list.php on line 12
[Sat May 18 16:11:53.486921 2019] [:error] [pid 5218] [client 192.168.72.1:20782] PHP Warning:  libvirt_list_active_domain_ids(): Invalid arguments in /var/www/html/libvirt/list.php on line 12
[Sat May 18 16:11:53.486963 2019] [:error] [pid 5218] [client 192.168.72.1:20782] PHP Notice:  Undefined variable: fd in /var/www/html/libvirt/list.php on line 16
[Sat May 18 16:11:53.486977 2019] [:error] [pid 5218] [client 192.168.72.1:20782] PHP Warning:  fread() expects parameter 1 to be resource, null given in /var/www/html/libvirt/list.php on line 16
/var/www/html/libvirt# cat test.log
[Sat May 18 16:12:39.424423 2019] [:error] [pid 5216] [client 192.168.72.1:20830] PHP Warning:  libvirt_connect(): authentication unavailable: no polkit agent available to authenticate action 'org.libvirt.unix.manage' in /var/www/html/libvirt/list.php on line 8
[2019-05-18 16:12:39 libvirt-php/core ]: libvirt_connect: Cannot establish connection to qemu:///system

[Sat May 18 16:12:39.424482 2019] [:error] [pid 5216] [client 192.168.72.1:20830] PHP Warning:  libvirt_list_domains() expects parameter 1 to be resource, boolean given in /var/www/html/libvirt/list.php on line 10
[Sat May 18 16:12:39.424487 2019] [:error] [pid 5216] [client 192.168.72.1:20830] PHP Warning:  libvirt_list_domains(): Invalid arguments in /var/www/html/libvirt/list.php on line 10
[Sat May 18 16:12:39.424493 2019] [:error] [pid 5216] [client 192.168.72.1:20830] PHP Warning:  libvirt_list_active_domain_ids() expects parameter 1 to be resource, boolean given in /var/www/html/libvirt/list.php on line 12
[Sat May 18 16:12:39.424497 2019] [:error] [pid 5216] [client 192.168.72.1:20830] PHP Warning:  libvirt_list_active_domain_ids(): Invalid arguments in /var/www/html/libvirt/list.php on line 12
[Sat May 18 16:12:39.424522 2019] [:error] [pid 5216] [client 192.168.72.1:20830] PHP Notice:  Undefined variable: fd in /var/www/html/libvirt/list.php on line 16
[Sat May 18 16:12:39.424529 2019] [:error] [pid 5216] [client 192.168.72.1:20830] PHP Warning:  fread() expects parameter 1 to be resource, null given in /var/www/html/libvirt/list.php on line 16

```

解决办法，
- 创建一个用于管理的用户组`groupadd libvirt`
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
# 相关连接
- [CentOS 7 virt-manager “authentication failed”错误及解决方法](https://blog.xiaoben.li/p/497)
- 