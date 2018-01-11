## 课程回顾：Servlet技术（重点） ##

**Servlet技术的简介**
	
	1.做什么工作：一个小的java程序，运行在服务器端，接收和响应从客户端发送过来的请求，使用HTTP协议。
	2.开发Servlet步骤：
		* 实现Servelt接口
		* 在web.xml的文件中编写配置文件
			* 配置Servlet的信息	
				<servlet>
					<servlet-name>xxx</servlet-name>
					<servlet-class>cn.itcast.servet.xxx</servlet-class>
				</servlet>
				<servlet-mapping>
					<servlet-name>xxx</servlet-name>
					<url-pattern>/demo1</url-pattern>
				</servlet-mapping>
	
**Servlet的生命周期**
	
	1.Servlet服务器创建的，第一次访问的时候创建的，调用init方法进行初始化，客户端发送请求，服务器调用service方法处理。每一次调用该方法，关闭服务器或者移除项目，销毁Servlet，调用destroy的方法，调用一次。
	
**Servlet开发的细节**
	
	1.继承HttpServlet
	2.重写doGet和doPost方法
	3.在web.xml中编写配置文件

**配置Servlet自动加载**
	
	1.服务器一启动，创建实例。
	2.配置 <serlvet>标签的中间配置		
	3. <load-on-startup>
	4. 中间的值是正整数，值越小，优先级越高。

