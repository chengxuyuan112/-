## 课程回顾：MySQL ##

	1.聚集函数
		* count
		* sum
		* avg
		* max
		* min
		
	2.分组
		* 默认是一组，可以关键字group by 字段 分组。
		* 需要分组和聚集一起使用。
		* 在group by having 条件（后面可以使用聚集函数）
	
	3.小结
		* select ... from ... where ... group by ... having ... order by
	
	4.重置密码，数据库备份和恢复
	5.数据库的主键
		* 主键代表记录的唯一标识。
		* 关键字	primary key 
		* 特点
			* 唯一
			* 非空
			* 被引用
	
	6.主键的自增长
		* 关键字 auto_increment
		* int bigint

	7.唯一和非空
		* unique
		* not null
		
	8.外键
		* 作用：保存数据的完整性。
		* 关键字：foreign key (dno) references dept (did);foreign key (dno) references dept (did);
	
	9.关系（创建表的时候）
		* 一对多	：在多方的表中添加一个字段，声明成外键。指向一方表的主键。
		* 多对多	：创建中间表（把一个多对多拆成两个一对多），中间表至少2个字段，作为外键。指向一方表的主键。
		* 一对一	：主键对应 唯一外键对应

	10.多表的查询
		* 内链接
			* 普通内连接
				* inner join ... on
			* 隐式内连接
				* select ... from where 条件;
		* 外链接
			* 左链接
				* left outer join .. on
			* 右链接
				* right outer join ... on
	
	11.子查询
		* 一个语句有多个select关键字


## JDBC的第一天 ##

**JDBC的简介和快速入门**

**JDBC的简介**
	
	1.MySQL驱动包（JDBC接口的实现类，就是一个jar包）。
	2.JDBC是SUN公司提出一套规范，就是一组接口。这写接口的实现类是由各个数据库的生产商提供的。
	3.JDBC的快速入门的开发有一些的开发步骤
	
	
**快速入门**
	
	1.把MySQL驱动包导入到工程中。
	2.创建数据库和表结构，手动添加数据。查询的快速入门。
		create database day17;
		use day17;
		create table t_user(
			id int primary key auto_increment,
			username varchar(30),
			password varchar(30)
		);
		insert into t_user values (null,'美美','123');
		insert into t_user values (null,'聪聪','456');
		insert into t_user values (null,'aaa','789');
		insert into t_user values (null,'bbb','111');
	
	3.可以编写java代码。
		
	
**讲解类和接口**

**DriverManage类**

	1.注册驱动
		* DriverManager.registerDriver(new Driver());
		* 其实这个方法有两点不好
			* 过于依赖MySQL驱动包。
			* 注册驱动的代码实际上执行了2次。
		* 想使用一种方式解决上述两个问题。	
			* Class.forName("com.mysql.jdbc.Driver");
			
	2.获取链接对象
		* Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/day17", "root", "root");
		* url			-- 数据库的链接地址	
			* jdbc:mysql://localhost:3306/day17
				* jdbc			：数据库之间传输数据的协议
				* mysql			：jdbc协议的子协议。
				* localhost		：服务器的地址
				* 3306			：默认的端口号
				* day17			：数据库的名称
			* 注意：如果你要链接你自己电脑上的数据库，简写的方式
				* jdbc:mysql:///day17
				
		* user			-- 用户名
		* password		-- 你的数据库的密码
	

**Connection对象（链接）**
	
	1.创建执行SQL语句的对象
		* Statement createStatement() 						-- 获取执行SQL语句对象
		* PreparedStatement prepareStatement(String sql)  	-- 获取执行SQL语句对象，预编译SQL语句，防止SQL注入
		* CallableStatement prepareCall(String sql) 		-- 获取执行SQL语句对象，执行存储过程
		
	2.管理事物
		* 事物：一组逻辑上的操作。
		* void setAutoCommit(boolean autoCommit) 			-- 设置事物自动提交
		* void commit() 									-- 提交事物
 		* void rollback() 									-- 回滚事物

**Statement对象**
	
	1.执行SQL语句
		* ResultSet executeQuery(String sql) 		-- 执行查询语句（select语句）
		* int executeUpdate(String sql) 			-- 执行增删改的操作（insert update delete）
		* boolean execute(String sql) 				-- 执行任意的sql（如果第一个结果为 ResultSet 对象，则返回 true；如果其为更新计数或者不存在任何结果，则返回 false ）
		
	2.执行批处理
		* 可以一批插入或者删除数据。（表格）
		* void addBatch(String sql) 				-- 把SQL语句添加批处理中
		* int[] executeBatch() 						-- 执行批处理
		* void clearBatch() 						-- 清除批处理
	
