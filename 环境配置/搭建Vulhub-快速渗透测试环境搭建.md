[项目地址](https://github.com/vulhub/vulhub)
[官网](https://vulhub.org/)

# 安装

```
# 安装docker，直接在官网上安装最新版的docker，也可以使用系统自带的包管理工具安装（yum、apt、rpm）
curl -s https://get.docker.com/ | sh 
# 安装pip 应该是也要先准备好python
curl -s https://bootstrap.pypa.io/get-pip.py | python
# 安装docker-compose
pip install docker-compose
# 这里可能会提示requests版本问题，直接忽略就好了，然后安装好启动的时候，有可能会遇到gssapi中没有一个什么方法直接安装以下组件就可以了
yum install python-paramiko -y
# 安装完成后查看版本,没有报错说明安装docker-compose成功
docker-compose -v
```