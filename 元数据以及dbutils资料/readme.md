## 课程回顾：事物和连接池 ##

**事物**

	1.啥是事物：作用要么全都成功，要么失败。
	2.事物的4大特性：
		* 原子性	
		* 一致性	
		* 隔离性	
		* 持久性	

	3.在MySQL中使用事物
		* start transaction;
			* sql
		* commit  或者 rollback
		
		* set autocommit = 0或者off
	
	4.JDBC中代码实现
		* conn.setAutoCommit(false);
		* conn.comiit();
		* conn.rollback();
	
	5.不考虑隔离性，引发的问题
		* 脏读		
		* 不可重复读	
		* 虚读		

	6.避免问题发生（设置隔离级别）
		* 未提交读
		* 提交读
		* 可重复读
		* 串行


**数据库的连接池**
	
	1.连接池概述
	2.装饰者模式（增强某个类中的方法）
		* 增强的类和被增强的类必须实现相同的接口
		* 增强的类可以获取被增强类的引用
		
	3.DBCP连接池
		* 手动设置4大参数
		* 提供配置文件（db.properties）
		
	4.C3P0连接池
		* 手动设置4大参数
		* 在src目录创建c3p0-config.xml
		
	5.Tomcat连接池
		* 在<Context>配置 -- 启动Tomcat，主页找
		* 需要把MySQL驱动包放到Tomcat/lib目录
		* 只能在Servlet/JSP代码


**事物加在哪一层**
	
	1.事物需要加在service层
	
## 元数据、DBUtils和编写案例 ##
	
	
**ThreadLocal事物的操作**
	
	1.目的：传递conn对象
		* 可以通过参数的方式传递
		* 通过ThreadLocal类把conn绑定到ThreadLocal中。
	
**元数据**
	
	1.SUN提供的一些规范，接口。
	2.元数据：可以获取数据库的基本的信息，表，字段的信息，参数的信息。
	3.作用：编写一些框架。
	4.元数据的分类
		* 数据库元数据
		* 参数元数据
		* 结果集元数据	
		
**数据库元数据**
	
	1.DataBaseMetaData  -- 数据库元数据
	2.谁创建数据库元数据？
		* 由Connection对象创建的
	
	3.数据库元数据获取的内容？
		* 获取url
		* 获取username
		* 获取驱动的名称
		* 获取主键
	
**参数元数据（ParameterMetaData）**
	
	1.作用：获取SQL语句中?的个数和类型（类型获取的不准确）
	2.PreparedStatement可以获取参数元数据
	3.方法
		* getParameterCount()   -- 获取参数的个数（获取?的个数）
		
		
**结果集元数据（ResultSetMetaData）**
	
	1.作用：获取结果集中的列的信息
	2.由ResultSet创建
	3.方法
		* getColumnCount() 返回resultset对象的列数
		* getColumnName(int column) 获得指定列的名称
		* getColumnTypeName(int column)获得指定列的类型 
	
	
**抽取通用的方法**
	
	1.编写。

**DBUtils工具类（框架）**
	
	1.commons-dbutils 是 Apache 组织提供的一个开源 JDBC工具类库，它是对JDBC的简单封装，学习成本极低，并且使用dbutils能极大简化jdbc编码的工作量，同时也不会影响程序的性能。因此dbutils成为很多不喜欢hibernate的公司的首选。
	
	2.导入jar包。commons-dbutils-1.4.jar
	
	3.核心的类	QueryRunner类 -- 做增删改查的操作
		* 方法
			* QueryRunner()
			* QueryRunner(DataSource ds) 

			* int update(String sql, Object... params) 
			* int update(Connection conn, String sql, Object... params) 

			*  <T> T query(String sql, ResultSetHandler<T> rsh, Object... params) 
			*  <T> T query(Connection conn, String sql, ResultSetHandler<T> rsh, Object... params) 
			
		* 组合
			* 没有事物
			* QueryRunner(DataSource ds)     -- 传入连接池（获取连接）
			* int update(String sql, Object... params) 
			* <T> T query(String sql, ResultSetHandler<T> rsh, Object... params) 
			
			
			* 和事物相关的（conn向下传递）
			* QueryRunner()
			* int update(Connection conn, String sql, Object... params) 
			* <T> T query(Connection conn, String sql, ResultSetHandler<T> rsh, Object... params) 
			
	4.ResultSetHandler接口，可以用户自己来实现，重写方法。9个实现类。
	5.DbUtils类
		* 和事物相关的方法
		*  static void 	rollbackAndClose(Connection conn) 
		*  static void 	rollback(Connection conn) 
		*  static void 	commitAndClose(Connection conn) 	


**QueryRunner类**	

	1.update(String sql,Object... obj)
	2.query(String sql ,ResultSetHandler reh,Object... ojb)

**ResultsetHandler的实现类**
	
	1.BeanHandler  			-- 把一条记录封装到JavaBean对象中
		id  username  money
		2   美美          10000	Account(2,"美美",10000)
	2.BeanListHandler			-- 把一条记录封装到JavaBean对象中，把多个JavaBean放入List集合中。
		Account(2,"美美",10000)
		Account(3,"小凤",10000)
		最终把这些JavaBean存放到List集合中
	
	
	3.ArrayHandler				-- 把一条记录封装到数组中
		2   美美          10000	[2,'美美',10000]
	4.ArrayListHandler,			-- 把一条记录封装到数组中，把数组存放在集合中
		[2,'美美',10000]
		[3,'小凤',10000]
		
	5.MapHandler				-- 一条记录封装到Map集合
		{id=2 username=美美 money=10000}
	6.MapListHandler			-- 一条记录封装到Map集合，把Map集合存放到集合中
		{id=2 username=小凤 money=10000}
	
	
	7.ScalarHandler			-- 封装count(*) 单行单列数据

	8.ColumnListHandler		-- 查询是一列数据，把一列数据封装到集合中。
		
	9.KeyedHandler			-- 把一条记录封装到一个map集合，把该map集合又存放在另一个map集合中。
	
	
	* 重点	BeanHandler 	BeanListHandler	ScalarHandler
		
		
**客户管理案例**
	
	1.目的：总结JDBC，和Servlet JSP结合到一起。
	2.开发中的一些小技巧。
	
	3.客户管理平台功能
		* 添加客户
		* 查询所有的客户的信息
		* 修改客户信息
		* 删除客户信息
		* 按条件查询
		* 分页查询数据
		
	4.准备环境
	5.Servlet + JSP +JavaBean + JDBC 
	6.数据库
		create database day19;
		use day19;
		create table t_customer(
			id varchar(40) primary key,
			username varchar(20),
			gender varchar(10),
			birthday varchar(20),
			cellphone varchar(20),
			email varchar(40),
			love varchar(100),
			type varchar(40)
		);		
		
	7.导入jar包。
		* MySLQ驱动包
		* BeanUtils包
		* JSTL标签库
		* DBUtils
		* c3p0
			
	8.创建包结构
		* cn.itcast.action
		* cn.itcast.service
		* cn.itcast.dao
		* cn.itcast.vo
		* cn.itcast.utils

	9.把工具类和配置文件复制过来
		

**添加客户的功能**
	
	1.form表单 -- 校验
	2.字段的名称和javaBean的属性和表单中name属性的值都是相同的。
	

