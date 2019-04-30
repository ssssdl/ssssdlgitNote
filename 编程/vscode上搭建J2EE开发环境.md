---
Myeclipse和IDEA破解越来越难，eclipse各种问题，感觉不如vscode直接来，没准还能多了解了解java  
---
# 准备
- windows 10
- vscode
- java环境

 
# windows环境准备
- 安装Maven
	- 下载地址[Welcome to Apache Maven](https://maven.apache.org/)
	- 安装：解压之后将bin目录添加到环境变量中即可，测试`mvn -version`命令是否回显正常
- 下载tomcat
	- 下载地址[Apache Tomcat](https://tomcat.apache.org)
	- 安装：直接解压就可以了

# vscode扩展安装
- Java Extension Pack 里面包含了基本java开发需要的五个插件Language Support for Java™ by Red Hat、 Debugger for Java、Java Test Runner、Maven Project Explorer、 Java Dependency Viewer、Visual Studio IntelliCode  
- Tomcat for Java 不太懂，因为之前用的myeclipse就是直接使用Tomcat，所以这个也是直接用这个吧，安装完成后会在资源管理器里面会多出一栏TOMCAT SERVERS，点击这一栏上的加号，选择刚才解压的tomcat目录(忘了是不是要选到bin目录了)

# 测试
- 在资源管理器工作空间里面右键->从Maven原型生成->选择