**url-pattern的配置**

	1.完全路径匹配	/demo	访问：localhost/day09/demo	
	2.目录匹配		/*		访问：localhost/day09/demo	
	3.扩展名匹配		*.do	访问：localhost/day09/demo.do
	4.优先级：完全 > 目录 > 扩展名

**在WEB开发中路径的编写**
	
	1.相对路径
		* 找到关系
	2.绝对路径
		* 正常的写法：http://localhost/day09/demo
		* 简便的写法：/day09/demo
	3.客户端绝对路径
		* /day09/demo
	4.服务器端绝对路径
		* /demo


**ServletConfig对象**

	1.获取初始化参数
	2.获取servlet配置文件的名称	


**ServletContext对象**
	
	1.一个WEB项目只有一个ServeltContext对象
	2.在N个Servlet来传递数据
	3.与天地同寿

**获取ServletContext对象**
	
	1.通过ServletConfig来获取ServletContext
	2.在GenericServlet有一个方法，getServletContext();
	
**域对象**
	
	1.在servlet可以传递数据。
	2.setAttribute("xx","yy");		设置属性的值（如果之前已经有了，产生覆盖的效果）
	3.getAttribute("xx");			获取属性的值
	4.removeAttribute("xx")			删除属性的值

**ServletContext的作用**
	
	1.获取全局的初始化参数
	2.共享数据
	3.读取资源文件
		* getResourceAsStream(String path);
		* getRealPath(String path);
	
**路径的写法**
	
	1.整个WEB项目发布到服务器端。
	2.编写绝对路径（以/开头），客户端的绝对路径（包含项目名称）和服务器端路径（不包含项目名称）
	
	
	* getResourceAsStream(String path);	值的写法不包含项目名称		/WEB-INF-classes/xxxx
	* getRealPath(String path); 值的写法不包含项目名称				/xxxx


**缺省的Servlet（了解）**
	
	1.在Tomcat服务器中，提供一个类。
	2.在tomcat/conf/web.xml中，配置DefaultServlet。访问的路径配置的是 <url-pattern>/</url-pattern>
	3.默认去处理静态的资源。
	4.如果自己编写的配置文件	<url-pattern>/</url-pattern>	 千万不要配置成/
	
	<servlet>
        <servlet-name>default</servlet-name>
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
        <init-param>
            <param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>
        <init-param>
            <param-name>listings</param-name>
            <param-value>true</param-value>			--   改成true
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
	
## ServletRequest和ServletResponse ##

	1.服务器来创建request和response对象。（浏览器是创建的这两个对象吗？必须错）
	2.request请求的对象（从客户端发送过来的请求，都给我找该对象）
	3.response响应的对象（在服务器端编写代码，向客户端做出响应，找该对象）
	
**ServletResponse响应的对象（从服务器端向客户端响应的）**

**介绍一些方法**
	
	void setHeader(String name, String value)  		设置响应头的信息
	void setIntHeader(String name, int value) 		值是int类型
	void setDateHeader(String name, long date)  	值是日期类型，参数是long 毫秒值
 	void addHeader(String name, String value)  		设置响应头的信息
	addIntHeader(String name, int value) 
	addDateHeader(String name, long date)
	
	* 一般的情况下，一个头对应一个值.（重要）
		* setHeader(String name, String value) 	 
			* setHeader("xx","yy")
			* setHeader("xx","zz")
			* 结果：xx:zz
	* 特殊的情况下，一个头对应多个值
		* addHeader(String name, String value) 	 
			* addHeader("xx","yy")
			* addHeader("xx","zz")
			* 结果：xx:yy,zz
	
	void setStatus(int sc)  						设置状态码（200 302 ）
	void setCharacterEncoding(String charset)  		设置response缓冲区的编码
	void setContentType(String type)  	
	void sendRedirect(String location)  			可以完成重定向的功能
	
	ServletOutputStream getOutputStream()  			获取输出的字节流
	PrintWriter getWriter()  						获取输出的字符流
		

**案例**
	
	重定向（登陆页面）				使用302和location头完成重定向
	页面定时刷新（页面读秒操作）	有一个定时刷新的头 refresh		
	禁用浏览器缓存（三个头信息）	有三个头（不要记）
	向页面输出中文（乱码问题）		
	实现文件下载					头Content-Dispositon
	实现验证码					
	
	
**重定向的例子（值的写法就包含项目名称）**
	
	1.特点：访问的是demo2，但是地址栏变成了demo3。记住：两次请求，两次响应。
	2.需求：登陆的html页面，用户名和密码。判断用户名和密码是否都是admin，如果都是，登陆成功，如果都不是，登陆失败（重定向到登陆页面）。
	
	3.掌握的知识
		* 重定向先理解（借钱）
		* 会编写重定向的代码（302 location）
		* location的值的路径写法：（包含项目名称）
		* 重定向的应用

	4.response对象提供了一个方法，可以直接完成重定向的功能。
		* 设置状态码302  提供地址
		
		* void sendRedirect(String location)	
	
	
**页面定时刷新（refresh）**
	
	1.知道refresh完成的功能
	2.refresh的值的写法。
		* response.setHeader("refresh","5;url=/day10/html/sucess.html");
	3.前台的知识。
		* span标签获取文本内容
		* 加载事件	onload
		* 定时器	
	
**禁用浏览器的缓存**
	
	1.知道有三个头。
	2.头值的写法必须知道。
	3.知道禁用浏览器缓存有什么？
		* 网银系统
		* 页面的数据一直发生变化。（现在的浏览器避免了这个问题）
	4.头信息
		response.setHeader("Cache-Control", "no-cache");
		response.setHeader("Pragma", "no-cache");
		// 第三个头是日期类型
		response.setDateHeader("Expires", -1);	
		
		
**向页面输出中文乱码的问题**
	
	1.向页面输出中文有乱码问题。
		* ServletOutputStream getOutputStream()  		获取输出的字节流
		* PrintWriter getWriter()  						获取输出的字符流
		
	2.解决字节输出中文乱码的问题
		*.设置浏览器打开文件时采用的编码。（编码一）		
	  			response.setHeader("Content-Type", "text/html;charset=UTF-8");
	  	*.获取中文的字节数组也采用固定的编码。（编码二）	
	  			"哈罗我的".getBytes("UTF-8")
	  	*.只需要编码一和编码二保证一致就不会乱码。

	3.解决字符的中文乱码
		*.设置response缓冲区的编码（默认是ISO-8859-1）。
	 		response.setCharacterEncoding("UTF-8");
	  	*.设置浏览器的默认打开文件的编码
	  		response.setHeader("Content-Type", "text/html;charset=UTF-8");
	
	4.字符的中文乱码可以有简单的写法
		response.setContentType("text/html;charset=UTF-8");
	
	
	5.总结：使用respone对象向浏览器输出中文的。
		* 子节(response.getOutputStream().write())	
			* 设置浏览器打开文件时所采用的编码
				* response.setHeader("Content-Type", "text/html;charset=UTF-8");
			* 获取中文的字节数组的时候，提供一个编码。
				* "哈罗我的".getBytes("UTF-8")
		
		* 字符(response.getWriter().write())
			* 设置浏览器打开文件时所采用的编码
				* response.setHeader("Content-Type", "text/html;charset=UTF-8");
			* 设置response缓冲区的编码。
				* response.setCharacterEncoding("UTF-8");


**文件下载**
	
	1.了解头信息	Content-Dispositon	值的写法：attachment;filename=文件的名称	我是作为附件形式打开。
	2.先准备一个文件，下载的文件。
	3.输入流（获取文件的输入流）和输出流（向浏览器端输出）
	4.知道下载的头信息的作用。

**验证码分析**
	
	/**
	 * 1.获取画布对象（纸）
	 * 2.获取画笔的对象	
	 * 	2.1 画一个实心的矩形，大小和画布的大小。产生了覆盖。
	 * 	2.2 画一个有边框，画一个空心的矩形，
	 * 3.提前准备好数据（大小写字母和数字）	ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890
	 * 	3.1 随机的4个获取数据
	 * 	3.2 把随机获取的数据画到画布上
	 * 4.画干扰线
	 * 5.把内存中验证码输出客户端
	 * 
	 * 6.如果向要完成旋转的效果使用  Graphics2D对象  void rotate(double theta, double x, double y) 
	 * 		double theta ：代表的是弧度
	 * 		弧度 = 角度 * Math.PI / 180;
	 * 		随机获取正负30之间的角度 = random.nextInt(60) - 30;
	 */
	
	
**验证码**
	
	1.先知道画布的对象（BufferedImage） 画笔对象（Graphics） 把内存中的图片输出到客户端（ImageIO）
	2.相关类的一些方法，也知道。
	3.提前准备好一些数据（英文和中文也提前准备好的），随机能获取到这些字符（Random随机类）
	
	
