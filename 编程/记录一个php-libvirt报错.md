首先这是一个apache没有操做libvirt权限的报错，我想搭建一个lamp管理
大致代码如下
```

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