**ResultSet对象**
	
	1.执行查询语句，才有结果集。
	2.游标的默认位置在第一行数据之前
	3.调用rs.next()方法，游标判断是否有下一行数据，如果有，移动到下一行。可以通过getXXX()方法获取该行的数据。
	4.游标默认只能向下移动，如果滚动，可以设置滚动的结果集。
	
	5.getXXX()	这些方法都是重载的int（字段的位置）   String（字段的名称）
	6.getObject()	获取任意类型的数据（强转）
	
**设置滚动的结果集（了解）**
	
	1.如果想使用滚动结果集，需要该方法来生成Statement对象
		* Statement createStatement(int resultSetType, int resultSetConcurrency) 
			* resultSetType				-- 结果集类型
			* resultSetConcurrency		-- 结果集并发策略

	* 结果集的类型
		* TYPE_FORWARD_ONLY 			-- 只能向下
		* TYPE_SCROLL_INSENSITIVE 		-- 可以滚动，不能修改
		* TYPE_SCROLL_SENSITIVE 		-- 可以滚动，又可以修改
		
	* 结果集并发策略
		* CONCUR_READ_ONLY 				-- 只能读
		* CONCUR_UPDATABLE 				-- 可以修改
	
	* 把类型和并发策略，组合。	
		* TYPE_FORWARD_ONLY   	  		CONCUR_READ_ONLY	默认的结果集，只能向下，不能修改记录
		* TYPE_SCROLL_INSENSITIVE	  	CONCUR_READ_ONLY	可以滚动，不能修改记录
		* TYPE_SCROLL_SENSITIVE     	CONCUR_UPDATABLE	可以滚动，也可以修改记录
	

**释放资源**
	
	1.connection链接对象，晚创建早释放。
	2.释放标准的代码
		if(rs != null){
			try {
				rs.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
			rs = null;
		}
	
	
**使用JDBC来完成增删改查的操作（必须会写）**
	
	1.操作t_user的表，完成增删改查的功能。
	
**封装工具类（一直修改）（自己封装）**
	
	1.封装的过程。
	
**介绍DAO模式**
	
	DAO模式就是持久层的一种解决方案，封装对于数据源及其数据的单个操作，需要提供一组接口，供业务层访问，业务调用DAO的代码时候需要传递一个对象。
	

**重写登陆的案例**
	
	1.搭建环境
	2.导入jar包（MySQL驱动包，BeanUtils包，JSTL的包）
	3.代码编写

**SQL注入的问题（漏洞）**
	
	1.在文本框输入一些特殊的字符，在已知用户名的情况下，输入用户登陆进去。
	2.xxx ' or  ' 1=1   或者  xxx ' –  
	3.咱们编写的SQL语句	select * from t_user where username = '' and password = '';
		* select * from t_user where username = 'bbb ' or ' 1=1 ' and password = '任意';
		* select * from t_user where username = 'bbb ' -– ' and password = '';
	
	4.根本原因就是因为拼接了参数。
	5.解决这种问题
		* 前台校验
		* 后台的验证（使用PreparedStatement对象）
	6.PreparedStatement来完成操作
		* 编写SQL语句，SQ	L语句的参数使用?代替
		* 预编译SQL语句	conn.prepareStatement(sql)
		* 设置参数的真正的值	stmt.setString(1,值)		1:代表?的位置
		* 执行SQL（不用传入SQL语句）
		

**封装单个表的操作**

	1.必须会写
	

**Text Blob大数据（了解）**
	
	1.创建数据库的表结构
	create table mytext(
		id int primary key auto_increment,
		mydata MEDIUMTEXT
	);
	

	create table myblob(
		id int primary key auto_increment,
		mydata MEDIUMBLOB
	);
	
	
**批处理（了解）**

	1.批量插入和批量删除。
	
	* void addBatch(String sql) 				-- 把SQL语句添加批处理中
	* int[] executeBatch() 						-- 执行批处理
	* void clearBatch() 						-- 清除批处理
	
	* insert into xxx values (null,"值");	如果使用Statement对象执行SQL语句。
	* insert into xxx values (null,?);		先发送SQL到服务器端，预编译。设置值。使用PreparedStatement对象

	
	create table mybatch(
		id int primary key auto_increment,
		name varchar(20)
	);

	

**作业**
	
	1.注册功能重写