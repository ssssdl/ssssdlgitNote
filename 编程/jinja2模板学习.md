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