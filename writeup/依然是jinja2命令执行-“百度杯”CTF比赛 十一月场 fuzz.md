> 先fuzz测试拿到可以get一个name参数（这也要考，真是服了）
# 开始测试输入
```
http://0a0e53c0f2d4f4fa60e8850469e05c794f99e0dbd734ea1.changame.ichunqiu.com
```
这样输出的是hello 2，推测是jinja2命令执行，继续深度测试