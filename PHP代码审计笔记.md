## PHP代码审计笔记

### 环境搭建

- IDEA
  - phpstorm
  - sublime
- 集成环境整合包
  - wampserver
- 数据库连接
  - nevicat

配置域名欺骗： 

```
C:\Windows\System32\drivers\etc\host
```

修改`host`文件，加上`127.0.0.1 www.k.com`类似的

配置`apache`虚拟机，使得可以通过一个`IP`访问不同文件

```
NameVirtualHost *:80
<VirtualHost *:80>
	ServerName localhost
	DocumentRoot "D:/wamp/www"
	<Directory "D:/wamp/www/">
		Order Deny,Allow
		Allow from all
	</Directory>
	Options Indexes FollowSymLinks
</VirtualHost>

<VirtualHost *:80>
	ServerName www.f.com
	DocumentRoot "D:/wamp/www/f.com"
	<Directory "D:/wamp/www/f.com">
		Order Deny,Allow
		Allow from all
	</Directory>
	Options Indexes FollowSymLinks
</VirtualHost>
```

 ### html基础

由于很多东西已经会了，这里直接把一些不太熟悉的写一写，唤醒一下记忆。这里直接放代码。

```html
//test1

<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>测试案例一</title>
</head>
<body>
	<h3><b>古诗</b></h3> <hr>
	<p>
		<b>冬夜读书示子章</b><br><br>
		<font>宋代：陆游</font><br><br>
		<font color="green">古人学问无遗力，</font><br>
		<font size="4" color="blue"><b>少壮功夫老始成。</b></font><br>
		<font color="orange">纸上得来终觉浅，</font><br>
		<font size="4" color="red"><b>绝知此事要躬行。</b></font><br>
	</p>
	<hr>
	<h3><b>网址导航</b></h3><hr>
	<ul>
		<li><a href="https://www.baidu.com">百度</a></li>
		<li><a href="https://youku.com/">优酷</a></li>
		<li><a href="edu.51cto.com">51cto</a></li>
	</ul>
	<hr>
	<h3><b>图片展示</b></h3><hr>
	<img src="./1.jpg" width="200px" height="150px" alt="加载失败">

</body>
</html>


//test2

<body>
	<!--
	autoplay 如果出现该属性，则音频在就绪后马上播放
	controls 如果出现该属性，则向用户展示控件，比如播放按钮
	preload 如果出现该属性，则音频在页面加载时进行加载，并预备播放 
	-->
	<audio src="./PaceMak1r Blazo - 未来还未来 (feat. Blazo).MP3" controls="controls"></audio><br>
	<video controls="controls" width="400" height="320">
		 <source src="./2.mp4" type="video/mp4" >
		 你的浏览器不支持视频播放
	</video>
	<br>

	<table border="1" width="400" cellspacing="0">
		<caption>表格标题</caption>
		<tr align="center">
			<th>1</th>
			<th>2</th>
			<th>3</th>
		</tr>

		<tr align="right">
			<td>11</td>
			<td>12</td>
			<td>13</td>
		</tr>
	</table>
</body>
</html> 


//test3

<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>测试案例3</title>
</head>
<body>
	<h2>表单案例</h2> <br>

	<form action="1.php" method="post" enctype="multipart/form-data">
	<!--如果提交中有文件，enctype要改成multipart/form-data-->

	账户： <input type="text" value="51cto" name="username" readonly > <br><br><!--加了readonly后不能改-->

	地址： <input type="text" value="上海市浦东新区xx号" name="address" size="40" disabled><br><br> <!--显示灰色disable-->

	密码： <input type="password" name="password" ><br><br>

	性别： <input type="radio"  value="1" name="sex">男
		   <input type="radio"  value="0" name="sex" checked="">女 <!--checked默认选中-->
		   <br><br>

    爱好： <input type="checkbox" name="like[]">篮球
    	   <input type="checkbox" name="like[]" checked="">足球
    	   <input type="checkbox" name="like[]">台球
    	   <input type="checkbox" name="like[]" checked="">乒乓球
    	   <br><br>

    头像： <input type="file" name="headpic"><br><br>

    日期： 
    <select name="year" disabled=""><--disable默认标签不可用-->
    	<option value="1990">1990</option>
    	<option value="1991" selected="">1991</option>
    	<option value="1992">1992</option>
    	<option value="1993">1993</option>
    	<option value="1994">1994</option>
    </select><br><br>

    城市：
    <select name="city">
    	<option value="北京">北京</option>
    	<option value="上海">上海</option>
    	<option value="广州">广州</option>
    	<option value="深圳">深圳</option>
    </select><br><br>

    简介：
    <textarea cols="30" rows="6">404player</textarea><br><br>

    <input type="submit" name="提交">
    <input type="reset" name="重置">
		
	</form>
</body>
</html>
```



