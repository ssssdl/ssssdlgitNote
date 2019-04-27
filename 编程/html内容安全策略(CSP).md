# 关于CSP
> 内容安全策略   (CSP) 是一个额外的安全层，用于检测并削弱某些特定类型的攻击，包括跨站脚本 (XSS) 和数据注入攻击等。无论是数据盗取、网站内容污染还是散发恶意软件，这些攻击都是主要的手段。(摘自MDN web docs)

个人理解就是一种执行在浏览器端的页面安全策略，通过设置白名单允许页面内容来自指定的源，比如验证js脚本的来源来消除来自其他源的js脚本加载，这样就能防止xss远程加载js脚本。

# 相关配置
两种
## 直接在html页面中添加
示例：
```
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; img-src https://*; child-src 'none';">
```
## 配置服务器，使其在响应头添加
- Apache
在VirtualHost的httpd.conf文件或者.htaccess文件中加入以下代码
```
Header set Content-Security-Policy "default-src 'self';"
```
- Nginx
在 server {}对象块中添加如下代码
```
add_header Content-Security-Policy "default-src 'self';";
```
- IIS 
web.config:中添加
```
<system.webServer>

  <httpProtocol>

    <customHeaders>

      <add name="Content-Security-Policy" value="default-src 'self';" />

    </customHeaders>

  </httpProtocol>

</system.webServer>
```
这样response 头里面就会有Content-Security-Policy: 标签
```
Content-Security-Policy: script-src 'self'; object-src 'none';style-src cdn.example.org third-party.org; child-src https:
```

# 关于csp的语法（简单介绍）
这个好难，不想看，做个实验，吃完饭回来在再看

# 测试
- 配置