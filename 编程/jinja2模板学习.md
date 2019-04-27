# 准备
> 搭建flask，jinja2环境

# 首先显示简单网页
Flask默认在templates文件夹中加载模板所以在当前工程根目录建立一个templates文件夹并写入一个html目录结构和hello.py代码如下
![title](https://i.loli.net/2019/04/27/5cc3c76de5749.png)
```
from flask import Flask  
from flask import render_template
app = Flask(__name__)  
 
@app.route("/")  
def hello():  
    return render_template('index.html')
  
if __name__ == "__main__":  
    app.run() 
```

# 显示变量
创建比如user.html
```
<html>
    <head>
        <title>请问你的名字是</title> 
    </head>
    <body>
        <p>你的名字是{{name}}</p>
    </body>
</html>
```
在重新写一下hello.py
```
from flask import Flask  
from flask import render_template
app = Flask(__name__)  
 
@app.route("/")  
def hello():  
    return render_template('index.html')

@app.route("/user/<name>")
def user(name):
    return render_template('user.html',name=name)

if __name__ == "__main__":  
    app.run() 
```
在访问http://127.0.0.1:5000/user/world就会显示world，经过测试会转义尖括号，应该没有XSS
但是把{{name}}改成{{name|safe}}就会取消转义，然后经过构造还是会有xss

除了safe还有很多其他的过滤器 输出同一个变量可以通过管道符连接多个过滤器{{ name|striptags|title }}title(striptags(name))
- safe 渲染值的时候不进行zhuany
- trim 把值首尾的空格删除掉
- striptags 渲染之前把值中的所有HTML标签删除

# 控制结构
支持for if等控制结构，支持宏等很多功能，有很多验证性的东西


# 相关文档
- [官方文档](http://jinja.pocoo.org/docs/2.9/templates/#filters)
- [Jinja2 模板的学习](https://www.jianshu.com/p/8fc66b083ecd)
