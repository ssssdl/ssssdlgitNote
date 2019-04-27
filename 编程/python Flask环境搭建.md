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
		> 三个包安装方法相同，先解压，然后切换到解压出的目录中，先打开虚p3vir拟环境，`F:\flask\p3vir\Scripts\activate.bat` 这时命令标头会出现(p3vir)字样,然后执行 `python setup.py install`就会把当前模块安装到虚拟环境p3vir中