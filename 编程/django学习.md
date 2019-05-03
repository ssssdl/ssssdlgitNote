---
照着官方文档敲就完了
---

# 环境搭建
安装
```
pip install django
```
建立工程
```
django-admin startproject 工程名
```
目录结构：
```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```
文件用途：
- 最外层的:file: mysite/ 根目录只是你项目的容器， Django 不关心它的名字，你可以将它重命名为任何你喜欢的名字。
- manage.py: 一个让你用各种方式管理 Django 项目的命令行工具。你可以阅读 [django-admin and manage.py](https://docs.djangoproject.com/zh-hans/2.2/ref/django-admin/) 获取所有 manage.py 的细节。
- 里面一层的 mysite/ 目录包含你的项目，它是一个纯 Python 包。它的名字就是当你引用它内部任何东西时需要用到的 Python 包名。 (比如 mysite.urls).
- mysite/__init__.py：一个空文件，告诉 Python 这个目录应该被认为是一个 Python 包。如果你是 Python 初学者，阅读官方文档中的 [更多关于包的知识](https://docs.python.org/3/tutorial/modules.html#tut-packages)。
- mysite/settings.py：Django 项目的配置文件。如果你想知道这个文件是如何工作的，请查看 [Django settings](https://docs.djangoproject.com/zh-hans/2.2/topics/settings/) 了解细节。**注意SECRET_KEY默认就在这个目录中**
- mysite/urls.py：Django 项目的 URL 声明，就像你网站的“目录”。阅读 [URL调度器](https://docs.djangoproject.com/zh-hans/2.2/topics/http/urls/) 文档来获取更多关于 URL 的内容。
- mysite/wsgi.py：作为你的项目的运行在 WSGI 兼容的Web服务器上的入口。阅读 [如何使用 WSGI 进行部署](https://docs.djangoproject.com/zh-hans/2.2/howto/deployment/wsgi/) 了解更多细节。

# 运行工程
```
# 默认端口是8000
# 在8080端口上运行
python manage.py runserver 8080
# 还可以指定ip 比如0(0.0.0.0)
python manage.py runserver 0:8080

# 除此之外manage.py还有很多选项，比如添加管理用户啊等等
python manage.py migrate  #加载一些相关的模块吧
python manage.py createsuperuser   #创建用户

默认管理路径是/admin
```

# 创建一个应用
```
# 在manage.py路径
python manage.py startapp polls  //创建一个叫polls的web应用

# 此时在当前文件夹下创建了一个新的polls文件夹目录结构如下
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```
# 简单开发polls应用
编写视图打开polls/view.py
```
# 添加代码如下
from django.http improt HttpResponse

def index(request):
	return HttpResponse("hello,world.You're at the polls index.")
```
接下来写一个url映射到这个视图
在polls目录里面创建一个urls.py的文件
```
# 创建完成后目录结构如下
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
# 输入代码如下
from django.urls import path
from . import views
urlpatterns = [
    path('', views.index, name='index'),
]
```
这样就创建了一个polls.urls的模块，接下来到