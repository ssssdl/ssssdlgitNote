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

