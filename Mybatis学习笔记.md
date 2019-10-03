#Mybatis学习笔记
---
##创建mybatis的基本原理
```
public class MybatisTest {

    public static void main(String[] args) throws Exception {
        //1.读取配置文件 (1.使用类加载器，只能读取类路径的配置文件 2. 使用ServletContext对象的getRealPath) 
        InputStream inputStream = Resources.getResourceAsStream("SqlMapConfig.xml");
//2.创建SqlSessionFactory工厂 builder使用了建造者模式
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        //工厂模式（解耦）
        SqlSessionFactory factory = builder.build(inputStream);
//3.使用工厂创建SqlSession
        SqlSession session =factory.openSession();
//4.使用SqlSession创建dao接口的代理对象
        IUserDao userDao =session.getMapper(IUserDao.class);
//5.使用代理对象的方法
        List<User> users = userDao.findAll();
        for (User user:users
             ) {
            System.out.println(user);
        }
//6.释放资源
        session.close();
        inputStream.close();
    }
}
```

###自定义mybatis的分析：
mybatis在使用代理dao的方式实现CRUD做了什么事？
		1.创建代理对象
		2.在代理对象中调用selectList
		
```
SqlMapConfig.xml
链接数据库，创建Connection对象
<property name="driver" value="com.mysql.cj.jdbc.Driver"/>
<property name="url" value="jdbc:mysql://localhost:3306/eesy"/>
<property name="username" value="root"/>
<property name="password" value="123456"/>
映射配置信息
<mappers>
<!--<mapper resource="com/charon/myabtis/dao/IUserDao.xml"/>-->
    <mapper class="com.charon.mybatis.dao.IUserDao"/>
</mappers>
```

+ 1.根据配置文件获取Connection对象  
注册驱动，获取连接  
+ 2.获取预处理对象PrepareStatement  
此时需要sql语句  
+ 3.执行查询  
ResultSet resultSet = preparedStatement.excute();  
+ 4.遍历结果集用于封装
  
```
List<E> list = new ArrayList();
while(resultSet.next()){
	E element = xxx;
	进行封装，把每个rs内容都加到element中
	list.add(element);
}
```
+ 5.返回list  
return list;  

![mybatis01.png](/Users/zhangwenmeng/Documents/markdown/images/mybatis01.png)


###mybatis流程图
![mybatis流程图.png](/Users/zhangwenmeng/Documents/markdown/images/mybatis流程图.png)


###OGNL表达式
Object Graphic Navigation Language 对象图 导航语言

它的通过对象的取值方法来获取数据，在写法上把get省略了：

	如获取用户名称：

	类中的写法：user.getUserName()
	OGNL中写法：user.username
	
mybaits之所以能够直接写username 而不用user.username。是因为parameterType 中已经提供了属性所属的类，所以此时不需要写对象名

```
配置 查询结果的列名与实体列的属性名的对应关系
<resultMap id="userMap" type="com.charon.mybatis.demain.User">
	<!-- 配置主键属性-->
	<id property="userId" column="id"></id>
	<!-- 非主键属性配置-->
	<result property="userName" column="username"></result>
	<result property="userAddress" column="address"></result>
	<result property="userSex" column="sex"></result>
	<result property="userBirthday" column="birthday"></result>
</resultMap>

<select id="findAll" resultMap="userMap">
	select * from user;
</select>
```