### CSS基础

- 内联写法

  ```html
  <p style="border: solid 1px red; padding: 20px"></p>
  <!--写在标签内部-->
  ```

  

- 内部方式（内嵌样式）

  ```html
  <style type="text/css"
         p{
         		border: solid 1px red;
         		padding: 20px;
         }
  </style>
  <!--作用于当前整个页面-->
  <!--   selector {property : value}  -->
  
  ```

  

- 外部导入方式

  ```html
  <link href="./css.css" type="text/css" rel="stylesheet">
  ```

  ```css
  /*css.css*/
  p{
  	border: solid 1px blue;
  	padding: 30px;
  }
  ```

> CSS样式发生冲突时，优先级取优先选择



#### CSS选择器

1. `html`选择符 （标签选择器）

   > 就是把html标签作为选择符使用

   > 如 p{......} 网页中所有p标签采用此样式

   >  h2{......} 网页中所有h2标签采用此样

   

2. `class`类选择器 （使用点 . 将自定义名（类名） 来定义的选择符）

   > 定义： . 类名{......}      其他选择符名 . 类名{......}
   >
   > 使用： <html标签  class="类名"> ......  </html标签>
   >
   > .mc{color: blue; } 
   >
   > p.ps {color:green; } 
   >
   > 注意: 类选择符可以在网页中重复使用

3. `id`选择器 

   > 定义： #{id} {.....}
   >
   > id 选择符只在网页中使用一页

4. 通配符选择器（通用选择器）

   > 选择器名为 *
   >
   > 作用： 写在最前面，对所有 html 标签起作用
   >
   > 一般用于页面的初始化工作，统一字体， 边框， 内外边框

5. 关联选择符 （包含选择符）

   > 对标签下的标签进行精准控制
   >
   > 格式： 选择符1 选择符2 选择符3 ... {...}
   >
   > 例如： table a{....}    h1 p{color:red}

#### CSS属性

1. `color`颜色属性：

   1. RGB颜色： 红(R) 、 绿(G)、蓝(B) 三个颜色通道的变化

      > background-color : #CCCCCC;

   2. RGBA颜色：红(R)、 绿(G)、蓝(B)、 透明度(A)

      > background-color：rgba(0,0,0,0.5);

2.  `font`字体属性

   > font-size        字体大小   20px   60%基于父对象的百分比取值
   >
   > font-family    字体："Microsoft Yahei"
   >
   > font-style       normal 正常      italic 斜体    oblique 倾斜的字体
   >
   > font-weight   字体加粗

3. 文本属性

   > text-indent       首行缩进
   >
   > text-overflow        文本的溢出是否使用省略标记(...)    clip | ellipsis （显示省略标记）
   >
   > white-space : nowrap 强制在同一行显示所有文本
   >
   > overflow : hidden  溢出就隐藏

   >多行溢出显示省略号：
   >
   >​	display : -webkit-box;
   >
   >​    -webkit-box-orient:vertical;
   >
   >​    -webkit-line-clamp:3
   >
   >​     overflow:hidden

   > text-align:    文本位置     center right left
   >
   > text-decoration:  字体画线    none   underline   line-throught   常在a标签下使用
   >
   > text-shadow: 1px  1px  rgba(0,0,0,0.3)
   >
   > letter-spacing; 文字或字母的间距
   >
   > line-height:   行高      设置跟height一样，就垂直居中了
   >
   > color： 字体颜色
   
4. 边框

   > border-color
   >
   > border-style : solid 实线     dotted  点状线   dashed  虚线
   >
   > border-width
   >
   > border-left-color
   >
   > border-left-style
   >
   > border-left-width
   >
   > border-radius 圆角处理
   >
   > box-shadow 设置或检索对象阴影


#### 盒子模型

- border     边框
- margin     外边距
- padding    内边距

