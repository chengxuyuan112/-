## 课程回顾JDBC第一天 ##
	
	1.JDBC的简介：SUN公司提供，操作数据库的一套规范（一组接口），实现类是数据库的生产商。
	2.驱动包提供的，使用什么数据库，相应的驱动包（jar包）
	3.JDBC开发的入门步骤
		* 注册驱动	 -- DriverManager
		* 获取链接	 -- Connection
		* 获取执行SQL语句 -- Statement （PreparedStatement -- 预编译）
		* 如果有查询结果 -- ResultSet 
		* 释放资源
		
	4.DriverManager
		* 注册驱动
			* Class.forName("com.mysql.jdbc.Driver");
		* 获取链接
			* getConnection(url,username,password)
			* url: jdbc:mysql://localhost:3306/数据库
		
	5.Connection接口
		* 获取执行SQL对象
			* Statement
			* PreparedStatement	-- 预编译 防止SQL注入
		* 管理事物
			* set autocommit(false)  -- 设置事物不自动提交
			* commit()			-- 提交
			* rollback()			-- 回滚
	
	6.Statement接口
		* 执行SQL
			* executeQuery(sql)		-- 查询语句（ResultSet）
			* executeUpdate(sql)    	-- 执行增删改语句（int）
		* 批处理
		
	7.ResultSet接口
		* 有一个游标默认指向第一行数据之前。
		* 默认只能向下移动（rs.next()）
		* 获取数据	（getXXX(int)  getXXX(String)）
		* 设置滚动的结果集
		
	8.释放资源
		* 步骤
		
	9.DAO的开发模式
		* SUN提供的开发模式
		* WEB层 -- 业务层 -- 持久层
		
	10.登陆的案例（SQL注入）
		* 产生：拼接SQL的字符串
		* PreparedStatement -- 编写SQL语句的时候使用?占位符代替的参数，预编译
		
	11.PreparedStatement完成DAO单表的增删改查的操作
	12.MyJdbcUtil的工具类
		* 注册驱动
		* 获取链接
		* 释放资源
	
	13.大数据
	14.批处理


## 事物和数据库连接池 ##

**事物**
	
	1.什么是事物：一组逻辑上的操作，要么全都成功，要么全都失败。
	2.模拟事物的操作
		create database day18;
		use day18;
		create table t_account(
			id int primary key auto_increment,
			username varchar(30),
			money double
		);
		
		insert into t_account values (null,'聪聪',10000);
		insert into t_account values (null,'美美',10000);
		insert into t_account values (null,'小凤',10000);
		insert into t_account values (null,'如花',10000);
		insert into t_account values (null,'邦邦',10000);

**MySQL使用事物**
	
	1.在MySQL使用命令
		start transaction;	-- 开启一个事物
			sql1...
			sql2...
		commit;			-- 把事物提交了
		rollback;			-- 把事物回滚了
		
	2.MySQL数据库中国，事物是默认提交的。
		* show variables like '%commit%'; 可以通过该命令查看数据库是事物是否是默认提交的。
		* 设置MySQL不让默认提交的 （set autocommit = off或者0;）
			sql1..
			sql2..
		commit;			-- 把事物提交了
		rollback;			-- 把事物回滚了
	
	3.进行测试
		* 第一种方式
			* 开启事物
				* start transaction;
			* update t_account set money = money - 1000 where username = '聪聪';
			* update t_account set money = money + 1000 where username = '美美';
			* 提交或者回滚
				* commit;  或者 rollback;

		* 测试第二种
			* 设置MySQL不让默认提交
				* set autocommit = 0或者off;
			* update t_account set money = money - 1000 where username = '聪聪';
			* update t_account set money = money + 1000 where username = '美美';
			* 提交或者回滚
				* commit;  或者 rollback;
		
	
**JDBC代码中使用事物**
	
	1.哪个对象可以操作事物？使用Connection接口操作事物。
		* void setAutoCommit(boolean autoCommit) 	-- 设置默认不默认提交
		* void commit() 						-- 提交事物
		* void rollback()  						-- 回滚事物
	
	
**保存点**
	
	1.Savepoint sp = conn.setSavepoint();
	2.conn.rollback(sp);
	3.回滚到保存点的位置，事物没有结束，必须还要提交或者回滚事物才结束。
	
	
