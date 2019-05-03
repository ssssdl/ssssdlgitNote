---
以前就听说过这个工具，是一个后渗透神器
---
# 简单介绍一个生成木马建立监听的过程
首先安装,我在用的是2.5版本的，大概在2.0之后的版本和之前的命令就不太一样了，现在网上很多教程都是1.x的就导致我学习的时候走了很多弯路
```
# kali clone项目到本地
git clone https://github.com/EmpireProject/Empire.git
# 安装
sudo ./Empire/setup/indtall.sh
# 可能要输入一个什么密码，直接回车就可以，全部执行完成后就算完成了，需要的话还可以把./Empire/empire添加到系统命令中
```

# 介绍 empire主要分为三个部分，
- module是一些payload，可以通过这个模块生成各种需要的后门之类的
- listeners监听器，几种类型
- agents管理一些反弹到的shell或者是代理之类的

# 简单使用过程
## 配置监听
```
# 查看帮助信息
help

# 进入监听模式，并查看当前的监听
listeners
# 此时提示符会加上listener字样 第一次会提示没有设置监听

# 配置一个监听
uselistener http 	# 使用http监听，还有很多其他形式的，
info			# 查看需要配置的信息，类似show options
set 			# set命令用来配置参数，格式set 选项 值,注意配置完成后并没有立即生效
execute			# 使set命令生效
back			# 退出监听配置

# 再次查看监听
listeners
# 配置监听要注意任何两个监听的Name和Port不能相同
kill +Name		# 删除监听
```
## 生成木马
注意要先建立监听才能继续生成木马，因为木马需要反弹shell的地址和端口
```
# 可以使用launcher和usestager两种方法生成木马吧launcher
```