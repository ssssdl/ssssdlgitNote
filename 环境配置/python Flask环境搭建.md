# 安装python
我的是python3.7，Windows10，安装完成后设置好环境变量，检查一下pip，easy_intall是否都安装好了
```
# 相关环境依赖 
pip install flask
pip install flask-script
pip install flask-bootstrap
pip install flask-wtf
pip install flask-sqlalchemy
pip install flask-login
```


# 安装python虚拟环境
** 就相当于专门安装一个python专门加载flask **
*感觉好像不安装直接把flask安装在系统的python上好像也可以，但是应该是每次启动都会加载flask，应该不太好*
- 安装virtualenv
	> 下载 [virtualenv](https://github.com/pypa/virtualenv/tree/master) 然后切换到这个目录下执行`python setup.py install`
- 创建虚拟环境
	> 找到一个目录执行`virtualenv p3vir` p3vir是生成的目录的名字，完成后会在p3vir目录中出现Include、Lib、Scripts等文件夹 

- 在虚拟环境中安装flask等模块
	- 下载地址：
		- [flask](https://github.com/mitsuhiko/flask)
		- [jinja2](https://github.com/mitsuhiko/jinja2)
		- [werkzeug](https://github.com/mitsuhiko/werkzeug)
	- 安装方法
		> 三个包安装方法相同，先解压，然后切换到解压出的目录中，先打开虚p3vir拟环境，`F:\flask\p3vir\Scripts\activate.bat` 这时命令标头会出现(p3vir)字样,然后执行 `python setup.py install`就会把当前模块安装到虚拟环境p3vir中，重复将上述三个模块都安装好就可以了

- vs code里面配置虚拟环境解析python
	> 新建一个工程文件夹。右键vs code打开，ctrl+shift+p，输入`python:select Interpreter` 然后先选上默认的python环境，这时会在目录下生成一个`.vscode/settings.json`的文件，编辑这个文件将python解析器换成p3vir/Scripts目录下的python.exe,就完事了# 安装python
我的是python3.7，Windows10，安装完成后设置好环境变量，检查一下pip，easy_intall是否都安装好了
```
# 相关环境依赖 
pip install flask
pip install flask-script
pip install flask-bootstrap
pip install flask-wtf
pip install flask-sqlalchemy
pip install flask-login
```


# 安装python虚拟环境
** 就相当于专门安装一个python专门加载flask **
*感觉好像不安装直接把flask安装在系统的python上好像也可以，但是应该是每次启动都会加载flask，应该不太好*
- 安装virtualenv
	> 下载 [virtualenv](https://github.com/pypa/virtualenv/tree/master) 然后切换到这个目录下执行`python setup.py install`
- 创建虚拟环境
	> 找到一个目录执行`virtualenv p3vir` p3vir是生成的目录的名字，完成后会在p3vir目录中出现Include、Lib、Scripts等文件夹 

- 在虚拟环境中安装flask等模块
	- 下载地址：
		- [flask](https://github.com/mitsuhiko/flask)
		- [jinja2](https://github.com/mitsuhiko/jinja2)
		- [werkzeug](https://github.com/mitsuhiko/werkzeug)
	- 安装方法
		> 三个包安装方法相同，先解压，然后切换到解压出的目录中，先打开虚p3vir拟环境，`F:\flask\p3vir\Scripts\activate.bat` 这时命令标头会出现(p3vir)字样,然后执行 `python setup.py install`就会把当前模块安装到虚拟环境p3vir中，重复将上述三个模块都安装好就可以了

- vs code里面配置虚拟环境解析python
	> 新建一个工程文件夹。右键vs code打开，ctrl+shift+p，输入`python:select Interpreter` 然后先选上默认的python环境，这时会在目录下生成一个`.vscode/settings.json`的文件，编辑这个文件将python解析器换成p3vir/Scripts目录下的python.exe,就完事了

# 测试
在配置好setting.json的工程文件夹中新建一个hello.py 内容如下
```
from flask import Flask  
app = Flask(__name__)  
 
@app.route("/")  
def hello():  
    return "Hello World!"  
  
if __name__ == "__main__":  
    app.run() 
```
右键运行，然后访问提示的url就可以看到hello world了


# 相关文档
- vscode 官方文档[Flask Tutorial in Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-flask)
- [Python Flask 开发环境搭建(Windows)](https://blog.csdn.net/Zhao_6666/article/details/78990002)