**事物的特性（面试）**
	
	1.原子性	：强调事物的不可分隔特性。
	2.一致性	：强调数据的完整性保存一致。
	3.隔离性	：强调是的一个事物的执行，不应该受到另外的事物的打扰。
	4.持久性	：强调是事物结束（提交或者回滚），数据永久的保存到数据库中。


**不考虑隔离性，产生一些问题**
	
	1.脏读		：一个事物读取到了另一个事物未提交的数据
	2.不可重复读	：一个事物度读取了（10000），另外一个事物做了修改的操作，提交了。第一个事物又做了一次查询，这次的查询结果变成了12000，产生了不可重复读。只要是在一个事物中，多次的查询结果必须是一致的。
	3.虚读（幻读）	：一个事物度读取了（10000），另外一个事物做了插入的操作，提交了。第一个事物又做了一次查询，这次的查询结果变成了12000，产生了不可重复读。只要是在一个事物中，多次的查询结果必须是一致的。
	

**设置事物的隔离级别**
	
	1.read uncommitted		：未提交读 脏读、不可重复读、虚读都可能发生
	2.read committed		：提交读	避免脏读，但是不可重复读和虚读有可能发生
	3.repeatable read		：可重复读	避免脏读和不可重复读，但是虚读有可能发生
	4.serializable			：避免脏读、不可重复读、虚读
	
	* 安全性：read uncommitted	< read committed < repeatable read < serializable
	* 效率：read uncommitted > read committed > repeatable read > serializable
	* 在数据库的设置隔离级别中，根据安全性和效率考虑，肯定不会用read uncommitted和serializable
	
	* 在MySQL中默认的隔离级别：repeatable read
	* 在Oracle中默认的隔离级别：read committed
	
	
**演示上述过程的发生和解决方案**
	
	* 查询数据库的隔离级别	select @@tx_isolation
	* 设置数据库的隔离级别	set session  transaction isolation level  XXX; （XXX代表上述4种级别）
	
	1.测试脏读
	2.开启两个窗口A窗口和B窗口
	3.分别登陆MySQL数据库，使用day18数据库。
	4.设置隔离级别
		* set session  transaction isolation level  read uncommitted;
	5.在AB两个窗口中分别开启事物
		* start transaction;
	
	6.在B窗口中完成一些操作（转账的操作）
		* update t_account set money = money - 1000 where username = '聪聪';
		* update t_account set money = money + 1000 where username = '美美';
		* 提交了吗？没有提交！！
	
	7.在A窗口做查询的操作
		* select * from t_account;
	
	8.产生了脏读
		

**避免脏读和演示不可重复读**
	
	1.设置A窗口的隔离级别
		* set session  transaction isolation level  read committed;
	2.在AB两个窗口中分别开启事物
		* start transaction;
		
	3.在B窗口中完成一些操作（转账的操作）
		* update t_account set money = money - 1000 where username = '聪聪';
		* update t_account set money = money + 1000 where username = '美美';

	4.在A窗口中
		* select * from t_account;

	5.避免了脏读，产生了不可重复读


**避免不可重复读**
	
	1.设置A窗口的隔离级别
		* set session  transaction isolation level  repeatable read;
	2.在AB两个窗口中分别开启事物
		* start transaction;
	3.在B窗口中完成一些操作（转账的操作）
		* update t_account set money = money - 1000 where username = '聪聪';
		* update t_account set money = money + 1000 where username = '美美';
	4.在A窗口中
		* select * from t_account;
	5.在A窗口中，结束事物，在做查询
		* 查询的结果是最新的结果

	6.总结：避免了脏读和不可重复读


**演示serializable隔离级别**
	
	1.设置A窗口的隔离级别
		* set session  transaction isolation level  serializable;
	2.在AB两个窗口中分别开启事物
		* start transaction;
	3.在B窗口中添加一条数据
		* insert into t_account values (null,'冠西',10000);
	4.在A窗口中做查询的操作
		* select * from t_account;
	

