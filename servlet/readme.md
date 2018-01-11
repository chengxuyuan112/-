## 课程回顾：Tomcat服务器和HTTP的协议 ##

**Tomcat服务器**

**WEB简介**
	
	1.网络的架构	C/S		B/S
	2.C/S	客户端/服务器		B/S		浏览器/服务器
		* C/S压力比较小，下载客户端软件，更新	B/S压力比较大，服务器更新。
	
**静态和动态WEB资源**
	
	1.静态：HTML CSS JS	数据不是活的
	2.动态：JSP/Servlet	数据是活的

**服务器简介**

	1.访问：http://ip:端口号/xxx/xxx

**Tomcat服务器**
	
	1.下载和安装服务器
	2.启动服务器	tomcat/bin/startup.bat
	3.关闭服务器	tomcat/bin/shutdown.bat


**常见的启动的问题**

	1.JDK环境没配置好
	2.端口占用的问题

**Tomcat目录结构**

	1.bin		
	2.conf		配置Tomcat配置文件
	3.lib		Tomcat运行时需要的jar包（不是咱们自己编写的第三方jar包）
	4.logs		日志文件
	5.temp		临时文件
	6.webapps	web applications 项目
	7.work		JSP翻译成.java存放的目录

**Tomcat服务器中配置资源**

	1.部署静态的WEB资源
		* 在webapps目录下创建一个文件夹（xxx），直接在xxx目录存放静态资源（HTML CSS JS JSP）
	
	2.部署动态的WEB资源
		* 在webapss目录下创建一个文件夹（yyy）
			yyy   --   项目
			 |    --   存放一些静态的资源(HTML CSS JS JSP)
			 |   
			WEB-INF	-- 名称都是固定的		
				|	--  存放一些静态的资源(HTML CSS JS JSP)
				|
				web.xml	 --  必须要有的（文档声明，根节点）
				classes	 --  文件夹（src目录下所有的java代码，编译成.class文件）
				lib		 --  第三方的jar包（DOM4J）
	3.访问：http://localhost:80/yyy/hello.html
	4.注意：	如果这些资源要是存放在WEB-INF目录下的话，不能直接访问的。http://localhost:80/yyy/WEB-INF/hello.html


**Tomcat与MyEclipse集合**
	
	1.集成
	2.创建项目 -- 部署
	3.启动服务器
	4.关闭服务器
	
	
**Tomcat部署WEB项目**
	
	1.把项目直接复制到webapps目录下
	2.配置虚拟路径
		* 第一种：在tomcat/conf/server.xml/在<Host>标签的中间 配置 <Context path="/itcast" docBase="项目的真实路径" />	
		* 第二种：在tomcat/conf/Catalina/localhost/xxx.xml文件，文件的名称作为虚拟路径（访问路径），在xxx文件中编写<Context  docBase="项目的真实路径" />	


**WEB的通信和配置虚拟主机**


**HTTP的协议**
	
	1.什么是HTTP的协议：
	2.HTTP的请求
		* 请求行
			* 请求方式
				* 常用的请求方式有get和post：
				* get方式把参数列表显示到地址栏上	post封装在请求体中
				* get方式不安全			post安全
				* get方式长度有限制		post支持大数据
			* 请求的地址
			* 协议版本（HTTP1.0 和 HTTP1.1）
		* 请求头
			* if-modified-since		控制本地的缓存
			* referer				记住来源（防止盗链 统计网页的访问 ）
			* user-agent				判断用的什么浏览器
		* 空行
		* 请求体
			* 封装post提交方式的参数列表
	3.HTTP的响应
		* 响应行
			* 协议版本
			* 状态码
				* 200		一起ok
				* 302		完成重定向的功能
				* 304		和两个一起控制缓存
				* 404		资源不存在（客户端有错误）
				* 500		服务器内部错误
			* 状态码描述
		* 响应头
			* location		和302一起完成重定向
			* last-modefied	和if-modified-since和304一起控制缓存
			* refresh		页面定时刷新
			* Content-Disposition	文件下载的时候
			* 禁用浏览器缓存（不用记）
				* 值是固定的
		* 空行
		* 响应体：展示给用户的数据


## Servlet技术（重点） ##

**Servlet简介**
	
	1.什么是Servlet：以后看javaee的文档。
		* Servlet是一个接口，下面有5个方法。
		* Servlet有两个实现类，GenericServlet和HttpServlet
	2.Servlet作用：
		* Servlet是一个小的java程序，运行在服务器端。
		* Servlet接收和响应从客户端发送过来的请求，使用的HTTP的协议。
		* Servlet开发中的角色（看图）
	
**编写一个Servlet程序**
	
	1.编写一个类，实现Servlet接口，重写5个方法。
	2.在web.xml文件中编写Servlet配置文件（记住）
		 <!-- 配置Servlet的信息 -->
		  <servlet>
		  	<!-- 在Servlet起名称（值是任意的） -->
		  	<servlet-name>ServletDemo1</servlet-name>
		  	<!-- 配置Servlet类的包名+类名(Class.forName("包名+类名")) -->
		  	<servlet-class>cn.itcast.servlet.ServletDemo1</servlet-class>
		  </servlet>
		  
		  <!-- 配置访问的路径 -->
		  <servlet-mapping>
		  	<!--  配置Servlet的名称（注意：servet-mapping的节点下的servlet-name必须和servlet节点下的servlet-name值是必须相同的） -->
		  	<servlet-name>ServletDemo1</servlet-name>
		  	<!-- 配置Servlet的访问路径（值有三种写法）想访问ServletDemo1的程序：http://localhost:80/day09/demo1  -->
		  	<url-pattern>/demo1</url-pattern>
		  </servlet-mapping>
	
	3.把day09项目发布到服务器中，启动服务器，访问ServletDemo1的程序。
	4.Servlet被访问的浏览器
	
**Servlet生命周期（面试题）**
	
	1.什么是生命周期：从人的角度人的出生、服务社会、死亡。
	2.Servlet实例的生命周期：从Servlet实例被创建，提供服务，Servlet销毁。在生命周期提供一些方法。
	3.生命周期相关的方法
		* Servlet被创建之后（被服务器创建），立即调用init方法初始化Servlet	init()
		* 从客户端发送过来的任何请求，都会使用service方法进行处理。			service()
		* Servlet从服务器中移除，调用destroy方法进行销毁的操作.				destroy()
	
	4.总结：Servlet实例被Tomcat服务器来创建，第一次访问的时候创建（在内存有一个实例（单例模式）），立即调用init方法进行初始化的操作，使用service方法对外提供服务（有一个请求，实例开启一个新线程，处理请求的内容），service有一次请求，service被调用一次。Tomcat服务器关闭或者移除项目的时候，Servlet被销毁，但是在销毁之前调用destroy方法进行一些销毁操作（释放一些资源），调用destory的方法一次。
	
	
**Servlet在Tomcat服务器怎么运行的**
	
	1.看图03
	
**Servlet的类之间的关系**
	
	1.Servlet接口
	2.GenericServlet实现类
	3.HttpServlet实现类
		
		Servlet接口
			|
		GenericSerlvet （5个方法全部都重写了）
			|
		HttpServlet (service方法进行重写了)
			|
		MyServlet（相当于重写了5个方法，）
		
	4.问题：
		* 编写一个类继承GenericServlet，里面有两个init方法，重写init重写哪一个呢？
			*  如果以后想要重写init方法，重写init()方法。
		* 编写一个类，继承HttpServlet，重写了两个service方法，分别干嘛用？
			ServletRequest（请求）    		ServletResponse（响应）
					|						  			|
			HttpServletRquest				HttpServletResponse
		* HttpServlet类中有两个service方法，其中一个service的方法的内部，判断请求的方式（get和post），如果是get请求方式，默认调用doGet方法。如果post请求，默认调用doPost的方法。
		
		* 现在在网络上就使用两种请求，一种get和post。如果编写一个类，继承HttpServelt，重写doGet和doPost方法。因为service方法默认去调用doGet或者doPost方法。开发中最简单的方式。
		
	5.开发Servlet程序（最简单的步骤）
		* 编写一个类，继承HttpServlet
		* 重写两个方法doGet和doPost方法
		* 在web.xml中编写配置文件
		
	6.一般的情况下，doGet和doPost方法中的逻辑都是相同，互相调用，简化编程。
		

**修改Servlet开发的模板**
	
	1.修改Servlet的模板，先找到MyEclipse的安装路径。
	2.找到该jar包com.genuitec.eclipse.wizards.xxx.jar
	3.不要解压 -- 右键 -- 用解压工具打开 -- 找templates目录 -- Servlet.java -- 修改该文件 -- 把doGet和doPost方法的注释和代码删除，在doPost方法的里面编写已经 doGet(request,response);
	4.重启MyEclipse。创建Servlet程序。
	

**Servlet配置自动加载**
	
	1.Servlet是第一次被访问的时候创建Servlet实例，立即调用init方法进行初始化。
	2.假如：AServlet（非常销毁时间的操作），第一次访问的时候，需要等待的时间非常长。
		* 可以配置Servelt自动加载来解决这一类问题。服务器一启动，自动加载Servlet加载。
		
		* 在web.xml的配置文件中，在<servlet>标签的中间
			<!-- 自动加载：服务器一启动，默认加载该类  值是正整数 ，值越小，优先级就越高。 -->
    		<load-on-startup>2</load-on-startup>
	3.注意：值是正整数，值越小优先级越高。
	
	
**Servlet配置访问路径**
	
	1.学会怎么配置？学会怎么来访问？优先级，谁的优先级高先访问谁。
	2.在web.xml中，配置<url-pattern>，怎么来访问Servlet程序。
		* <url-pattern>/servlet/demo1</url-pattern>	-- 完全路径匹配 -- 访问：http://localhost/day09/servlet/demo1
		* <url-pattern>/*</url-pattern>	-- 目录匹配 -- 访问：http://localhost/day09/xxx
		* <url-pattern>*.do或者 *.action</url-pattern> -- 扩展名 访问：http://localhost/day09/xxx.do
	
	3.总结
		* 完全路径匹配	/demo1  以/开头		访问：http://localhost/day09/demo1
		* 目录匹配		/*		以/开头  	访问：http://localhost/day09/xxx
		* 扩展名匹配		*.do	不能以/开头 	访问：http://localhost/day09/yyy.do
	
	4.优先级（一个WEB程序中有多个Servlet程序，到底是访问哪个Servlet呢？）
		* 完全路径匹配 > 目录匹配 > 扩展名匹配
		
		
**Servlet开发中WEB阶段的路径问题**	

	1.学习编写路径
	2.相对路径和绝对路径
		* 不能以/开头
		* 相对路径：当前的这个文件和比对的文件的位置关系。
		* 在WEB服务器中开发相对路径的写法
			* 先访问aa.html  http://localhost/day09/aa.html
			* 访问demo5写法      http://localhost/day09/demo5
			* 在aa.html的文件中直接访问demo5 	
				* <a href="demo5">aa</a><br/> 
				* <a href="./demo5">aa</a><br/>
			
			* 先访问bb.html  http://localhost/day09/bb/bb.html
			* 访问demo5写法      http://localhost/day09/demo5
			* 在bb.html的文件中访问demo5
				* <a href="../demo5">bb</a><br/>
	
	3.总结：相对路径的写法，需要先找这两个文件之间关系。但是如果一旦文件的位置改变了，写的相对的路径写法存在问题。解决这种问题，写绝对路径。	
	
	4.编写绝对路径
		* 以/开头的
		* http://localhost/day09/demo5
		* 简便写法： /day09/demo5
		
	5.在绝对路径中有分成两种
		* 客户端绝对路径
			* 包含项目名称	/day09/demo5
		* 服务器端绝对路径
			* 不包含项目名称	/demo5
	
**ServletConfig对象**
	
	1.和在web.xml的配置文件中相关的一个对象。
	2.ServletConfig对象的方法
		* String getServletName() 		获取配置Servlet的名称（<servlet-name>ServletDemo5</servlet-name>） 
		* String getInitParameter(String name)  	获取初始化参数
 		* Enumeration getInitParameterNames()  		获取初始化参数
		
		* ServletContext getServletContext()  		获取ServletContext对象
	
	3.总结：配置初始化参数只能针对当前的Servlet，不能在其他的Servlet程序中来获取。
		
**ServletContext对象（重要）**

**ServletContext对象的简介**
	
	1.在javaweb的开发中，一共存在（JSP）4个域对象。在Servlet中一共存在3个域对象。
	2.学习的第一个域对象。
	
	3.ServletContext对象
		* 一个WEB项目只有一个ServletContext对象
		* 可以被WEB项目中的所有的Servlet所共享，可以通过ServletContext来传递数据。
		* 与天地同寿。Tomcat服务器一启动，会为每一个WEB应用创建一个ServletContext对象。服务器关闭的时候ServletContext销毁。
	
**获取ServletContext对象**
	
	1.可以通过ServletConfig获取ServletContext对象
	2.可以直接使用该方法：ServletContext getServletContext()  

**域对象**
	
	1.域对象可以在多个Servlet中传递数据。以后需要学习JSP技术（JSP和Servlet程序之间通过域对象来传递数据）
	2.可以向域对象中存入数据	
		* setAttribute("属性名称","值");			setAttribute("xx","123")	setAttribute("xx","456")
	3.可以从域对象中获取数据
		* Object getAttribute("属性名称");
	4.从域对象中删除属性
		* removeAttribute("属性名称");
	
**获取ServletContext作用**
	
	1.获取全局的初始化参数
		* String getInitParameter(String name)  	
		* Enumeration getInitParameterNames()  
		* 配置全局的初始化参数
		
	2.实现数据的共享（*****）
		*  void setAttribute(String name, Object object) 	设置属性的值
		*  Object getAttribute(String name)  				获取属性的值
		*  void removeAttribute(String name)  				删除属性的值
		*  Enumeration getAttributeNames() 					获取属性的名称们
					
		* 统计访问的次数
		
	3.读取资源文件（*****） String path的写法
		* InputStream getResourceAsStream(String path)  	通过文件的路径获取文件的输入流
		* String getRealPath(String path) 					通过文件的路径获取文件的绝对磁盘路径
 		
	4.总结：读取资源文件path路径的写法
		* 如果db放在src目录下：		path = "/WEB-INF/classes/db.proterties";
		* 如果db放在某个包下：			path = "/WEB-INF/classes/cn/itcast/context/db.proterties";
		* 如果db放在WebRoot目录下：	path = "/db.proterties";
		
	5.使用类的加载器来读取资源文件
	
**缺省的Servlet（了解）**



request和response对象（案例）
		
	