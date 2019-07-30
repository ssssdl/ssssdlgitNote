
centos7 安装maven
进入指定目录

cd /usr/local/src/
 
下载maven 包
wget http://mirrors.hust.edu.cn/apache/maven/maven-3/3.1.1/binaries/apache-maven-3.1.1-bin.tar.gz
 
解压改名
tar zxf apache-maven-3.1.1-bin.tar.gz mv apache-maven-3.1.1 /usr/local/maven3
 
vi /etc/profile然后还需要 配置环境变量。
#在适当的位置添加
export M2_HOME=/usr/local/maven3
export PATH=$PATH:$JAVA_HOME/bin:$M2_HOME/bin
 
保存退出后运行下面的命令使配置生效，或者重启服务器生效。
source /etc/profile
 
验证版本
mvn -v
出现maven版本即成功


摘自[Mr.chengJQ](https://www.cnblogs.com/qiyuan880794/p/9407342.html)