用JDBC连接数据库是发生的惨案：
在用JDBC连接MYSQL数据库做查询数据的操作时一直报各种各样的错误，记录以下数据错误：
--------------------------------------------------------------------- 
java.lang.ClassNotFoundException: com.mysql.jdbc.Driver()
	at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1352)
	at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1180)
	at java.lang.Class.forName0(Native Method)
-------------------------------------------------------------------
踩坑一：ClassNotFoundException:com.mysql.jdbc.Driver()的错误，很明显错误就是在
	com.mysql.jdbc.Driver后面加了个"()"导致报错
--------------------------------------------------------------------------	
com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Unknown database '3306/user'
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
----------------------------------------------------------------------------
踩坑二：报错提示如下：
 
报错提示，在确定端口号之后添加端口，原来是在mysql的端口号出错了，jdbc:mysql://localhost:3306/user为正确写法，老夫写成jdbc:mysql://localhost/3306/user


这两个坑只能说是粗心大意导致的，接下来这个坑是踩的老夫没有一点脾气，
大坑：

Class.forName(driver); 
			connection = DriverManager.getConnection(url, user, password);
			String sql ="select id,name,number,dress from student";
			ps = connection.prepareStatement(sql);
			resultSet = ps.executeQuery();
			while(resultSet.next()) {
				int stu_id = resultSet.getInt(1);
				String stu_name = resultSet.getString(2);
				int stu_num = resultSet.getInt(3);
				String stu_dress = resultSet.getString(4);
				studentBean student = new studentBean(stu_id, stu_name, stu_num, stu_dress);
				stu.add(student);
				}
			} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
			}finally {
				if (resultSet != null) {
					try {
						resultSet.close();
					} catch (SQLException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();

 
这样一看代码是没有问题，没有一点报错的迹象，倒包什么的也都准确，
 jar文件也在，也已经buidpath了，然后给我报错，各种奇奇怪怪的错误，sql异常，trycach语句块异常，空指针异常，报了大概有好几个错，悲剧bug改好了过一会又出现各种异常报错，里里外外各种Debug折腾了2个小时，
最后发现了问题所在：问题还是出在了jar文件上，mysql的驱动已经放在lib文件夹下，也已经成功加载到项目中，一直报类加载异常的错误，换了个低版本的驱动还是不行报错依旧

解决问题：问题是出在tomcat，eclipse中创建动态web项目中jar文件会自动biudpath到项目中，这是没问题的，但是在加载mysql驱动的时候Class.forName("com.mysql.jdbc.driver");开发工具不会根据字符串来查找jar文件到项目中，最后只能手动复制mysql驱动jar报到tomcat的lib目录下才搞定。
