会经常出现但总被我忽视的文件

# robots.txt

## 0x00简介 

robots.txt（统一小写）是一种存放于[网站](https://zh.wikipedia.org/wiki/%E7%BD%91%E7%AB%99)根目录下的[ASCII](https://zh.wikipedia.org/wiki/ASCII)编码的[文本文件](https://zh.wikipedia.org/wiki/%E6%96%87%E6%9C%AC%E6%96%87%E4%BB%B6)，它通常告诉网络[搜索引擎](https://zh.wikipedia.org/wiki/%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E)的漫游器（又称[网络蜘蛛](https://zh.wikipedia.org/wiki/%E7%BD%91%E7%BB%9C%E8%9C%98%E8%9B%9B)），此网站中的哪些内容是不应被搜索引擎的漫游器获取的，哪些是可以被漫游器获取的。

## 0x01用法



禁止所有机器人访问特定目录：

```text
User-agent: *
Disallow: /cgi-bin/
Disallow: /images/
Disallow: /tmp/
Disallow: /private/
```



禁止所有机器人访问特定文件类型：

```text
User-agent: *
Disallow: /*.php$
Disallow: /*.js$
Disallow: /*.inc$
Disallow: /*.css$
```



`Sitemap`指令被几大搜索引擎支持（包括Google、Yahoo、Bing和Ask），指定了网站[Sitemaps](https://zh.wikipedia.org/w/index.php?title=Sitemaps&action=edit&redlink=1)文件的位置。Sitemaps文件包含了网站页面所在的URL的一个列表。`Sitemap`指令并不受`User-agent`指令的限制，所以它可以放在robots.txt文件中的任意位置。[\[3\]](https://zh.wikipedia.org/wiki/Robots.txt#cite_note-3) 唯一要注意的就是要使用网站地图指令，&lt;sitemap\_location&gt;,并将URL的"location"值换成网站地图的地址，例如，下面就是一个网站地图指令的例子：

```text
Sitemap: <http://www.example.com/sitemap.xml>
```



几大抓取工具支持`Crawl-delay`参数，设置为多少秒，以等待同服务器之间连续请求：[\[4\]](https://zh.wikipedia.org/wiki/Robots.txt#cite_note-4)[\[5\]](https://zh.wikipedia.org/wiki/Robots.txt#cite_note-5)

```text
User-agent: *
Crawl-delay: 10
```