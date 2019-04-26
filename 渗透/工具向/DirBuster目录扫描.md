# kali目录扫描工具DirBuster

## 0X001 安装路径

有这个方便找字典其实不难记

kali很多工具都安装在/usr/share/路径下

DirBuster就安装在/usr/share/dirbuster/

自带的字典在/usr/share/dirbuster/wordlists

启动直接在所有程序里面搜Dir就可以找到了（感觉这个还是比较好记，就是目录的意思）

## 0X002 配置

 Options—Advanced Options

这里面可以配置一些不扫描的文件，还有代理，设置遇到表单自动登录，增加HTTP头（Cookie……），以及超时链接设置，默认线程，字典，扩展名设置，有待慢慢学习

## 0X003 扫描

一图说明所有问题

注意：7的地方按照图中配置应该是 /DedeCms5.7/{dir}，这个系统会自动生成，只要配置好{dir}就可以了
![title](https://i.loli.net/2019/04/26/5cc2a53b30bab.png)