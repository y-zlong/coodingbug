��JDBC�������ݿ��Ƿ����ĲҰ���
����JDBC����MYSQL���ݿ�����ѯ���ݵĲ���ʱһֱ�����ָ����Ĵ��󣬼�¼�������ݴ���
--------------------------------------------------------------------- 
java.lang.ClassNotFoundException: com.mysql.jdbc.Driver()
	at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1352)
	at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1180)
	at java.lang.Class.forName0(Native Method)
-------------------------------------------------------------------
�ȿ�һ��ClassNotFoundException:com.mysql.jdbc.Driver()�Ĵ��󣬺����Դ��������
	com.mysql.jdbc.Driver������˸�"()"���±���
--------------------------------------------------------------------------	
com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Unknown database '3306/user'
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
----------------------------------------------------------------------------
�ȿӶ���������ʾ���£�
 
������ʾ����ȷ���˿ں�֮����Ӷ˿ڣ�ԭ������mysql�Ķ˿ںų����ˣ�jdbc:mysql://localhost:3306/userΪ��ȷд�����Ϸ�д��jdbc:mysql://localhost/3306/user


��������ֻ��˵�Ǵ��Ĵ��⵼�µģ�������������ǲȵ��Ϸ�û��һ��Ƣ����
��ӣ�

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

 
����һ��������û�����⣬û��һ�㱨��ļ��󣬵���ʲô��Ҳ��׼ȷ��
 jar�ļ�Ҳ�ڣ�Ҳ�Ѿ�buidpath�ˣ�Ȼ����ұ�����������ֵֹĴ���sql�쳣��trycach�����쳣����ָ���쳣�����˴���кü���������bug�ĺ��˹�һ���ֳ��ָ����쳣���������������Debug������2��Сʱ��
��������������ڣ����⻹�ǳ�����jar�ļ��ϣ�mysql�������Ѿ�����lib�ļ����£�Ҳ�Ѿ��ɹ����ص���Ŀ�У�һֱ��������쳣�Ĵ��󣬻��˸��Ͱ汾���������ǲ��б�������

������⣺�����ǳ���tomcat��eclipse�д�����̬web��Ŀ��jar�ļ����Զ�biudpath����Ŀ�У�����û����ģ������ڼ���mysql������ʱ��Class.forName("com.mysql.jdbc.driver");�������߲�������ַ���������jar�ļ�����Ŀ�У����ֻ���ֶ�����mysql����jar����tomcat��libĿ¼�²Ÿ㶨��
