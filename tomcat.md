## 课程回顾：Java基础加强 ##

**MyEclipse简介**

**DeBug调试技巧**
	
	* F5	跳入		
	* F6	跳过 执行完这一行
	* F7 	跳出
	* F8	执行下一个断点，如果没有，执行完成
	* Drop To Frame	跳到方法的第一行

**MyEclipse的快捷键**

**JUnit测试**
	
	* 单元测试：最小单位是一个方法，你自己编写一个方法，需要进行测试。
	* JUnit包含的版本：3.x和4.x（使用）
	* 最终的规范
		* public 共有的
		* void	没有返回值
		* 没有参数列表
	
	* @Test		测试方法
	* @Ignore	忽略方法
	* @Before	在方法执行之前执行该方法
	* @After	在执行方法之后执行该方法
	* @Test(timeout=100)	在多少毫秒内执行完成

**JDK5.0的特性**

**泛型**
	
	* 应用在集合上，属于源代码阶段，但是一变成.class文件，泛型背擦除。
	* 使用集合进行遍历	List	Set		Map
	* 自定义泛型方法和类
		* 跟逻辑没关，数据类型可以是变化，就可以是自定义泛型。
		* public <T> void xxx(){ T t }
		* xxx(Integer )
		class User<T>{
					
		}
		* 如果静态方法不能使用类的泛型	

**枚举**
	
	* 作用：在一定范围内取值。常量代替（现在用）。
	* 引入了枚举
		enum Color{
			RED,BULE;
		}
	* 枚举的构造方法是私有的。	
	* 特殊的枚举类
		* 重写了枚举的构造方法
		* 如果枚举要是有抽象方法
		* 在枚举的类中也可以写普通方法

	* 枚举api

**自动拆装箱**
	
	* 自动拆箱：把包装类赋值给基本数据类型
	* 自动装箱：把基本数据类型赋值给包装类
	* 原理：Integer.initValue()	Integer.valueof()
	* 面试题
		Integer a1 = 100;
		Integer a2 = 100;
		Integer a3 = new Integer(100);
		Integer a4 = new Integer(100);

**增强for循环**
	
	* 语法：for(数据类型 变量 : 要遍历的集合){  }
	* 哪些集合可以使用增强for循环：实现Iterable接口的类，都可以使用for循环。
	* 增强for循环的底层也是使用迭代器来完成的。

**可变参数**
	
	* 参数：int... arrs	作为参数出现。一个方法只能有一个可变参数，必须出现参数列表的末尾位置。
	* 可变参数底层是数组。传入的值个数不定，可以传入一个数组。
	
**反射技术**
	
	* 反射的原理：就是通过Class对象获取.class文件中的内容（构造器、属性和方法）
	* 应用：服务器	框架
	* 获取Class的对象三种方式：
		* 类名.class
		* 实例对象.getClass()
		* Class.forName("包名+类名");
	* 有了Class对象
		* 可以获取构造器对象
		* 可以获取属性对象
		* 可以获取方法对象


## Tomcat服务器 和 HTTP协议 ##

**WEB开发简介**
	
	* 当前网络上两种架构
		* C/S	Client/Server	客户端/服务器		需要下载客户端软件		例子：QQ		快播		暴风影音
			* 优点：服务器压力相对比较小，安全性比较高。
			* 缺点：需要下载客户端软件，总去更新。
		* B/S	Browser/Server	浏览器/服务器		不需要下载客户端软件（客户端就指浏览器）	例子：购物网站（淘宝  京东） 12306
			* 缺点：服务器压力比较大（硬件比较强）
			* 优点：浏览器，不用更新，服务器端去做更新了。
		
		
**WEB相关知识**
	
	* WEB：网页。JavaWeb：使用Java语言来开发网页。
	* 静态的WEB资源
		* HTML CSS JavaScript
	* 动态的WEB资源
		* Servlet/JSP
	
	* 静态和动态的区别
		* 动态的资源数据是活的，例子：假如说班长登陆淘宝，显示班长的名字。我登陆了淘宝，我的名字。
	
	* 微软	ASP.net
	* PHP	小巧（开发网站非常强大，处理大数据）
	* RUBY	小日本
	
	* Java语言优点：开发了网站，没有任何优势。优势是服务器端，处理业务（电信，淘宝，银行）。
	
	* 静态Web资源：简单一句话，浏览器能看的懂的。
	* 动态Web资源：先需要服务器把它转换成HTML，再给浏览器看。
	
	
**服务器的简介**
	
	* 服务器整体概念：
	* 硬件：一台电脑。
	* 软件：服务器的软件，Tomcat服务器软件。
	* 如果安装了服务器软件了，启动服务器和关闭服务器。假如启动了服务器，怎么访问？
		* 访问：http://www.baidu.com	一回车访问百度了
		* http://			代表HTTP的协议
		* www.baidu.com		域名（DNS域名服务器注册	.baidu.com  61.135.169.125）
		* 最终：http://192.168.1.100:端口号（默认值80）
		* 最终：http://192.168.1.100:80/index.html
		
**常见的WEB服务器**	
	
	* Tomcat（Apache）	开源免费的	开发中应用最广的服务器	支持JSP/Servlet规范	SSH
	* JBoss				免费的		支持JAVAEE所有的规范	EJB规范 JSP/Servlet
	* Weblogic	原来的公司BEA公司		收费的 大型的服务器	支持JAVAEE所有的规范	被Oracle收购了
			SUM公司（Java语言）	+ 	数据库（Oracle MySQL（也被收购了））	+  服务器（Weblogic）
	* Websphere	公司的IBM公司			收费	  大型的服务器	支持JAVAEE所有的规范
	
	
**Tomcat服务器**
	
	* 下载tomcat服务器，安装版本和解压版本。现在都使用解压版本（7.x）
	* 解决文件，放在本地的磁盘上（目录：不要有中文和空格）
	* 启动服务器：在tomcat/bin/startup.bat（批处理文件），双击文件。弹出黑色的窗口。服务器成功。（不要把黑窗口关闭）
	* 访问服务器的主页：http://localhost:8080	就可以访问tomcat默认主页
	* 关闭服务器：（关闭黑窗口，关闭暴力的），温柔的关闭。在bin的目录下，有shutdown.bat。双击该文件，关闭服务器。
	
	
**常见问题**
	
	* 第一个注意：必须安装JDK，必须配置Java_Home环境变量。窗口一闪而过。说明环境变量没配置好。
	* 不小心，已经启动了一个服务器，又想启动服务器。端口占用的问题。
		* 端口占用的问题：java.net.BindException: Address already in use: JVM_Bind
		* 解决占用的问题：
			* 先找到占用端口的应用程序，结束掉该应用程序。
				* netstat  –ano	查看所有占用端口的应用程序，找到程序的PID，要任务任务管理器中结束掉。	
				* 有一个应用一直占用，一开机就占用。
			* 修改端口号（修改tomcat服务器的端口号）。（默认是8080，改成其他的端口号）
				* Tomcat服务器的配置文件	-- tomcat/conf/server.xml的文件
				* 一般情况下改成80，80端口是HTTP协议的默认端口号，访问可以不写。
				* 如果万一占用的80端口，干掉它。系统中的服务要是占用80端口，禁用该服务。
				
	* 如果系统自带的微软服务器IIS（World wide web publish IIS），去系统服务中把服务禁用。
	
**Tomcat目录结构**
	
	** bin				可执行文件（启动和关闭）
	***** conf			存放的Tomcat配置文件
	*** lib				给Tomcat服务器运行时所需jar
	*** logs			存放Tomcat运行时产生日志文件。
	** temp				Tomcat运行时产生临时文件
	***** webapps		Wab Applicatons（WEB应用们），在该目录下存放就是项目。
	***** work			JSP翻译成.java的文件，存放在work的目录下。
	
**在webapps目录创建静态和动态的WEB资源**
	
	* webapps目录下存放的是项目，项目区分静态的WEB资源和动态的WEB资源。
	* 静态和动态在webapps的目录下存在的方式不一样。
		* 如果静态的WEB资源 -- 在webapps目录创建一个文件（项目名称） -- 直接可以放在静态资源（HTML CSS JS）
		* 如果动态的WEB资源 
			* 在webapps目录下创建一个文件
			* 在该文件下创建WEB-INF的目录（名称固定、大写固定）
			* 在WEB-INF目录下web.xml的文件（必须要有，有文档声明，根节点和约束，复制一份）
			* 在WEB-INF目录下		classes文件夹（.class文件）
			* 在WEB-INF目录下		lib文件夹（引入第三方jar包）
			
			
**MyEclipse和服务器集成**
	
	* MyEclipse是开发工具（编写代码）
	* Tomcat服务器：运行的项目
	* MyEclipse和Tomcat集成的步骤
		* window -- 首选项 -- MyEclipse -- servers -- Tomcat7.x -- 配置JDK -- 配置Tomcat目录 -- Enable，点击ok
		* 启动 -- 在server窗口中 -- 右键选择启动和关闭
		
	* 如果在MyEclipse部署项目。
	
**Context上下文（虚拟路径）**
	
	* 虚拟路径：理解访问路径（默认和项目名称是相同的）。
	* 发布到服务器中，作为访问路径。
	* 总结：在webapps的目录下的项目的名称其实是虚拟路径（访问路径），虚拟的路径默认情况下和项目名称是相同的。
	
	