**ServletRequest请求的对象（从客户端来的）**
	
	* String getParameter(String name)  			获取请求的参数（文本框，密码框）
	* String[] getParameterValues(String name) 		获取请求的参数（复选框）
 	* Map<String,String[]> getParameterMap() 		获取请求参数（封装数据，使用该方法）
 	
	* Object getAttribute(String name) 
 	* void setAttribute(String name, Object o) 
 	* void removeAttribute(String name) 
  	
	* String getHeader(String name) 				获取请求头	referer  user-agent
	

**获取客户端的一些信息**
	
	1.重点：getContextPath()  -- 获取虚拟路径的名称（默认和项目名称相同）
	http://localhost:8080/day10/AServlet?username=xxx&password=yyy
　　　　> String getScheme()：获取协议，http
　　　　> String getServerName()：获取服务器名，localhost
　　　　> String getServerPort()：获取服务器端口，8080
　　　　> *****String getContextPath()：获取项目名，/day10
　　　　> String getServletPath()：获取Servlet路径，/AServlet
　　　　> String getQueryString()：获取参数部分，即问号后面的部分。username=xxx&password=yyy
　　　　> String getRequestURI()：获取请求URI，等于项目名+Servlet路径。/day10/AServlet
　　　　> String getRequestURL()：获取请求URL，等于不包含参数的整个请求路径。http://localhost:8080/day10/AServlet

**获取请求头的信息**
	
	1.String getHeader(String name) 	获取头信息
	2.请求头
		* if-modified-since		控制缓存
		* referer				记住当前网页的来源（防止盗链）
		* user-agent				判断浏览器的版本


**获取请求的参数（表单）**
	
	* String getParameter(String name)  			获取请求的参数（文本框，密码框）
	* String[] getParameterValues(String name) 		获取请求的参数（复选框）
 	* Map<String,String[]> getParameterMap() 		获取请求参数（封装数据，使用该方法）
	
	1.注意：getParameter获取是一个key对应一个value的情况。（文本框、密码框）
	2.注意：getParameterValues获取第一个key对应多个value的情况。（复选框）
	3.注意：getParameter(参数：表单的name属性的值)
 	
	4.总结：request对象获取中文乱码的问题
		* post提交
			* 设置request缓冲区的编码（注意：放在获取参数的方法之前）
			* request.setCharacterEncoding("UTF-8");
		* get提交
			* 先获取请求参数
			* username = new String(username.getBytes("ISO-8859-1"),"UTF-8");
		

**request域对象**
	
	1.ServletContext对象：代表整个WEB应用。服务器一启动，就创建一个ServletContext对象。
	2.request域对象：就是一次请求的范围内。（重定向是两次请求，可不可以使用request域在重定向中进行传递数据呢？）
	
	* Object getAttribute(String name) 				获取属性的值
 	* void setAttribute(String name, Object o) 		设置属性值
 	* void removeAttribute(String name)				删除属性值
	
**转发和重定向的区别（面试题）**

	1.重定向：借钱。
	2.转发：也是借钱。
	
	3.想完成转发的操作
		* RequestDispatcher getRequestDispatcher(String path) 
		* request.getRequestDispatcher(path);返回是RequestDispatcher的对象。在这个对象有两个方法，
		* void forward(ServletRequest request, ServletResponse response)  就是转发的方法
 		
	4.转发和重定向的区别
		* 重定向的地址栏发生变化，转发地址栏不会发生变化。
		* 重定向是两次请求和响应，转发是一次请求和响应。
		* 重定向不能使用request域对象传值，转发可以使用request域对象传值。
		* 重定向的路径（/day10/request2），转发的路径（/request2）
		* 重定向可以定向到任何页面（网络上的资源），转发只能在web应用内部。
		
**细节**
	
	1.字节流和字符流互斥的，不能一起使用。
	2.在完成的时候，会把response的缓冲区清空。
	
**RequestDispatcher类**
	
	1.void forward(ServletRequest request, ServletResponse response)  转发（重点）
	2.void include(ServletRequest request, ServletResponse response)  包含（了解） 可以把几个页面包含到一起，完成页面的布局工作。
	
	

request.getParameter()	获取参数列表（和form表单或者超链接提交有关的）

request.getAttribute()  获取request域对象中的值的


**路径的问题**
	
	1.ServletContext域对象有两个方法
		* getResourceAsStream("路径的写法不包含项目名称  /WEB-INF/classes/xxx ")
		* getRealPath("路径的写法不包含项目名称   /WEB-INF/classes/xxx")
	
	2.重定向
		* 路径的写法：必须包含项目名称	response.setHeader("location","包含项目名称   /day10/demox");
	
	3.转发的路径
		* 路径的写法：不包含项目名称	request.getRequestDispatcher("不包含项目名称   /demox").forward(request, response);
	
	4.refresh定时刷新路径
		* response.setHeader("refresh","5;url=/day10/demox");


<url-pattern>/AServlet</url-pattern>
localhost:8080/day10/AServlet

String str = "username=xxx&password=yyy";