**在JDBC中设置隔离级别（了解）**
	
	
	* 在在Connection接口中提供了常量，这写常量分别代表不同的隔离级别
		static int TRANSACTION_READ_UNCOMMITTED  ：指示可以发生脏读 (dirty read)、不可重复读和虚读 (phantom read) 的常量。 
		static int TRANSACTION_READ_COMMITTED  ：指示不可以发生脏读的常量；不可重复读和虚读可以发生。 
		static int TRANSACTION_REPEATABLE_READ  ：指示不可以发生脏读和不可重复读的常量；虚读可以发生。 
		static int TRANSACTION_SERIALIZABLE  ：指示不可以发生脏读、不可重复读和虚读的常量。
	
	* 通过方法来进行设置，在Connection接口提供了一个方法
		* void setTransactionIsolation(int level) 
	

**数据库连接池**
	
	1.为什么要有连接池：创建和销毁是销毁时间的，连接池初始化的时候创建几个连接。
	2、连接池池参数（开发自定义的连接池）
		初始大小：10个
		最小空闲连接数：3个
		增量：一次创建的最小单位（5个）
		最大空闲连接数：12个
		最大连接数：20个
		最大的等待时间：1000毫秒
		
	2.使用连接池设置4个大参数（用户给出）
		* 驱动类的包名+类	com.mysql.jdbc.Driver
		* url				jdbc:mysql:///day18
		* username		root
		* passowrd		root
		
	3.怎么使用连接池对象
		* 获取链接的方法


**自定义数据库连接池**
	
	1.实现DataSource接口
	2.重写几个方法。
		
	3.自定义的数据库的连接池存在哪些问题？
		* 开源的连接池解决这一类的问题。
	4.不能使用DataSource接口构造
	5.归还链接的方法
		public void addBack(Connection conn){
			// 添加到集合中
			connlist.add(conn);
		}
	
	6.如果能把close()增强了，变成归还的方法
	
	
**装饰者模式（增强某个类中的方法）**
	
	1.目的：增强某个类中的某个方法，有哪些方式？
		* 继承 			：你需要知道它爹是谁？
		* 装饰者的模式（增强Connection接口的实现类（还不知道名字，起个名字 小聪），中的close()方法）
			* 增强的类（自己来写）和被增强的类（小聪）必须实现相同的接口。
			* 在增强的类中能获取到被增强类的引用。
			
		* 动态代理
		
	
**开源数据库连接池**

	1.这些开源的连接池已经增强了close方法，全部都是归还。

**DBCP连接池**
	
	1.导入jar包。
		* commons-dbcp-1.4.jar
		* commons-pool-1.5.6.jar
	
	2.开发入门
		* 需要手动设置链接的参数（必须得设置4大参数）
			* BasicDataSource 
		* 读取配置文件的方式（properties文件）
			* BasicDataSourceFactory
	
	3.其他的配置文件（课后看）
	

**C3P0连接池**
	
	1.导入jar包
		* c3p0-0.9.1.2.jar
	2.使用
		* ComboPooledDataSource 
		* 设置参数
		
	3.C3P0默认读取一个XML的配置文件（c3p0-config.xml），文件存放在src的目录下。
	4.在c3p0-config.xml文件编写配置（4大参数）
	
	
**Tomcat内置连接池（配置）**
	
	1.介绍Tomcat内置在连接池。Tomcat管理连接池，只要配置信息。
	2.介绍<Context>的配置某个位置
		tomcat/conf/context.xml  -- 被tomcat下的所有的虚拟主机、虚拟路径访问.
		 tomcat/conf/Catalina/localhost/context.xml -- 被tomcat下的localhost虚拟主机下的所有虚拟路径访问.
		工程下/META-INF/context.xml -- 被当前工程所访问.
		
	3.在context.xml需要配置什么内容？
		<Context>
		 	 <Resource name="jdbc/EmployeeDB" auth="Container"
		            type="javax.sql.DataSource" username="root" password="123"
		            driverClassName="com.mysql.jdbc.Driver" url="jdbc:mysql:///day18"
		            maxActive="8" maxIdle="4"/>
		</Context>
		
	4.将mysql驱动包copy到tomcat/lib下
	5.类必须是运作在Tomcat容器中才可以访问.Servlet/JSP
	
	
**事物加在service或者DAO层**
	
	1.事物的解决方案有两种
		* 通过参数的方式向下传递
		* 把conn绑定到当前的线程中
	
	2.ThreadLocal类底层维护Map集合。默认把当前的线程作为Map的key。