**部署项目（两种方式）**
	
	* 直接把项目复制到webapps的目录下
	
	* 原因：需要把你的系统部署到公司的服务器。
	* 在tomcat/conf/server.xml -- 在<Host>标签的中间配置虚拟路径（希望找到C盘oa的项目）
		<Context path="/itcast"  docBase="C:/oa" />
		* path="项目的虚拟路径（访问路径）"
		* docBase="指定真实项目的路径"
	
	* 也是配置虚拟路径的方式，但是不用去修改tomcat/conf/server.xml。
	* 创建一个XML的配置文件，名称可以是任意（例子：hello5.xml），它就会以文件的名称作为虚拟路径（访问路径）。我就会把hello5文件的名称作为虚拟路径。 http://localhost/hello5.
	* hello5文件书写的内容：
		* 文档声明
		* 直接编写<Context docBase="c:/ob"  />


**WEB通信**
	
	* 看图
	
**配置虚拟主机（了解）**
	
	<Host>完成虚拟主机的配置。
	
	
## HTTP的协议（重点）重点掌握头的信息，固定的作用 ##

**HTTP协议的简介**
	
	* 什么是HTTP的协议：协议：甲乙双方根据一些规定达成的共识。人与人之间的协议。
	* 人与计算机怎么沟通呢？人通过浏览器与计算机的服务器进行沟通。
	* 客户端与服务器之间怎么沟通：涉及到数据的传输。风姐传到服务器端，接收凤姐，服务器内部查找内容，返回给你浏览器。
	* 凤姐是怎么传输啊？图片或者html的内部怎么传输啊？
	* HTTP的协议
		* 把凤姐数据封装到协议规定的格式里，发送到服务器。
		* 服务器把HTML，图片的数据封装到协议的规定的格式，返回给浏览器。
	* HTTP协议的格式
		* 咱们要学的是这些格式？这是格式有一些内容，需要学的？
	
	* 请求：从客户端发起，向服务器端发送请求。
	* 响应：从服务器做出回应，接收到客户端发送过来的请求，对客户端做出了响应。
	

**HTTP协议的版本**
	
	* HTTP协议1.0	
		* 从客户端链接服务器端，发送请求，得到响应。立即断开。
	* HTTP协议1.1（现在使用）	
		* 从客户端链接服务器端，发送请求，得到响应。不会立即断开，链接一会，如果一段时间内，没有请求，自动断开。
		
**HTTP协议的请求**
	
	* 请求行
		* 请求方式
			* 提交方式有哪些？
			* 提交方式有很多，主要有两种，get和post。之间区别：
		* 提交的地址	
		* 协议版本	HTTP/1.1
	* 请求头
		Accept: text/html,image/*    
		Accept-Charset: ISO-8859-1
		Accept-Encoding: gzip
		Accept-Language:zh-cn 
		Host: www.itcast.com:80
		If-Modified-Since: Tue, 11 Jul 2000 18:23:51 GMT
		Referer: http://www.itcast.com/index.jsp
		User-Agent: Mozilla/4.0 (compatible; MSIE 5.5; Windows NT 5.0)
		Connection: close/Keep-Alive   
		Date: Tue, 11 Jul 2000 18:23:51 GMT
			
		* 重点的有
			* If-Modified-Since		需要和响应头和304（状态码）和在一起使用，控制本地的缓存。
			* Referer				记住当前网页的来源（作用：统计网站的访问，防止盗链）
			* User-Agent				获取浏览器的版本信息
			
	* 空行
	* 请求体
		* 封装的是post提交方式的参数列表。
	
	
**HTTP协议的响应**
	
	* 响应行
		* 协议版本
		* 状态码（重点记住）
			* 200 ：请求成功处理，一切OK		
			* 302 ：请求重定向
			* 304 ：服务器端资源没有改动，通知客户端查找本地缓存 
			* 404 ：客户端访问资源不存在
			* 500 ：服务器内部出错 
			
		* 状态码描述
	* 响应头
		Location: http://www.it315.org/index.jsp 
		Server:apache tomcat
		Content-Encoding: gzip 
		Content-Length: 80 
		Content-Language: zh-cn 
		Content-Type: text/html; charset=GB2312 
		Last-Modified: Tue, 11 Jul 2000 18:23:51 GMT
		Refresh: 1;url=http://www.it315.org
		Content-Disposition: attachment; filename=aaa.zip
		Expires: -1
		Cache-Control: no-cache  
		Pragma: no-cache   
		Connection: close/Keep-Alive   
		Date: Tue, 11 Jul 2000 18:23:51 GMT
		
		* 重点的响应头	
			* Location					和302一起完成重定向
			* Last-Modified	和 If-Modified-Since	 和304一起来完成控制缓存的操作。
			* Refresh					定时页面刷新（页面定时跳转）
			* Content-Disposition		文件下载的时候需要使用		
			* 下面这三个头需要一起使用
				Expires: -1
				Cache-Control: no-cache  
				Pragma: no-cache
				作用：禁用浏览器缓存。
		
	* 空行
	* 响应体：服务器向客户端返回的数据。