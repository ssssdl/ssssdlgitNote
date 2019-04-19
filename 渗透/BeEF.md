# BeEF相关配置（包含关联msf）
修改 /usr/share/beef-xss/config.yaml
```
metasploit:
	enable: ture
```
修改 /usr/share/beef-xss/extensions/demos/config.yaml
```
enable true
```
修改 /usr/share/beef-xss/extensions/metasploit/config.yaml
```
	ssl: true
	ssl_version: 'TLSv1'
```
修改默认管理员密码 /etc/beef-xss/config.yaml 默认beef beef

# 启动beef+msf
启动相关服务
```
service postgresql start
service metasploit start
```
启动msf,
```
msfconsole
load msgrpc ServerHost=127.0.0.1 User=beef Pass=beef SSL=y
```