![image-20210325204840755](https://raw.githubusercontent.com/404player/pic-bed/main/typora202103/25/204841-358821.png)





- display 
  - inline   元素不会独占一行，将块元素转化为了行元素，但是转化后`width`， `height`属性无效
  - inline-block   可以控制`width`，`height`

#### CSS响应式布局

使用`media`的三种方法

1. 直接在CSS文件中使用

   ```css
   @media 类型 and （条件1） and （条件2）{
       CSS样式
   }
   
   @media screen and (max-width;1024px){
       body{
           background-color:red;
       }
   }
   ```

2. 使用`@import`导入

   ```css
   @import url("css/moxie.css") all and (max-width:980px);
   ```

3. 也是最常用的方法——直接使用`link`连接，`media`属性用于设置查询的方法

   ```css
   <link rel="stylesheet" type="text/css" href="css/moxie.css" media="all and (max-width=980px)">
   ```

   

### Bootstrap 基本用法

去官网出查中文文档，有详细介绍

https://v3.bootcss.com/css/#forms

重点： 栅格系统

栅格系统可以适用于响应式布局



### PHP语法基础

1. 注释

   ```php
   # 这是单行注释
   /*
   这是
   多行注释
   */
   ```

2. 查看`PHP`配置

   ```php
   <?
       phpinfo();
   ?>
   ```

3. 变量

   ```php
   <?
       $a = 10;
   	echo $a;
   	//php变量区分大小写
   
   	/*
   	三个常见函数：
   	isset  返回此变量是否定义 返回boolean变量
   	例： <?php $a = 1; echo isset($a); //输出1 ?>
   	
   	var_dump 详细输出这个变量的类型   type value
   	
       gettype 返回此变量的类型
   	
   	*/
   ?>
   ```

   ```php
   //数组
   
   //索引数组
   
   $a = array(1,2,"baidu.com",2.1);
   
   //关联数组
   
   ```

4. 字符串基本知识和一些常用函数

   ```php
   <?php
   	$a = "baidu";
   	$b = "taobao";
   
   	echo $a.$b; //拼接字符串
   
   	//单引号定义的字符串不会解释变量，双引号的会解释内部的变量
   	//单引号不会解释转义符，但是双引号会解释
   	$c = 'xxxx$a';
   	$d = "xxxx$a";
   
   	echo "c=".$c."<br/>";
   	echo "d=".$d."<br/>";
   
   	
   ?>
   ```

   ```php
   //常用函数
   //php前面先加一句规定使用utf-8解码   header("Content-type:text/html;charset=utf-8");
   
   - strlen  计算字符串长度
       
     计算字节长度
     $a = "北京";
     echo strlen($a); //6, 一个汉字占三个字节
       
   - mb_strlen(string, encode-type)  以特定编码计算字符串长度
       
     计算真实字符长度
     $b = "北京";
     echo mv_strlen($b,"utf-8"); //2
   
   - strpos(string $haystack,mixed $needle) 查询字符串第一次出现的位置
       
   - stripos 与strpos功能一样，但是不区分大小写
       
   - strrpos  从右边开始数，相当于最后一次出现的位置
       
   - str_replace(search , replace , subject)  将为subject 中的 search 换成 replace
       
   - str_ireplace 不区分大小写
       
   - strstr(subject, needle) 查找字符串首次出现的位置， 并返回字符串
      
     例： echo strstr("www.baidu.com", "baidu"); //输出 baidu.com
   
   - substr(string, start, length) 截取字符串，返回从start开始长度为length的字符串
       
     例：echo substr("www.51cto.com", 4, 5); //输出51cto
   
   - strrchr(haystack, needle) 返回haystack中最后一个needle开始的字符串
       
      例： echo strrchr("www.baidubaidu.com","b"); //返回baidu.com
      取文件后缀名的时候需要用到
          
   - explode(string a,string b) 以 a 为分界点分割字符串b 
     例： $arrstr = explode(".", " www.baidu.com");
   	  print_r($arrstr);
   	  //输出 Array ( [0] => www [1] => baidu [2] => com )
   
   - implode(array a, string b);   以 b 为连接符 连接  array 数组
       
   - trim 消除字符串前后的空白符  ltri  rtrim   
       
   
       
    防御XSS注入重要的两个函数
       
   - addslashes  会让 ' 变 /' 等等， 使注入变得很蛋疼
       
   - htmlspecialchars 使用html编码 替换 特殊符号
   
   ```

5. 数组

   ```php
   <?php
   	$a = "aaaa";
   	$b = 123;
   	
       // 索引数组
   	$arr = array(1,2,3,"www.baidu.com",false);
   
   	//print_r($arr);
   
       //关联数组
   	$arr1 = array(
   		"aa" => "wuhan",
   		"bb" => 10,
   		"xx" => "beijing"
   
   	);
   	var_dump($arr);
   ?>
   ```

   ```PHP
   //数组的遍历
   
   for($i = 0; $i < count($arr); $i ++)
   {
       echo $arr[$i];
       echo <br/>;
       
   }
   
   //可遍历索引和关联数组
   
   foreach($arr1 as $key => $value){
       
       echo $key."---".$value."<br/>";
   }
   
   //其他方式
   
   while($row = each($arr))
   {
       list($k,$v) = $row;
       echo $k."---".$v."<br/>";
   }
   
   ```

   ```php
   //数组的增删改查
   <?php
       $arr = array("a","vv","cc");
   
   	$arr[] = "ff"; //增
   	
   	unset($arr[1]); //删
   
   	$arr[2] = "www.baidu.com";
   
   	echo $arr[1];
   ?>
   ```

   ``` 
   //数组的几个函数
   
   array_key_exists()  判断键值是否存在
   
   	$arr1 = array(
   		"aa" => "wuhan",
   		"bb" => 10,
   		"xx" => "beijing"
   
   	);
   	$k = array_key_exists("aa",$arr1);
   
   in_array()  判断value是否在数组里
   
   
   ```

6. 函数基本用法

   ```php
   <?php
   	
   	//无参函数
   	function a(){
   
   		echo "www.baidu.com<br/>";
   		echo "www.etc.com<br/>";
   
   	}
   
   	//调用函数
   	a();
   
   
   	function b($a,$c){
   		$x = $a * $a + $c * $c;
   		return $x;
   	}
   
   	$y = b(3,4);
   	echo $y;
   
   ?>
   ```

7. 超全局变量

   ```php
   $GLOBALS (超全局数组)
       var_dump($GLOBALS);  //输出一个二维数组
   
   <?php
       $y = "www.baidu.com";
   	echo $GLOBALS[“GLOBALS”]["y"]; //与 echo $y  效果相同
   ?>
       
   $_SERVER  //输出服务器与客户端的信息
       
   <?php
   	//var_dump($_SERVER);
   
   
   	echo $_SERVER["REMOTE_ADDR"]."<br/>";
   	echo $_SERVER["HTTP_USER_AGENT"];
   	
   ?>
       
   $_GET  //接受get方法传参
       
    <?php
   
   	function getAdd($a,$b){
   		return $a+$b;
   	}
   
   	if(empty($_GET)){
   		echo "no get<br/>";
   	}else{
   		$x = $_GET["a"];
   		$y = $_GET["b"];
   
   		$c = getAdd($x,$y);
   		echo $c;
   	}
   	
   ?>
       
   $_POST //接受post方法传参  url上不会显示参数
       
   $_REQUEST   //相当于   $_GET + $POST 
       
   
   ```

   

8. 变量与作用域

   ```php
   <?php
   	
   	define("PI", 3.14);//定义常量， 不能进行修改
   
   	echo PI;
   
   	var_dump(defined("PI"));//查看PI是否被定义
   
   	//超全局变量  看上面
   	//全局变量
   
   	$a = 10;
   	echo $a;
   	$str = "www.baidu.com";
   	echo $str;  //以上全是全局变量
   
   	function a($x){
           $x = $x +  10; //$x 就是局部变量
       }
   	$x = 100; //这里的x是全局变量
    
       //跟	C 语言一样， 可以通过 & 来传递地址
   
   
   	//传址
   	function b(&$a){
           $a = $a + 10; //传入参数在外部也会改变
       }
   	$b = 100;
   	b($b);
   	echo $b; //110
   
   	function c($a){
           $a = $a * 10;
           global $xx; //在函数中定义全局变量
           $xx ++;
       }
   	
   	$xx  = 10;
   	c($a);
   	echo $xx; // 输出11
   
   
   ?>
   ```


### PHP会话技术详解

- ```php
  # $_COOKIE
  
  # cookie设置
  # test.php
  <?php
  	setcookie("aaa","www.xx.com");
  ?>
  # test1.php
  <?php
      var_dump($_COOKIE);
  ?>
  # 先执行test.php 设置 cookies 再执行 test1.php 查看cookie
      
  #通过时间戳设置cookie过期时间
      
  <?php
      setcookie("ooo","123456",time()-60);//销毁cookie
  	setcookie("aaa","123456"); //没有过期时间
  	setcookie("bbb","123456",time()+60); //一分钟过期
  	setcookie("ccc","123456",time()+60*10); //十分钟过期
  	setcookie("ddd","123456",time()+60*60); //一小时过期
  	setcookie("eee","123456",time()+60*60*24*30); //30天过期
  ?>
  
  # 第一次设置cookie时无法正常取出
  
  <?php
      setcookie("num",20);
  	$num = $_COOKIE["num"];
  	echo $num;
  ?>  # 会报错
  ```

- 利用`cookie`实现计数器页面

  ```php
  # cookie 有延迟特性，即当次设置 只有等到下一次访问 cookie才会更新
  
  <?php
  	
  	$num = 0;
  	if(empty($_COOKIE["access"]))
  	{
  		setcookie("access",1);
  		// $num = $_COOKIE["access"]; 第一次无法取出，所以会报错
  		// echo $_COOKIE["access"];
  		echo "1";
  	}else{
  		$num = $_COOKIE["access"];
  		$num ++;
  		setcookie("access",$num);
  		// echo  $COOKIE["access"]; 由于cookie的延迟特性，设置完要下一次访问才能更新，所以这样写会导致每一次计数都少一次
  		echo $num;
  	}
  ```

- `$_SESSION`

  ```php
  //用session之前一定要  session_start();
  
  <?php
  	//cookie 保存在浏览器端的
  	//session 是保存在server端的
  	// session 要依靠 cookie 才能实现
  
  	
  	session_start();
  	$_SESSION["website"] = "www.baidu.com";
  
  ?>
      
  <?php
  	session_start();
  	var_dump($_SESSION);
  ?>
      
  //两个文件都要 session_start(); 最终才能输出
      
  // session 清空
  // 两种方法： 1. seesion_destroy()     2. 将$_SESSION 设置为空数组
  
   <?php
  	session_start();
  	//session_destroy();
  	$_SESSION = array();
  
  ?>
  ```

  

- 编写一个`session `登录页面

  ```php
  //login.html
  
  <!DOCTYPE html>
  <html>
  <head>
  	<meta charset="utf-8">
  	<title>session登录测试</title>
  </head>
  <body>
  
  	<form action="login.php" method="post">
  		用户名： <input type="text" name="name2" />
  		密码： <input type="password" name="pass2" />
  		<input type="submit" value="登录" />
  	</form>
  </body>
  </html>
      
  //login.php
      
  <?php
  	header("Content-type:text/html;charset=utf-8");
  	session_start();
  
  	if(empty($_POST))
  	{
  		echo "无法登录<br/>";
  		echo "<a href='login.html'>请登录</a>";
  
  	}else{
  		$name = $_POST["name2"];
  		$pass = $_POST["pass2"];
  
  		if($name == "admin" && $pass == "123" ){
  			$_SESSION["name"] = "gy";
  			header("Location:test.php");
  		}else{
  			echo "用户名或密码错误<br/>";
  			echo "<a href='login.html'>请登录</a>";
  		}
  	}
  
  
  // test.php
  
  <?php
  	header("Content-type:text/html;charset=utf-8");
  	session_start();
  	if(empty($_SESSION)){
  		echo "无权登录<br/>";
  		echo "<a href='login.html'>请登录</a>";
  	}else{
  		$name = $_SESSION["name"];
  		echo "欢迎".$name."登录系统";
  	}
  ?>
  
  
  ```

### PHP 文件操作详解

1. 文件包含

   ```php
   //require
   //include
   
   //t2.php  测试文件
   
   <?php
   	$conn = array(
   		"host"=>"localhost",
   		"mysql"=>"localhost",
   		"port"=>'3306',
   		"database"=>"testdb"
   	);
   
   ?>
       
   // t3.php
       
   <?php
       echo "www.x.com<hr/>"
       
       require "t7.php"; //没有这个文件，报错立即终止执行
   	// include "t7.php"; //没有这个文件，报错继续执行
   	$host = $conn["host"];
   	echo $host;
   
   	echo "xxxxxxx<br/>";
   ?>
       
       
   //require_once
   //include_once
       
   <?php
   	
   	echo "www.x.com<hr/>";
   	require_once "t2.php";
   
   	echo "xxxxxx<hr/>";
   	echo "xxxxxx<br/>";
   	require_once "t2.php";
   	echo "xxxxxx<br/>";
   	echo "xxxxxx<br/>";
   	echo "xxxxxx<br/>";
   	echo "xxxxxx<br/>";
   	echo "xxxxxx<br/>";
   	require_once "t2.php";
   	echo "xxxxxx<br/>";
   	echo "xxxxxx<br/>";
   
   ?> //require_once 只会执行一次
       
   //include_once 也是一样，只是报错方式有所不同
       吗你
   ```

2. 文件上传

   ```php
   //$_FILES
   
   // t2.php 上传表单
   
   <!DOCTYPE html>
   <html>
   <head>
   	<meta charset="utf-8">
   	<title>测试文件上传</title>
   </head>
   <body>
   	<form action="t3.php" method="post" enctype="multipart/form-data">
   		<input type="file" name="pic" />
   		<input type="submit" />
   	</form>
   </body>
   </html>
       
       
   // t3.php
       
   <?php
   	
   	//var_dump($_FILES);  输出关于图片信息的数组
   	if(empty($_FILES)){
   		echo "请上传图片";
   	}else{
   		echo $_FILES["pic"]["name"]."<br/>";
   		echo $_FILES["pic"]["tmp_name"]."<br/>";
   	}
   
   ?>
   ```

   ```php
   // php简单文件上传  move_uploaded_file  把文件从临时文件夹放到文件服务器中
   
   <?php
   	
   	header("Content-type:text/html;charset=utf-8");
   	//var_dump($_FILES);  输出关于图片信息的数组
   	if(empty($_FILES)){
   		echo "请上传图片";
   	}else{
   		//echo $_FILES["pic"]["name"]."<br/>";
   		//echo $_FILES["pic"]["tmp_name"]."<br/>";
   
   		define("PATH",dirname(__DIR__));
   
   		$path = PATH."/upload"."/images";
   
   		$dir1 = date("Ym");//年月
   		$dir2 = date('d');//日
   
   		$fullpath = $path."/".$dir1."/".$dir2;
   
   		if(is_dir($fullpath))
   		{
   			echo "yes";
   		}else{
   			//echo "no";
   			mkdir($fullpath,0777,true);
   		}
   
   
   		$fileName = rand(10000,99999);
   		$fileType = strrchr($_FILES["pic"]["name"], ".");
   		$fileName = $fileName.$fileType;
   
   
   		move_uploaded_file($_FILES["pic"]["tmp_name"], $fullpath."/".$fileName);
   
   	}
   
   ?>
   ```

3. 文件管理

   ```php
   // realpath
   
   <?php
       echo realpath("."); //输出当前目录
   	echo realpath(".."); //输出上一级目录
   ?>
       
   //opendir  打开目录   readdir  读取目录
       $fileName = opendir(".");
   	while($row = readdir($fileName))
       {
           echo $row."<br/>";
       }
   	closedir($fileName);  // 会打印出 . 和 ..
   
   //unlink 删除一个文件
   
   unlink("t1.txt");
   
   
   // file_get_contents 读取文件内容
   
   <?php
       header("Content-type:text/html;charset=utf-8");
   	$str = file_get_contents("t3.php");
   
   	var_dump($str);
   ?>
   
       
   // file_put_contents  写入文件
   
   ```

4. 文件递归

   ```php
   function showDir($path,$lev=0){
       $fh = opendir($path);
       while($row = readdir($fh)){
           //如果目录为. 和 .. 跳过
           if(($row == '.') || ($row == '..')){
               continue;
           }
           echo str_repeat("&nbsp;&nbsp;",$lev).$row.'<br/>'; // 使文件分层
           //如果目录中还有目录，继续往下读取
           if(is_dir($path.'/'.$row)){
               showDir($path.'/'.$row,$lev + 1);
           }
       }
       closedir($fh);
   }
   
   showDir('..');
   ```

### PHP +  Mysql

#### Mysql

1. 客户端

   ```
   phpadmin   
   //改密码后配置phpadmin
   - config.inc.php $cfg['Server'][$i]['password']='123456'
   - config.inc.php $cfg['Server'][$i]['auth_type']='cookie'
   
   console --命令行
   - 在wamp 中找到 mysql console
   - 配置好环境变量后， mysql -uroot -p123456
   
   navicat 
   - 图形化客户端
   ```

2.  mysql 基础命令

   ```mysql
   show databases;  // 查看数据库
   
   set names utf8; // 设置编码为utf8
   
   create database test0002;  //创建数据库test0002
   
   drop database test0002; //删除数据库test0002
   
   ```

3. 数据类型

   1. 整数型
   2. 




