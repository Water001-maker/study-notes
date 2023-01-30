typora-root-url: SpringPicture
typora-copy-images-to: SpringPicture

# 1.Spring

## 1.1简介

- Spring : 春天 --->给软件行业带来了春天

- 2002年，Rod Jahnson首次推出了Spring框架雏形interface21框架。

- 2004年3月24日，Spring框架以interface21框架为基础，经过重新设计，发布了1.0正式版。

- 很难想象Rod Johnson的学历 , 他是悉尼大学的博士，然而他的专业不是计算机，而是音乐学。

- Spring理念 : 使现有技术更加实用 . 本身就是一个大杂烩 , 整合现有的框架技术



- SSH：Struct2+Spring+Hibernate
- SSM：Spring+SpringMVC+MyBatis

官网 : http://spring.io/

官方下载地址 : https://repo.spring.io/libs-release-local/org/springframework/spring/

GitHub : https://github.com/spring-projects

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.9</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.9</version>
</dependency>
```



## 1.2优点

- Spring是一个开源免费的框架 , 容器  .

- Spring是一个轻量级的, 非侵入式的框架

- **控制反转（ IOC）  , 面向切面 （AOP）！**

- 对事务的支持 , 对框架的支持



*一句话概括：**Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器（框架）。***



## 1.3组成

![image-20211010133752503](SpringPicture.assets\image-20211010133752503.png)

Spring 框架是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器定义了创建、配置和管理 bean 的方式 .

![image-20211010133219274](SpringPicture.assets\image-20211010133219274.png)

组成 Spring 框架的每个模块（或组件）都可以单独存在，或者与其他一个或多个模块联合实现。每个模块的功能如下：

- **核心容器**：核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 BeanFactory，它是**工厂模式**的实现。BeanFactory 使用*控制反转*（IOC） 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。
- **Spring 上下文**：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
- **Spring AOP**：通过配置管理特性，Spring AOP 模块直接将面向切面的编程功能 , 集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理任何支持 AOP的对象。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中。
- **Spring DAO**：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。
- **Spring ORM**：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。
- **Spring Web 模块**：Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
- **Spring MVC 框架**：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。

## 1.4拓展

**Spring Boot与Spring Cloud**

- Spring Boot 是 Spring 的一套快速配置脚手架，可以基于Spring Boot 快速开发单个微服务;
- Spring Cloud是基于Spring Boot实现的；
- Spring Boot专注于快速、方便集成的单个微服务个体，Spring Cloud关注全局的服务治理框架；
- Spring Boot使用了约束优于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置 , Spring Cloud很大的一部分是基于Spring Boot来实现，Spring Boot可以离开Spring Cloud独立使用开发项目，但是Spring Cloud离不开Spring Boot，属于依赖的关系。
- SpringBoot在SpringClound中起到了承上启下的作用，如果你要学习SpringCloud必须要学习SpringBoot。

![image-20211010134006422](SpringPicture.assets\image-20211010134006422.png)

- Spring Boot

  - 一个快速开发的脚手架

  - 基于SpringBoot可以快速的开发单个微服务

  - 约定大于配置

- Spring Cloud

  - SpringCloud是基于SpringBoot实现的

大多数公司都在使用SpringBoot开发，学习SpringBoot的前提是，熟练掌握Spring及SpringMVC！（承上启下）

**弊端：发展了太久，违背了原来的理念！配置十分繁琐，人称“配置地狱”**

# 2.IOC理论推导

**控制反转、依赖注入、DI**

## 2.1IoC基础

新建一个空白的maven项目

> 分析实现  

我们先用我们原来的方式写一段代码 .

1、先写一个UserDao接口

```java
public interface UserDao {
   public void getUser();
}
```

2、再去写Dao的实现类

```java
public class UserDaoImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("获取用户数据");
  }
}
```

3、然后去写UserService的接口

```java
public interface UserService {
   public void getUser();
}
```

4、最后写Service的实现类

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao = new UserDaoImpl();

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

5、测试一下

```java
@Test
public void test(){
   UserService service = new UserServiceImpl();
   service.getUser();
}
```

这是我们原来的方式 , 开始大家也都是这么去写的对吧 . 那我们现在修改一下 .

6、把Userdao的实现类增加一个 .

```java
public class UserDaoMySqlImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("MySql获取用户数据");
  }
}
```

7、紧接着我们要去使用MySql的话 , 我们就需要去service实现类里面修改对应的实现

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao = new UserDaoMySqlImpl();

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

8、在假设, 我们再增加一个Userdao的实现类 .

```java
public class UserDaoOracleImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("Oracle获取用户数据");
  }
}
```

那么我们要使用Oracle , 又需要去service实现类里面修改对应的实现 . 假设我们的这种需求非常大 , 这种方式就根本不适用了, 甚至反人类对吧 , 每次变动 , 都需要修改大量代码 . 这种设计的耦合性太高了, 牵一发而动全身 .

![image-20211010151329250](SpringPicture.assets\image-20211010151329250.png)

**那我们如何去解决呢 ?** 

我们可以在需要用到他的地方 , 不去实现它 , 而是留出一个接口 , 利用set , 我们去代码里修改下 .

```java
public class UserServiceImpl implements UserSerice {
    private UserDao userDao;

    //利用set进行动态实现值的注入
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void getUser() {
        userDao.getUser();
    }
}
```

现在去我们的测试类里 , 进行测试 ;

```java
    @Test
    public void UserTest(){

        //用户实际调用的是业务层，dao层他们不需要接触
        UserServiceImpl userService = new UserServiceImpl();

        userService.setUserDao(new UserOracleImpl());
        userService.getUser();

        userService.setUserDao(new UserDaoMySqlImpl());
        userService.getUser();

        userService.setUserDao(new UserDaoImpl());
        userService.getUser();

    }
```

以前所有东西都是由程序去进行控制创建 , 而现在是由我们自行控制创建对象 , 把主动权交给了调用者 . 程序不用去管怎么创建，怎么实现了，它只负责提供一个接口 。(Service中只提供set方法，方法调用交给Controller去调用)

这种思想，从本质上解决了问题，我们程序员不再去管理对象的创建了，更多的去关注业务的实现，耦合性大大降低，这也就是IOC的原型 !

之前：

![image-20211010153219240](SpringPicture.assets\image-20211010153219240.png)

现在：

![image-20211010153249618](SpringPicture.assets\image-20211010153249618.png)



> IOC本质

**控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法**，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

![image-20211010153434657](SpringPicture.assets\image-20211010153434657.png)

**IoC是Spring框架的核心内容**，使用多种方式完美的实现了IoC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现IoC。

Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从Ioc容器中取出需要的对象。

![image-20211010153602864](SpringPicture.assets\image-20211010153602864.png)

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）。**

# 3.HelloSpring

## 3.1导入Jar包

注 : spring 需要导入commons-logging进行日志记录 . 我们利用maven , 他会自动下载对应的依赖项 .

```xml
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>5.1.10.RELEASE</version>
</dependency>
```

## 3.2编写代码

1、编写一个Hello实体类

```java
public class Hello {
    private String str;

    public String getStr() {
        return str;
    }
    public void setStr(String str) {
        this.str = str;
    }
    @Override
    public String toString() {
        return "Hello{" +
                "str='" + str + '\'' +
                '}';
    }
}
```

2、编写我们的spring文件 , 这里我们命名为beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--bean就是java对象 , 由Spring创建和管理-->
    <bean id="hello" class="com.SpringTest.pojo.Hello">
        <property name="str" value="Spring"/>
    </bean>

</beans>
```

3、我们可以去进行测试了 .

```java
    @Test
    public void helloTest(){
        //获取Spring的上下文对象，解析beans.xml文件 , 生成管理相应的Bean对象
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        Hello hello = (Hello)context.getBean("hello");
        System.out.println(hello.toString());

    }
```

> 思考

- Hello 对象是谁创建的 ?  hello 对象是由Spring创建的
- Hello 对象的属性是怎么设置的 ?  hello 对象的属性是由Spring容器设置的

这个过程就叫控制反转 :

- 控制 : 谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制创建的 , 使用Spring后 , 对象是由Spring来创建的
- 反转 : 程序本身不创建对象 , 而变成被动的接收对象 .

依赖注入 : 就是利用**set方法**来进行注入的.

IOC是一种编程思想，由主动的编程变成被动的接收

可以通过newClassPathXmlApplicationContext去浏览一下底层源码 .

## 3.3修改案例一

我们在案例一中， 新增一个Spring配置文件beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--bean就是java对象 , 由Spring创建和管理-->
    <bean id="mysqlImpl" class="com.SpringTest.Dao.UserDaoMySqlImpl"/>
    <bean id="daoImpl" class="com.SpringTest.Dao.UserDaoImpl"/>
    <bean id="oracleImpl" class="com.SpringTest.Dao.UserOracleImpl"/>

    <bean id="UserServiceImpl" class="com.SpringTest.Service.UserServiceImpl">
        <!--
        ref 引用Spring容器中创建好的对象
        value 具体的值，基本类型数据
        -->
        <property name="userDao" ref="mysqlImpl"/>
    </bean>

</beans>
```

测试！

```java
    @Test
    public void UserTest() {

        //获取ApplicationContext;
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        UserServiceImpl userServiceImpl = (UserServiceImpl) context.getBean("UserServiceImpl");

        userServiceImpl.getUser();

    }
```

OK , 到了现在 , 我们彻底不用再程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改 , 所谓的IOC,一句话搞定 : **对象由Spring 来创建 , 管理 , 装配 !** 

# 4.IOC创建对象方式

## 4.1通过无参构造方法来创建

1、User.java

```java
public class User {

    private String name;

    public User() {
        System.out.println("User的无参构造");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

2、beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.SpringTest.pojo.User">
        <property name="name" value="threewater"/>
    </bean>

</beans>
```

3、测试类

```java
    @Test
    public void UserTest() {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //在执行getBean的时候, user已经创建好了 , 通过无参构造
        User user = (User) context.getBean("user");
        //调用对象的方法 .
        System.out.println(user.toString());

    }
```

结果可以发现，在调用show方法之前，User对象已经通过无参构造初始化了！

## 4.2通过有参构造方法来创建

1、UserT . java

```java
public class User {

    private String name;
    
    public User(String name) {
        System.out.println("User的有参构造");
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

2、beans.xml 有三种方式编写

```xml
    <!--有参构造-->
    <!--方法一：下标赋值-->
    <bean id="user" class="com.SpringTest.pojo.User">
        <constructor-arg index="0" value="threewater"/>
    </bean>

    <!--方法二：根据数据类型赋值（不建议使用）-->
    <bean id="user" class="com.SpringTest.pojo.User">
        <constructor-arg type="java.lang.String" value="threewater"/>
    </bean>

    <!--方法三：直接根据参数名赋值(最常使用)-->
    <bean id="user" class="com.SpringTest.pojo.User">
        <constructor-arg name="name" value="threewater"/>
    </bean>
```

3、测试

```java
    @Test
    public void UserTest() {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //在执行getBean的时候, user已经创建好了 , 通过无参构造
        User user = (User) context.getBean("user");
        //调用对象的方法 .
        System.out.println(user.toString());

    }
```

***结论：在配置文件加载的时候。其中管理的对象都已经初始化了！***

# 5.Spring配置

## 5.1别名

alias 设置别名 , 为bean设置别名 , 可以设置多个别名

```xml
<!--设置别名：在获取Bean的时候可以使用别名获取-->
<alias name="user" alias="u"/>
```

## 5.2Bean的配置

```xml
<!--bean就是java对象,由Spring创建和管理-->

    <!--
    id是bean的标识符,要唯一,如果没有配置id,name就是默认标识符
    如果配置id,又配置了name,那么name是别名，可以取多个别名，比alias高级
    class是bean对象所对应的全限定名
    -->
    <bean id="user" class="com.SpringTest.pojo.User" name="u u1,u2;u3">
        <property name="name" value="测试"/>
    </bean>
```

## 5.3import

团队的合作通过import来实现 

可以将多个配置文件，导入合并为一个

将所有beans*.xml全部合并到applicationContext.xml

```xml
<import resource="beans1.xml"/>
<import resource="beans2.xml"/>
<import resource="beans3.xml"/>
```

# 6.依赖注入

概念

- 依赖注入（Dependency Injection,DI）。
- 依赖 : 指Bean对象的创建依赖于容器 . Bean对象的依赖资源 .
- 注入 : 指Bean对象所依赖的资源 , 由容器来设置和装配 .

## 6.1构造器注入

我们在之前的案例已经讲过了

## 6.2Set方法注入 （重点）

- 依赖注入：Set注入
  - 依赖：bean对象的创建依赖于容器
  - 注入：bean对象中的所有属性，由容器来注入

【环境搭配】

要求被注入的属性 , 必须有set方法 , set方法的方法名由set + 属性首字母大写 , 如果属性是boolean类型 , 没有set方法 , 是 is .

测试pojo类 :

Address.java

```java
public class Address {

    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "Address{" +
                "address='" + address + '\'' +
                '}';
    }
}
```

Student.java

```java
public class Student {

    private String name;
    private Address address;
    private String[] books;
    private List<String> bobby;
    private Map<String,String> card;
    private Set<String> games;
    private String wife;
    private Properties info;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String[] getBooks() {
        return books;
    }

    public void setBooks(String[] books) {
        this.books = books;
    }

    public List<String> getBobby() {
        return bobby;
    }

    public void setBobby(List<String> bobby) {
        this.bobby = bobby;
    }

    public Map<String, String> getCard() {
        return card;
    }

    public void setCard(Map<String, String> card) {
        this.card = card;
    }

    public Set<String> getGames() {
        return games;
    }

    public void setGames(Set<String> games) {
        this.games = games;
    }

    public String getWife() {
        return wife;
    }

    public void setWife(String wife) {
        this.wife = wife;
    }

    public Properties getInfo() {
        return info;
    }

    public void setInfo(Properties info) {
        this.info = info;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", address=" + address +
                ", books=" + Arrays.toString(books) +
                ", bobby=" + bobby +
                ", card=" + card +
                ", games=" + games +
                ", wife='" + wife + '\'' +
                ", info=" + info +
                '}';
    }
}
```

beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="address" class="com.SpringTest.pojo.Address">
        <property name="address" value="西安"/>
    </bean>

    <bean id="student" class="com.SpringTest.pojo.Student">
        <!--普通值注入-->
        <property name="name" value="大毛"/>

        <!--Bean注入，ref-->
        <property name="address" ref="address"/>

        <!--数组注入-->
        <property name="books">
            <array>
                <value>javaSE</value>
                <value>MyBatis</value>
                <value>javaWeb</value>
                <value>Spring</value>
            </array>
        </property>

        <!--List注入-->
        <property name="hobby">
            <list>
                <value>学习javaSE</value>
                <value>学习MyBatis</value>
                <value>学习javaWeb</value>
                <value>学习Spring</value>
            </list>
        </property>

        <!--Map注入-->
        <property name="card">
            <map>
                <entry key="ID编号" value="001"/>
                <entry key="QQ号" value="000000"/>
                <entry key="电话号" value="123456"/>
            </map>
        </property>

        <!--Set注入-->
        <property name="games">
            <set>
                <value>LOL</value>
                <value>QQ飞车</value>
            </set>
        </property>

        <!--null注入-->
        <property name="wife">
            <null/>
        </property>

        <!--Properties注入-->
        <property name="info">
            <props>
                <prop key="学号">2021001</prop>
                <prop key="年级">大二</prop>
                <prop key="姓名">大毛</prop>
                <prop key="性别">男</prop>
            </props>
        </property>

    </bean>

</beans>
```

### 注入种类

1、**常量注入**

```xml
    <bean id="student" class="com.SpringTest.pojo.Student">
        <!--第一种：普通值注入-->
        <property name="name" value="大毛"/>
    </bean>
```

测试：

```java
    @Test
    public void StudentNameTest() {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        Student student = (Student) context.getBean("student");
        System.out.println(student.getName());
    }
```

2、**Bean注入** 

注意点：这里的值是一个引用，ref(引用Spring容器中创建好的对象)

```xml
<bean id="address" class="com.SpringTest.pojo.Address">
    <property name="address" value="西安"/>
</bean>
 
<bean id="student" class="com.SpringTest.pojo.Student">
    <!--Bean注入，ref-->
    <property name="address" ref="address"/>
</bean>
```

3、**数组注入**

```xml
<!--数组注入-->
<property name="books">
    <array>
        <value>javaSE</value>
        <value>MyBatis</value>
        <value>javaWeb</value>
        <value>Spring</value>
    </array>
</property>
```

4、**List注入**

```xml
<!--List注入-->
<property name="hobby">
    <list>
        <value>学习javaSE</value>
        <value>学习MyBatis</value>
        <value>学习javaWeb</value>
        <value>学习Spring</value>
    </list>
</property>
```

5、**Map注入**

```xml
<!--Map注入-->
<property name="card">
    <map>
        <entry key="ID编号" value="001"/>
        <entry key="QQ号" value="000000"/>
        <entry key="电话号" value="123456"/>
    </map>
</property>
```

6、**set注入**

```xml
<!--Set注入-->
<property name="games">
    <set>
        <value>LOL</value>
        <value>QQ飞车</value>
    </set>
</property>
```

7、**Null注入**

```xml
<!--null注入-->
<property name="wife">
    <null/>
</property>
```

8、**Properties注入**

```xml
<!--Properties注入-->
<property name="info">
    <props>
        <prop key="学号">2021001</prop>
        <prop key="年级">大二</prop>
        <prop key="姓名">大毛</prop>
        <prop key="性别">男</prop>
    </props>
</property>
```

测试结果：

```java
@Test
public void StudentTest() {
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    Student student = (Student) context.getBean("student");
    System.out.println(student.toString());
}

/*
Student{
name='大毛', 
address=Address{address='西安'}, 
books=[javaSE, MyBatis, javaWeb, Spring], 
hobby=[学习javaSE, 学习MyBatis, 学习javaWeb, 学习Spring], 
card={ID编号=001, QQ号=000000, 电话号=123456}, 
games=[LOL, QQ飞车], 
wife='null', 
info={学号=2021001, 性别=男, 年级=大二, 姓名=大毛}}
 */
```

## 6.3p命名和c命名注入

User.java ：【注意：这里没有有参构造器！】

```java
public class User {

    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public User() {
    }
}
```

Userbeans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

<bean id="user" class="com.SpringTest.pojo.User" p:age="15" p:name="大毛"/>

<bean id="user2" class="com.SpringTest.pojo.User" c:age="15" c:name="大毛"/>

</beans>
```

1、P命名空间注入 : 需要在头文件中加入约束文件

```xml
导入约束 :xmlns:p="http://www.springframework.org/schema/p"
 
<!--P(属性: properties)命名空间 , 属性依然要设置set方法-->
<bean id="user" class="com.SpringTest.pojo.User" p:age="15" p:name="大毛"/>
```

2、c 命名空间注入 : 需要在头文件中加入约束文件

```xml
导入约束 : xmlns:c="http://www.springframework.org/schema/c"

<!--C(构造: Constructor)命名空间 , 属性依然要设置set方法-->
<bean id="user2" class="com.SpringTest.pojo.User" c:age="15" c:name="大毛"/>
```

发现问题：爆红了，刚才我们没有写有参构造！

解决：把有参构造器加上，这里也能知道，c 就是所谓的构造器注入！

测试代码：

```java
    @Test
    public void UserTest(){
        ApplicationContext context = new ClassPathXmlApplicationContext("Userbeans.xml");
        User user = context.getBean("user", User.class);

        System.out.println(user.toString());
    }
```

## 6.4Bean的作用域

在Spring中，那些组成应用程序的主体及由Spring IoC容器所管理的对象，被称之为bean。简单地讲，bean就是由IoC容器初始化、装配及管理的对象 .

![image-20211011141544695](SpringPicture.assets\image-20211011141544695.png)

几种作用域中，request、session作用域仅在基于web的应用中使用（不必关心你所采用的是什么web应用框架），只能用在基于web的Spring ApplicationContext环境。

### 6.4.1Singleton(单例模式):Spring默认机制

当一个bean的作用域为Singleton，那么Spring IoC容器中只会存在一个共享的bean实例，并且所有对bean的请求，只要id与该bean定义相匹配，则只会返回bean的同一实例。Singleton是单例类型，就是在创建起容器时就同时自动创建了一个bean的对象，不管你是否使用，他都存在了，每次获取到的对象都是同一个对象。注意，Singleton作用域是Spring中的缺省作用域。要在XML中将bean定义成singleton，可以这样配置：

```xml
<bean id="user" class="com.SpringTest.pojo.User" p:age="15" p:name="大毛" scope="singleton"/>
```

测试：

```java
 @Test
 public void test03(){
     ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
     User user = (User) context.getBean("user");
     User user2 = (User) context.getBean("user");
     System.out.println(user==user2);//false
 }
```

### 6.4.2Prototype（原型模式）

当一个bean的作用域为Prototype，表示一个bean定义对应多个对象实例。Prototype作用域的bean会导致在每次对该bean请求（将其注入到另一个bean中，或者以程序的方式调用容器的getBean()方法）时都会创建一个新的bean实例。Prototype是原型类型，它在我们创建容器的时候并没有实例化，而是当我们获取bean的时候才会去创建一个对象，而且我们每次获取到的对象都不是同一个对象。根据经验，对有状态的bean应该使用prototype作用域，而对无状态的bean则应该使用singleton作用域。在XML中将bean定义成prototype，可以这样配置：

```xml
<bean id="user" class="com.SpringTest.pojo.User" p:age="15" p:name="大毛" scope="prototype"/>
  或者
 <bean id="account" class="com.foo.DefaultAccount" singleton="false"/>
```

### 6.4.3Request（Web开发中使用）

当一个bean的作用域为Request，表示在一次HTTP请求中，一个bean定义对应一个实例；即每个HTTP请求都会有各自的bean实例，它们依据某个bean定义创建而成。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

```xml
 <bean id="loginAction" class=cn.csdn.LoginAction" scope="request"/>
```

针对每次HTTP请求，Spring容器会根据loginAction bean的定义创建一个全新的LoginAction bean实例，且该loginAction bean实例仅在当前HTTP request内有效，因此可以根据需要放心的更改所建实例的内部状态，而其他请求中根据loginAction bean定义创建的实例，将不会看到这些特定于某个请求的状态变化。当处理请求结束，request作用域的bean实例将被销毁。

### 6.4.4Session（Web开发中使用）

当一个bean的作用域为Session，表示在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

```xml
 <bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/>
```

针对某个HTTP Session，Spring容器会根据userPreferences bean定义创建一个全新的userPreferences bean实例，且该userPreferences bean仅在当前HTTP Session内有效。与request作用域一样，可以根据需要放心的更改所创建实例的内部状态，而别的HTTP Session中根据userPreferences创建的实例，将不会看到这些特定于某个HTTP Session的状态变化。当HTTP Session最终被废弃的时候，在该HTTP Session作用域内的bean也会被废弃掉。

# 7.自动装配说明

- 自动装配是使用spring满足bean依赖的一种方法
- spring会在应用上下文中为某个bean寻找其依赖的bean。

Spring中bean有三种装配机制，分别是：

1. 在xml中显式配置；
2. 在java中显式配置；
3. 隐式的bean发现机制和自动装配。

这里我们主要讲第三种：自动化的装配bean。

Spring的自动装配需要从两个角度来实现，或者说是两个操作：

1. 组件扫描(component scanning)：spring会自动发现应用上下文中所创建的bean；
2. 自动装配(autowiring)：spring自动满足bean之间的依赖，也就是我们说的IoC/DI；

组件扫描和自动装配组合发挥巨大威力，使得显示的配置降低到最少。

**推荐不使用自动装配xml配置 , 而使用注解 。**

## 7.1测试环境搭建

1、新建一个项目

2、新建两个实体类，Cat  Dog  都有一个叫的方法

```java
public class Cat {
    public void shout(){
        System.out.println("miao~");
    }
}

public class Dog {
    public void shout() {
        System.out.println("wang~");
    }
}
```

3、新建一个用户类 User

```java
public class People {

    private Cat cat;
    private Dog dog;
    private String name;

    public People() {
    }

    public People(Cat cat, Dog dog, String name) {
        this.cat = cat;
        this.dog = dog;
        this.name = name;
    }

    public Cat getCat() {
        return cat;
    }

    public void setCat(Cat cat) {
        this.cat = cat;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "People{" +
                "cat=" + cat +
                ", dog=" + dog +
                ", name='" + name + '\'' +
                '}';
    }
}
```

4、编写Spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dog" class="com.SpringTest.pojo.Dog"/>
    <bean id="cat" class="com.SpringTest.pojo.Cat"/>
    
    <bean id="people" class="com.SpringTest.pojo.People">
        <property name="name" value="大毛"/>
        <property name="cat" ref="cat"/>
        <property name="dog" ref="dog"/>
    </bean>

</beans>
```

5、测试

```java
    @Test
    public void test() {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        People people = context.getBean("people", People.class);

        people.getCat().shout();
        people.getDog().shout();

    }
```

结果正常输出，环境OK

## 7.2byName

**autowire byName (按名称自动装配)**

由于在手动配置xml过程中，常常发生字母缺漏和大小写等错误，而无法对其进行检查，使得开发效率降低。

采用自动装配将避免这些错误，并且使配置简单化。

测试：

1、修改bean配置，增加一个属性  autowire="byName"

```XML
<bean id="people" class="com.SpringTest.pojo.People" autowire="byName">
    <property name="name" value="大毛"/>
</bean>
```

2、再次测试，结果依旧成功输出！

3、我们将 cat 的bean id修改为 catXXX

4、再次测试， 执行时报空指针java.lang.NullPointerException。因为按byName规则找不对应set方法，真正的setCat就没执行，对象就没有初始化，所以调用时就会报空指针错误。

**小结：**

当一个bean节点带有 autowire byName的属性时。

1. 将查找其类中所有的set方法名，例如setCat，获得将set去掉并且首字母小写的字符串，即cat。

2. 去spring容器中寻找是否有此字符串名称id的对象。

3. 如果有，就取出注入；如果没有，就报空指针异常。


## 7.3byType

**autowire byType (按类型自动装配)**

使用autowire byType首先需要保证：同一类型的对象，在spring容器中唯一。如果不唯一，会报不唯一的异常。

```java
NoUniqueBeanDefinitionException
```

测试：

1、将user的bean配置修改一下 ： autowire="byType"

2、测试，正常输出

3、在注册一个cat 的bean对象！

```XML
    <bean id="dog" class="com.SpringTest.pojo.Dog"/>
    <bean id="cat" class="com.SpringTest.pojo.Cat"/>
	<bean id="cat2" class="com.SpringTest.pojo.Cat"/>

    <bean id="people" class="com.SpringTest.pojo.People" autowire="byType">
        <property name="name" value="大毛"/>
    </bean>
```

4、测试，报错：NoUniqueBeanDefinitionException

5、删掉cat2，将cat的bean名称改掉！测试！因为是按类型装配，所以并不会报异常，也不影响最后的结果。甚至将id属性去掉，也不影响结果。

这就是按照类型自动装配！

## 7.4使用注解实现自动装配

jdk1.5开始支持注解，spring2.5开始全面支持注解。

准备工作：利用注解的方式注入属性。

1、在spring配置文件中引入context文件头

```xml
xmlns:context="http://www.springframework.org/schema/context"

http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
```

2、开启属性注解支持！

```xml
<context:annotation-config/>
```

beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">
    <context:annotation-config/>

</beans>
```

#### @Autowired

- @Autowired是按类型自动转配的，不支持id匹配。
- 需要导入 spring-aop 的包！

测试：

1、将User类中的set方法去掉，使用@Autowired注解

```java
public class People {

    @Autowired
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
  	private String str;

   	public Cat getCat() {
       	return cat;
  	}
    
   	public Dog getDog() {
       	return dog;
  	}
    
   	public String getStr() {
       	return str;
  	}
    
}
```

2、此时配置文件内容

```xml
<bean id="dog" class="com.SpringTest.pojo.Dog"/>
<bean id="cat" class="com.SpringTest.pojo.Cat"/>
<bean id="people" class="com.SpringTest.pojo.People"/>
```

3、测试，成功输出结果！

##### 科普

@Autowired(required=false)  说明：false，对象可以为null；true，对象必须存对象，不能为null。

```java
//如果允许对象为null，设置required = false,默认为true
@Autowired(required = false)
private Cat cat;
```

#### @Qualifier

- @Autowired是根据类型自动装配的，加上@Qualifier则可以根据byName的方式自动装配
- **@Qualifier不能单独使用。**

测试实验步骤：

1、配置文件修改内容，保证类型存在对象。且名字不为类的默认名字！

```xml
<bean id="dog" class="com.SpringTest.pojo.Dog"/>
<bean id="dog2" class="com.SpringTest.pojo.Dog"/>
<bean id="cat" class="com.SpringTest.pojo.Cat"/>
<bean id="cat2" class="com.SpringTest.pojo.Cat"/>
```

2、没有加Qualifier测试，直接报错

3、在属性上添加Qualifier注解

```java
    @Autowired
    @Qualifier(value = "cat2")
    private Cat cat;

    @Autowired
    @Qualifier(value = "dog2")
    private Dog dog;
```

测试，成功输出！

#### @Resource

- @Resource如有指定的name属性，先按该属性进行byName方式查找装配；
- 其次再进行默认的byName方式进行装配；
- 如果以上都不成功，则按byType的方式自动装配。
- 都不成功，则报异常。

实体类：

```java
public class User {
  	 //如果允许对象为null，设置required = false,默认为true
	@Resource(name = "cat2")
	private Cat cat;
	
	@Resource(name = "dog2")
	private Dog dog;
	
	private String name;
}
```

beans.xml

```
<bean id="dog" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
<bean id="cat2" class="com.kuang.pojo.Cat"/>

<bean id="user" class="com.kuang.pojo.User"/>
```

测试：结果OK

配置文件2：beans.xml ， 删掉cat2

```
<bean id="dog" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
```

实体类上只保留注解

```
@Resource
private Cat cat;
@Resource
private Dog dog;
```

结果：OK

结论：先进行byName查找，失败；再进行byType查找，成功。

### 小结

@Autowired与@Resource异同：

1、@Autowired与@Resource都可以用来装配bean。都可以写在字段上，或写在setter方法上。

2、@Autowired默认按类型装配（属于spring规范），默认情况下必须要求依赖对象必须存在，如果要允许null 值，可以设置它的required属性为false，如：@Autowired(required=false) ，如果我们想使用名称装配可以结合@Qualifier注解进行使用

3、@Resource（属于J2EE复返），默认按照名称进行装配，名称可以通过name属性进行指定。如果没有指定name属性，当注解写在字段上时，默认取字段名进行按照名称查找，如果注解写在setter方法上默认取属性名进行装配。当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的是，如果name属性一旦指定，就只会按照名称进行装配。

它们的作用相同都是用注解方式注入对象，但执行顺序不同。@Autowired先byType，@Resource先byName。

# 8.使用注解开发

在spring4之后，想要使用注解形式，必须得要引入aop的包

![image-20211011185343591](SpringPicture.assets\image-20211011185343591.png)

在配置文件当中，还得要引入一个context约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">

</beans>
```

## 8.1Bean的实现

我们之前都是使用 bean 的标签进行bean注入，但是实际开发中，我们一般都会使用注解！

1、配置扫描哪些包下的注解

```xml
    <!--指定要扫描的包，这个包下的注解就会生效-->
    <context:component-scan base-package="com.SpringTest.pojo"/>
```

2、在指定包下编写类，增加注解

```java
//等价于<bean id="user" class="com.SpringTest.pojo.User"/>
@Component("user")
public class User {
    public String name = "大毛";
}
```

3、测试

```java
    @Test
    public void Test() {
        ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");

        User user = (User) context.getBean("user");

        System.out.println(user.name);
    }
```

## 8.2属性注入

使用注解注入属性

1、可以不用提供set方法，直接在直接名上添加@value("值")

```java
@Component("user")
public class User {
    @Value("大毛")
    public String name;
}
```

2、如果提供了set方法，在set方法上添加@value("值");

```java
@Component("user")
public class User {
    
    public String name;

    @Value("大毛")
    public void setName(String name) {
        this.name = name;
    }
}
```

## 8.3衍生注解

```xml
<context:component-scan base-package="com.SpringTest"/>
```

我们这些注解，就是替代了在配置文件当中配置步骤而已！更加的方便快捷！

**@Component三个衍生注解**

为了更好的进行分层，Spring可以使用其它三个注解，功能一样，目前使用哪一个功能都一样。

- @Controller：web层

  ```java
  @Controller
  public class UserController {
      
  }
  ```

- @Service：service层

  ```java
  @Service
  public class UserService {
      
  }
  ```

- @Repository：dao层

  ```java
  @Repository
  public class UserDao {
  
  }
  ```

- 写上这些注解，就相当于将这个类交给Spring管理装配了！

## 8.4自动装配注解

详见7.4使用注解实现自动装配

## 8.5作用域

@scope

- singleton：默认的，Spring会采用单例模式创建这个对象。关闭工厂 ，所有的对象都会销毁。
- prototype：多例模式。关闭工厂 ，所有的对象不会销毁。内部的垃圾回收机制会回收

```java
@Controller("user")
@Scope("prototype")
public class User {
   @Value("大毛")
   public String name;
}
```

## **小结**

**XML与注解比较**

- XML可以适用任何场景 ，结构清晰，维护方便
- 注解不是自己提供的类使用不了，开发简单方便

**xml与注解整合开发** ：推荐最佳实践

- xml管理Bean
- 注解完成属性注入
- 使用过程中， 可以不用扫描，扫描是为了类上的注解

```xml
<context:annotation-config/>  
```

作用：

- 进行注解驱动注册，从而使注解生效

- 用于激活那些已经在spring容器里注册过的bean上面的注解，也就是显示的向Spring注册

- 如果不扫描包，就需要手动配置bean

- 如果不加注解驱动，则注入的值为null！


# 9.基于Java类进行配置Spring

JavaConfig 原来是 Spring 的一个子项目，它通过 Java 类的方式提供 Bean 的定义信息，在 Spring4 的版本， JavaConfig 已正式成为 Spring4 的核心功能 。

1、编写一个实体类

```java
public class User {

    private String name;

    public String getName() {
        return name;
    }

    @Value("大毛")
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

2、新建一个config配置包，编写一个MyConfig配置类

```java
@Configuration//这个也会被Spring容器托管，注册到容器中因为他本来就是一个@Component
//@Configuration这是一个配置类，和beans.xml的作用是一样的
@ComponentScan("com.SpringTest.Pojo")
public class MyConfig {

    /*
    注册一个bean，就相当于之前写的bean标签
    方法名相当于bean标签中的id属性
    返回值相当于bean标签中的class属性
    */
    @Bean
    public User user(){
        return new User();//返回要注入到bean中的对象
    }
}
```

3、测试

```java
@Test
public void ConfigTest(){
    
    //如果完全使用了配置类的方法去做，我们就只能通过 AnnotationConfig 上下文来取容器，通过配置类的class对象加载
    ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
    User getUser = (User) context.getBean("user");
    String name = getUser.getName();
    System.out.println(name);
    
}
```

4、成功输出结果！

**导入其他配置如何做呢？**

1、我们再编写一个配置类！

```java
@Configuration  //代表这是一个配置类
public class MyConfig2 {
}
```

2、在之前的配置类中我们来选择导入这个配置类

```java
@Configuration
@ComponentScan("com.SpringTest.Pojo")
@Import(MyConfig2.class)
public class MyConfig {
    @Bean
    public User user(){
        return new User();
    }
}
```

关于这种Java类的配置方式，我们在之后的SpringBoot 和 SpringCloud中还会大量看到，我们需要知道这些注解的作用即可！

# 10.静态/动态代理模式

为什么要学习代理模式，因为AOP的底层机制就是动态代理！

代理模式：

- 静态代理
- 动态代理

学习aop之前 , 我们要先了解一下代理模式！

![image-20211011192218095](SpringPicture.assets\image-20211011192218095.png)



## 10.1 静态代理

**静态代理角色分析**

- 抽象角色 : 一般使用接口或者抽象类来实现

- 真实角色 : 被代理的角色

- 代理角色 : 代理真实角色 ; 代理真实角色后 , 一般会做一些附属的操作 .

- 客户  :  使用代理角色来进行一些操作 .


**代码实现**

Rent . java 即抽象角色

```java
//抽象角色：租房
public interface Rent {
   public void rent();
}
```

Host . java 即真实角色

```java
//真实角色: 房东，房东要出租房子
public class Host implements Rent{
   public void rent() {
       System.out.println("房屋出租");
  }
}
```

Proxy . java 即代理角色

```java
//代理角色：中介
public class Proxy implements Rent {

   private Host host;
   public Proxy() { }
   public Proxy(Host host) {
       this.host = host;
  }

   //租房
   public void rent(){
       seeHouse();
       host.rent();
       fare();
  }
    
   //看房
   public void seeHouse(){
       System.out.println("带房客看房");
  }
   //收中介费
   public void fare(){
       System.out.println("收中介费");
  }
}
```

Client . java 即客户

```java
//客户类，一般客户都会去找代理！
public class Client {
   public static void main(String[] args) {
       //房东要租房
       Host host = new Host();
       //中介帮助房东
       Proxy proxy = new Proxy(host);

       //你去找中介！
       proxy.rent();
  }
}
```

分析：在这个过程中，你直接接触的就是中介，就如同现实生活中的样子，你看不到房东，但是你依旧租到了房东的房子通过代理，这就是所谓的代理模式，程序源自于生活，所以学编程的人，一般能够更加抽象的看待生活中发生的事情。

**静态代理的好处:**

- 可以使得我们的真实角色更加纯粹 . 不再去关注一些公共的事情 .
- 公共的业务由代理来完成 . 实现了业务的分工 ,
- 公共业务发生扩展时变得更加集中和方便 .

缺点 :

- 类多了 , 多了代理类 , 工作量变大了 . 开发效率降低 .

我们想要静态代理的好处，又不想要静态代理的缺点，所以 , 就有了动态代理 !

## 10.2静态代理再理解

同学们练习完毕后，我们再来举一个例子，巩固大家的学习！

练习步骤：

1、创建一个抽象角色，比如咋们平时做的用户业务，抽象起来就是增删改查！

```java
//抽象角色：增删改查业务
public interface UserService {
   void add();
   void delete();
   void update();
   void query();
}
```

2、我们需要一个真实对象来完成这些增删改查操作

```java
//真实对象，完成增删改查操作的人
public class UserServiceImpl implements UserService {

   public void add() {
       System.out.println("增加了一个用户");
  }

   public void delete() {
       System.out.println("删除了一个用户");
  }

   public void update() {
       System.out.println("更新了一个用户");
  }

   public void query() {
       System.out.println("查询了一个用户");
  }
}
```

3、需求来了，现在我们需要增加一个日志功能，怎么实现！

- 思路1 ：在实现类上增加代码 【麻烦！】
- 思路2：使用代理来做，能够不改变原来的业务情况下，实现此功能就是最好的了！

4、设置一个代理类来处理日志！代理角色

```java
//代理角色，在这里面增加日志的实现
public class UserServiceProxy implements UserService {
   private UserServiceImpl userService;

   public void setUserService(UserServiceImpl userService) {
       this.userService = userService;
  }

   public void add() {
       log("add");
       userService.add();
  }

   public void delete() {
       log("delete");
       userService.delete();
  }

   public void update() {
       log("update");
       userService.update();
  }

   public void query() {
       log("query");
       userService.query();
  }

   public void log(String msg){
       System.out.println("执行了"+msg+"方法");
  }

}
```

5、测试访问类：

```java
public class Client {
   public static void main(String[] args) {
       //真实业务
       UserServiceImpl userService = new UserServiceImpl();
       //代理类
       UserServiceProxy proxy = new UserServiceProxy();
       //使用代理类实现日志功能！
       proxy.setUserService(userService);

       proxy.add();
  }
}
```

OK，到了现在代理模式大家应该都没有什么问题了，重点大家需要理解其中的思想；

我们在不改变原来的代码的情况下，实现了对原有功能的增强，这是AOP中最核心的思想

聊聊AOP：纵向开发，横向开发

![image-20211011200603263](SpringPicture.assets\image-20211011200603263.png)

## 10.3动态代理

- 动态代理的角色和静态代理的一样。

- 动态代理的代理类是动态生成的，静态代理的代理类是我们提前写好的

- 动态代理分为两类 : 一类是基于接口动态代理 , 一类是基于类的动态代理

- - 基于接口的动态代理——JDK动态代理
  - 基于类的动态代理——cglib
  - 现在用的比较多的是 javasist(java字节码实现) 来生成动态代理 . 百度一下javasist
  - 我们这里使用JDK的原生代码来实现，其余的道理都是一样的！、

**JDK的动态代理需要了解两个类**

核心 : InvocationHandler   和   Proxy  ， 打开JDK帮助文档看看

【InvocationHandler：调用处理程序】

![image-20211011203533674](SpringPicture.assets\image-20211011203533674.png)

![image-20211011203600752](SpringPicture.assets\image-20211011203600752.png)

```java
Object invoke(Object proxy, 方法 method, Object[] args)；
//参数
    
//proxy - 调用该方法的代理实例
    
//method -所述方法对应于调用代理实例上的接口方法的实例。方法对象的声明类将是该方法声明的接口，它可以是代理类继承该方法的代理接口的超级接口。
    
//args -包含的方法调用传递代理实例的参数值的对象的阵列，或null如果接口方法没有参数。原始类型的参数包含在适当的原始包装器类的实例中，例如java.lang.Integer或java.lang.Boolean 。
```

【Proxy  : 代理】

![image-20211011203232939](SpringPicture.assets\image-20211011203232939.png)

![image-20211011203319505](SpringPicture.assets\image-20211011203319505.png)

![image-20211011204014982](SpringPicture.assets\image-20211011204014982.png)

```java
//生成代理类
public Object getProxy(){
   return Proxy.newProxyInstance(this.getClass().getClassLoader(),rent.getClass().getInterfaces(),this);
}
```

**代码实现** 

抽象角色和真实角色和之前的一样！

Rent . java 即抽象角色

```java
//抽象角色：租房
public interface Rent {
   public void rent();
}
```

Host . java 即真实角色

```java
//真实角色: 房东，房东要出租房子
public class Host implements Rent{
   public void rent() {
       System.out.println("房屋出租");
  }
}
```

ProxyInvocationHandler. java 即代理角色

```java
public class ProxyInvocationHandler implements InvocationHandler {
    private Rent rent;

    public void setRent(Rent rent) {
        this.rent = rent;
    }

    //生成代理类，重点是第二个参数，获取要代理的抽象角色！之前都是一个角色，现在可以代理一类角色
    public Object getProxy() {
        return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                rent.getClass().getInterfaces(), this);
    }

    // proxy : 代理类 method : 代理类的调用处理程序的方法对象.
    // 处理代理实例上的方法调用并返回结果
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //核心：本质利用反射实现！
        Object result = method.invoke(rent, args);

        return result;
    }
    
}
```

Client . java

```java
//租客
public class Client {

   public static void main(String[] args) {
       //真实角色
       Host host = new Host();
       //代理实例的调用处理程序
       ProxyInvocationHandler pih = new ProxyInvocationHandler();
       pih.setRent(host); //将真实角色放置进去！
       Rent proxy = (Rent)pih.getProxy(); //动态生成对应的代理类！
       proxy.rent();
  }

}
```

核心：**一个动态代理 , 一般代理某一类业务 , 一个动态代理可以代理多个类，代理的是接口！、**

## 10.4深化理解

我们来使用动态代理实现代理我们后面写的UserService！

我们也可以编写一个通用的动态代理实现的类！所有的代理对象设置为Object即可！

```java
public class ProxyInvocationHandler implements InvocationHandler {
   private Object target;

   public void setTarget(Object target) {
       this.target = target;
  }

   //生成代理类
   public Object getProxy(){
       return Proxy.newProxyInstance(this.getClass().getClassLoader(),
               target.getClass().getInterfaces(),this);
  }

   // proxy : 代理类
   // method : 代理类的调用处理程序的方法对象.
   public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
       log(method.getName());
       Object result = method.invoke(target, args);
       return result;
  }

   public void log(String methodName){
       System.out.println("执行了"+methodName+"方法");
  }

}
```

测试(demo2中的方法)

```java
public class Test {
   public static void main(String[] args) {
       //真实对象
       UserServiceImpl userService = new UserServiceImpl();
       //代理对象的调用处理程序
       ProxyInvocationHandler pih = new ProxyInvocationHandler();
       pih.setTarget(userService); //设置要代理的对象
       UserService proxy = (UserService)pih.getProxy(); //动态生成代理类！
       proxy.delete();
  }
}
```

测试，增删改查，查看结果！

## 10.5动态代理的好处

静态代理有的它都有，静态代理没有的，它也有！

- 可以使得我们的真实角色更加纯粹 . 不再去关注一些公共的事情 .
- 公共的业务由代理来完成 . 实现了业务的分工 ,
- 公共业务发生扩展时变得更加集中和方便 .
- 一个动态代理 , 一般代理某一类业务
- 一个动态代理可以代理多个类，代理的是接口！

## 10.6 实例

Spring的AOP中以下两种动态代理的方式都用到了

### jdk动态代理

```java
//用户的接口
public interface IUserService {
    public String add(Integer a) ;
}
```

```java
public class UserServiceImpl implements IUserService {

    @Override
    public String add(Integer a) {

        System.out.println("add......"+a);

        return "娃哈哈";
    }
}
```

```java
//处理器: 方法的增强
public class UserServiceHandler implements InvocationHandler {

    private  IUserService  userService;

    public UserServiceHandler(IUserService userService) {
        this.userService = userService;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //2如果有参数： 可以改变参数的实际值 :比如原来是1  ==》变成2
        Integer i = (Integer)args[0];
        System.out.println("i="+i);
        args[0] = i+100;

        System.out.println("调用之前");
        //调用原来的方法，： 方法所在类的对象， 方法的参数列表
        Object invoke = method.invoke(userService, args);
        System.out.println("调用之后");

        //3如果有返回值： 返回值也是可以变的
        String str = (String)invoke;
        System.out.println("str="+str);
        return invoke+"(⊙o⊙)哦";
    }
}
```

```java
public class UserServiceTest {

    public static void main(String[] args) {

        //1.要有被代理 对象
        IUserService userService = new UserServiceImpl();
        
        //2.处理器
        UserServiceHandler handler = new UserServiceHandler(userService);

        //3.创建代理 对象
        IUserService proxy = (IUserService) Proxy.newProxyInstance(UserServiceTest.class.getClassLoader(),
                userService.getClass().getInterfaces(),//被 代理对象的接口列表
                handler
                );
        String add = proxy.add(10);
        System.out.println("======================");
        System.out.println(add);

    }

}
```

### cglib动态代理

```java
//被代理类
public class UserAddress {

    //增
    public  void add() {
        System.out.println("add......");
    }

    public  void del() {
        System.out.println("del......");
    }

    public  void update() {
        System.out.println("update......");
    }

    public  void select() {
        System.out.println("select......");
    }

}
```

```java
//为每个方法提供拦截器，用来做方法增强
public class AddInterceptor  implements MethodInterceptor {

    //方法的增强
    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {

        //调用原来的方法
        Object invoke = methodProxy.invokeSuper(o, objects);
        return invoke;
    }
}
```

```java
//为每个方法提供拦截器，用来做方法增强
public class DelInterceptor implements MethodInterceptor {

    //方法的增强
    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {

        //调用原来的方法
        Object invoke = methodProxy.invokeSuper(o, objects);
        return invoke;
    }
}
```

```java
//为每个方法提供拦截器，用来做方法增强
public class SelectInterceptor implements MethodInterceptor {

    //方法的增强
    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {

        //调用原来的方法
        Object invoke = methodProxy.invokeSuper(o, objects);
        return invoke;
    }
}
```

```java
//为每个方法提供拦截器，用来做方法增强
public class UpdateInterceptor implements MethodInterceptor {

    //方法的增强
    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {

        System.out.println("调用前");
        //调用原来的方法
        Object invoke = methodProxy.invokeSuper(o, objects);
        System.out.println("调用后");

        return invoke;
    }
}
```

```java
//过滤回调，主要用来标记： 方法的前后顺序
public class UserAddressCallBackFilter implements CallbackFilter {

    @Override
    public int accept(Method method) {

        String methodName = method.getName();

        if(methodName.equals("add")) {
            return  0;
        }else if(methodName.equals("del")) {
            return 1;
        }else if(methodName.equals("update")) {
            return 2;
        }else {
            return 3;
        }
    }
}
```

```java
public class UserAddressMain {

    public static void main(String[] args) {

        //1创建一个增强器
        Enhancer enhancer = new Enhancer();

        //2设置被 代理类的类型
        enhancer.setSuperclass(UserAddress.class);

        //3.设置回调的过滤器对象
        enhancer.setCallbackFilter(new UserAddressCallBackFilter());

        //4.设置过滤器和  具体方法栏截器的对应关系
        Callback[] callbacks = {
                new AddInterceptor(),  //下标是0
                new DelInterceptor(),  //下标是1
                new UpdateInterceptor(), //下标是2
                new SelectInterceptor() //下标是3
        };
        enhancer.setCallbacks(callbacks);

        //5.创建代理类对象
        UserAddress userAddress = (UserAddress)enhancer.create();
        userAddress.add();

    }

}
```

### 二者区别

 （1）JDK动态代理只能对实现了接口的类生成代理，而不能针对类

 （2）CGLIB是针对类实现代理，主要是对指定的类生成一个子类，覆盖其中的方法。 因为是继承，所以该类或方法最好不要声明成final

# 11.AOP

## 11.1什么是AOP

AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

![image-20211012124918226](SpringPicture.assets\image-20211012124918226.png)

## 11.2Aop在Spring中的作用

提供声明式事务；允许用户自定义切面

以下名词需要了解下：

- 连接点（JointPoint）：连接点是程序类中客观存在的方法，可以被Spring拦截并切入内容，与切入点匹配的执行点。
- 切入点（PointCut）：被Spring切入的连接点（增强的方法）。切面通知执行的 “地点”的定义。
- 通知、增强（Advice）：切面必须要完成的工作。即，它是类中的一个方法。分为前置通知、后置通知、环绕通知、最终通知、异常通知。
- 目标对象（Target）：代理的目标对象，即，被通知对象。
- 引介（Introduction）：一种特殊的增强，可在运行期为类动态添加Field和Method。
- 织入(Weaving)：把通知应用到具体的类，今儿创建新的代理类的过程。
- 代理（Proxy）：向目标对象应用通知之后创建的对象。
- 切面（ASPECT）：由切点和通知组成，将横切逻辑织入切面所指定的连接点中。横切关注点被模块化的特殊对象。即，它是一个类。

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志 , 安全 , 缓存 , 事务等等 ....

![image-20211012125253272](SpringPicture.assets\image-20211012125253272.png)

SpringAOP中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice:

![image-20211012125425019](SpringPicture.assets\image-20211012125425019.png)

即 Aop 在 不改变原有代码的情况下 , 去增加新的功能 .

**主要用于日志**

**在Spring中利用AOP来完成对事物的控制**

## 11.3使用Spring实现Aop

【重点】使用AOP织入，需要导入一个依赖包！

```XML
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
   <groupId>org.aspectj</groupId>
   <artifactId>aspectjweaver</artifactId>
   <version>1.9.4</version>
</dependency>
```

### 11.3.1**第一种方式**

**通过 Spring API 实现**

首先编写我们的业务接口和实现类

```java
public interface UserService {

   public void add();

   public void delete();

   public void update();

   public void search();

}
public class UserServiceImpl implements UserService{

   @Override
   public void add() {
       System.out.println("增加用户");
  }

   @Override
   public void delete() {
       System.out.println("删除用户");
  }

   @Override
   public void update() {
       System.out.println("更新用户");
  }

   @Override
   public void search() {
       System.out.println("查询用户");
  }
}
```

然后去写我们的增强类 , 我们编写两个 , 一个前置增强 一个后置增强

```java
//前置增强
public class Log implements MethodBeforeAdvice {

   //method : 要执行的目标对象的方法
   //objects : 被调用的方法的参数
   //Object : 目标对象
   @Override
   public void before(Method method, Object[] objects, Object o) throws Throwable {
       System.out.println( o.getClass().getName() + "的" + method.getName() + "方法被执行了");
  }
}

//后置增强
public class AfterLog implements AfterReturningAdvice {
   //returnValue 返回值
   //method被调用的方法
   //args 被调用的方法的对象的参数
   //target 被调用的目标对象
   @Override
   public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
       System.out.println("执行了" + target.getClass().getName()
       +"的"+method.getName()+"方法,"
       +"返回值："+returnValue);
  }
}
```

最后去spring的文件中注册 , 并实现aop切入实现 , 注意导入约束 .

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--注册bean-->
    <bean id="userService" class="com.SpringTest.Service.UserServiceImpl"/>
    <bean id="log" class="com.SpringTest.Log.Log"/>
    <bean id="afterLog" class="com.SpringTest.Log.AfterLog"/>

    <!--方式一：使用原生Spring API接口-->
    <!--aop的配置:需要导入aop的约束-->
    <aop:config>
        <!--切入点 expression:表达式匹配要执行的方法-->
        <aop:pointcut id="pointcut" expression="execution(* com.SpringTest.Service.UserServiceImpl.*(..))"/>
        <!--执行环绕; advice-ref执行方法 . pointcut-ref切入点-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>

    <!--
     execution() 切点表达式
     * 默认
     * 最外层的包名 com
     * 下层包名     qf
     * 通常为service包名
     * impl
     * impl包中的类
     * 类中的方法
     （..） 任意参数
     -->

    <aop:config>
        <!-- 前置通知 advice-ref="前置通知类"  pointcut="切点表达式"-->
	    <aop:advisor advice-ref="before" pointcut="execution(* *.*.*.*.*(..))"></aop:advisor>
    </aop:config>
    
</beans>
```

测试

```java
    @Test
    public void aopTest01(){
        ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");

        UserService userService = context.getBean("userService", UserService.class);

        userService.add();
    }
```

Aop的重要性 : 很重要 . 一定要理解其中的思路 , 主要是思想的理解这一块 .

Spring的Aop就是将公共的业务 (日志 , 安全等) 和领域业务结合起来 , 当执行领域业务时 , 将会把公共业务加进来 . 实现公共业务的重复利用 . 领域业务更纯粹 , 程序猿专注领域业务 , 其本质还是动态代理 . 

### 11.3.2**第二种方式**

**自定义类来实现Aop**

目标业务类不变依旧是userServiceImpl

第一步 : 写我们自己的一个切入类

```java
public class DIYPointCut {
    public void before(){
        System.out.println("---------方法执行前---------");
    }
    public void after(){
        System.out.println("---------方法执行后---------");
    }
}
```

去spring中配置

```xml
    <!--方式二：自定义实现AOP-->
    <!--注册bean-->
    <bean id="diy" class="com.SpringTest.DIY.DIYPointCut"/>
    <aop:config>
        <!--自定义切面，ref要引用的类-->
        <aop:aspect ref="diy">
            <!--切入点-->
            <aop:pointcut id="point" expression="execution(* com.SpringTest.Service.UserServiceImpl.*(..))"/>
            <!--通知-->
            <aop:before method="before" pointcut-ref="point"/>
            <aop:after method="after" pointcut-ref="point"/>
        </aop:aspect>
    </aop:config>
```

测试：

```java
    @Test
    public void aopTest02(){
        ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");

        UserService userService = context.getBean("userService", UserService.class);

        userService.add();
    }
```

### 11.3.3**第三种方式**

**使用注解实现**

第一步：编写一个注解实现的增强类

```java
@Aspect//标注这个类是一个切面
public class AnnotationPointCut {

    @Before("execution(* com.SpringTest.Service.UserServiceImpl.*(..))")
    public void before() {
        System.out.println("---------方法执行前---------");
    }

    @After("execution(* com.SpringTest.Service.UserServiceImpl.*(..))")
    public void after() {
        System.out.println("---------方法执行后---------");
    }

    //在环绕增强中，我们可以给定一个参数，代表我们要获取处理的切入的点
    @Around("execution(* com.SpringTest.Service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint jp) throws Throwable {
        System.out.println("环绕前");
        System.out.println("签名:" + jp.getSignature());//获取签名
        //执行目标方法proceed
        Object proceed = jp.proceed();//执行方法
        System.out.println("环绕后");
        System.out.println(proceed);
    }

}
```

第二步：在Spring配置文件中，注册bean，并增加支持注解的配置

```xml
    <!--方式三：用注解-->
    <bean id="annotationPointCut" class="com.SpringTest.DIY.AnnotationPointCut"/>
    <!--开启注解支持  JDK（默认proxy-target-class="false"）cglib(proxy-target-class="true")-->
    <aop:aspectj-autoproxy expose-proxy="true"/>
```

**aop:aspectj-autoproxy：说明**

> 通过aop命名空间的<aop:aspectj-autoproxy />声明自动为spring容器中那些配置@aspectJ切面的bean创建代理，织入切面。当然，spring 在内部依旧采用AnnotationAwareAspectJAutoProxyCreator进行自动代理的创建工作，但具体实现的细节已经被<aop:aspectj-autoproxy />隐藏起来了
>
> <aop:aspectj-autoproxy />有一个proxy-target-class属性，默认为false，表示使用jdk动态代理织入增强，当配为<aop:aspectj-autoproxy  poxy-target-class="true"/>时，表示使用CGLib动态代理技术织入增强。不过即使proxy-target-class设置为false，如果目标类没有声明接口，则spring将自动使用CGLib动态代理。

## 11.4补充

### 1.普通实现

之前的内容中我们已经讲了前置通知、后置通知和环绕通知，接下来我们再补充一下通知的相关内容。

AfterHandle.java

```java
import org.springframework.aop.AfterReturningAdvice;

import java.lang.reflect.Method;

public class AfterHandle implements AfterReturningAdvice {

    @Override
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("后置通知");
    }
}
```

BrforeHandle.java

```java
import org.aopalliance.aop.Advice;
import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

public class BeforeHandle implements MethodBeforeAdvice {
    @Override
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println("我是前置通知");
    }
}

```

AfterThrowingHandle.java

```java
import org.aspectj.lang.annotation.AfterThrowing;
import org.springframework.aop.ThrowsAdvice;

public class AfterThrowingHandle implements ThrowsAdvice {

    public void afterThrowing(Exception ex){
        System.out.println("发生异常"+ex.getMessage());
    }
}
```

AfterFinalHandle.java

```java
import org.aspectj.lang.annotation.AfterThrowing;
import org.springframework.aop.AfterAdvice;
import org.springframework.aop.ThrowsAdvice;

import java.lang.reflect.Method;

public class AfterFinalHandle implements ThrowsAdvice {

    public void after(Object returnValue, Method method, Object[] args, Object target){
        System.out.println("最终通知");
    }
}
```

AroundHanle.java

```java
import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;

public class AroundHandle implements MethodInterceptor {
    @Override
    public Object invoke(MethodInvocation invocation) throws Throwable {
        
        Object proceed =null;
        try {
            System.out.println("前置通知");
            proceed=invocation.proceed();
            System.out.println("后置通知");
        }catch (Exception e){
            System.out.println("异常通知");
        }finally {
            System.out.println("最终通知");
        }

        return proceed;
    }
}

```

为了增加通知模式，就要写5个类太占用空间；因此需要创建一个切面

```java
import org.aopalliance.intercept.Joinpoint;
import org.aspectj.lang.ProceedingJoinPoint;

/**
 * 自定义切面
 */
public class MyAsepect {


    public void before(){
        System.out.println("前置通知");
    }

    public void afterReturing(){
        System.out.println("后置通知");
    }

    public void afterThrowing(){
        System.out.println("异常通知");
    }

    public void after(){
        System.out.println("最终通知");
    }


    //环绕通知=前置通知+后置通知
    //同时环绕通知可以调用原来的方法，，以此来修改参数，或是完善功能
    public Object around(ProceedingJoinPoint point){
        Object proceed=null;
        try {
            System.out.println("前置");
             proceed = point.proceed();
            System.out.println("后置");
        } catch (Throwable throwable) {
            throwable.printStackTrace();
            System.out.println("异常");
        } finally {
            System.out.println("最终");
        }
        return proceed;
    }
}
```

spring-context.xml

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean id="userService" class="com.qf.service.impl.UserServiceImpl"></bean>
    <!-- 前置通知-->
    <bean id="before" class="com.qf.aopHandel.BeforeHandle"></bean>
    <!-- 后置通知-->
    <bean id="after" class="com.qf.aopHandel.AfterHandle"></bean>
    <!-- 异常通知-->
    <bean id="throwing" class="com.qf.aopHandel.AfterThrowingHandle"></bean>
    <!-- 最终通知-->
    <bean id="afterfinal" class="com.qf.aopHandel.AfterFinalHandle"></bean>
    <!-- 环绕通知-->
    <bean id="aroundHandle" class="com.qf.aopHandel.AroundHandle"></bean>

    <!-- 将切面类交给spring-->
    <bean id="myAspect"  class="com.qf.aopAspect.MyAsepect"></bean>
    <!-- 配置aop-->

    <!--切面配置-->
    <aop:config>
        <!-- 前置通知 advice-ref="前置通知类"  pointcut="切点表达式"-->
		<aop:advisor advice-ref="before" pointcut="execution(* *.*.*.*.*(..))"></aop:advisor>
		<!--后置通知-->
		<aop:advisor advice-ref="after" pointcut="execution(* *.*.*.*.*(..))"></aop:advisor>
		<!--异常通知 与后置通知只会执行一个-->
		<aop:advisor advice-ref="throwing" pointcut="execution(* *.*.*.*.*(..))"></aop:advisor>
        <!--最终通知-->
		<aop:advisor advice-ref="afterfinal" pointcut="execution(* *.*.*.*.*(..))"></aop:advisor>
        <!--  环绕通知 -->
		<aop:advisor advice-ref="aroundHandle" pointcut="execution(* *.*.*.*.*(..))"></aop:advisor>
       	
        <!--切入点:配置一个切面-->
        <!--aspectj语言格式: execution(对哪些类的方法进行增强)-->
        <aop:pointcut id="myPoint" expression="execution(* *.*.*.*.*(..))"/>
        
        <!-- 自定义切面-->
        <aop:aspect ref="myAspect">
			<aop:before method="before" pointcut-ref="myPoint"></aop:before>
			<aop:after-returning method="afterReturing" pointcut-ref="myPoint"></aop:after-returning>
			<aop:after-throwing method="afterThrowing" pointcut-ref="myPoint"></aop:after-throwing>
			<aop:after method="after" pointcut-ref="myPoint"></aop:after>
            <aop:around method="around" pointcut-ref="myPoint"></aop:around>
        </aop:aspect>
        
    </aop:config>
</beans>
```

### 2.用接口实现

```java
//标注当前类是切面类
@Aspect
@Component
public class MyAspectAnno {

    //切点表达式
    @Pointcut(value = "execution(* *.*.service.impl.*.*(..))")
    public void pt1(){
    }

    @Before("pt1()")//括号里放切点表达式
    public void before(){
        System.out.println("前置通知");
    }
    @AfterReturning("pt1()")
    public void afterReturing(){
        System.out.println("后置通知");
    }
    @AfterThrowing("pt1()")
    public void afterThrowing(){
        System.out.println("异常通知");
    }
    @After("pt1()")
    public void after(){
        System.out.println("最终通知");
    }

    @Around("pt1()")
    public Object around(ProceedingJoinPoint point){
        Object proceed=null;
        try {
            System.out.println("前置");
            proceed = point.proceed();
            System.out.println("后置");//有异常不通知
        } catch (Throwable throwable) {
            throwable.printStackTrace();
            System.out.println("异常");
        } finally {
            System.out.println("最终");
        }
        return proceed;
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--扫描-->
<context:component-scan base-package="com.qf"></context:component-scan>
    <!--开启aop的注解 -->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>

 </beans>
```

## 11.5 bean的生命周期（待完善）

构造->注入属性 满足依赖 -> 后处理器前置过程 -> 初始化 -> 后处理器后置过程 -> 返回 -> 销毁

# 12.整合MyBatis

## 12.1步骤

1、导入相关jar包

junit

```xml
<dependency>
   <groupId>junit</groupId>
   <artifactId>junit</artifactId>
   <version>4.12</version>
</dependency>
```

mybatis

```xml
<dependency>
   <groupId>org.mybatis</groupId>
   <artifactId>mybatis</artifactId>
   <version>3.5.2</version>
</dependency>
```

mysql-connector-java

```xml
<dependency>
   <groupId>mysql</groupId>
   <artifactId>mysql-connector-java</artifactId>
   <version>5.1.47</version>
</dependency>
```

spring相关

```xml
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>5.1.10.RELEASE</version>
</dependency>
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-jdbc</artifactId>
   <version>5.1.10.RELEASE</version>
</dependency>
```

aspectJ AOP 织入器

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
   <groupId>org.aspectj</groupId>
   <artifactId>aspectjweaver</artifactId>
   <version>1.9.4</version>
</dependency>
```

mybatis-spring整合包 【重点】

```xml
<dependency>
   <groupId>org.mybatis</groupId>
   <artifactId>mybatis-spring</artifactId>
   <version>2.0.2</version>
</dependency>
```

配置Maven静态资源过滤问题！

```xml
<build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
```

2、编写配置文件

3、代码实现

## 12.2回忆MyBatis

**编写pojo实体类**

```java
public class User {

    private int id;
    private String name;
    private String pwd;

    public User() {
    }

    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}
```

**实现mybatis的配置文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <typeAliases>
        <typeAlias type="com.SpringTest.Pojo.User" alias="user"/>
    </typeAliases>
    
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url"
                          value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf8&amp;useSSL=false&amp;serverTimezone=UTC&amp;allowPublicKeyRetrieval=true"/>
                <property name="username" value="root"/>
                <property name="password" value="132736602xm824"/>
            </dataSource>
        </environment>
    </environments>


    <mappers>
        <mapper resource="com/SpringTest/mapper/UserMapper.xml"/>
    </mappers>
</configuration>
```

**UserDao接口编写**

```java
public interface UserMapper {

    public List<User> getUser();

}
```

**接口对应的Mapper映射文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.SpringTest.mapper.UserMapper">

    <select id="getUser" resultType="user">
        select * from mybatis.user;
    </select>

</mapper>
```

**测试类**

```java
    @Test
    public void test() throws IOException {
        String resources = "mybatis-config.xml";
        InputStream stream = Resources.getResourceAsStream(resources);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(stream);
        SqlSession sqlSession = sqlSessionFactory.openSession(true);

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> users = mapper.getUser();

        for (User user : users) {
            System.out.println(user);
        }

        sqlSession.close();
    }
```

## 12.3MyBatis-Spring学习

引入Spring之前需要了解mybatis-spring包中的一些重要类；

http://www.mybatis.org/spring/zh/index.html

**什么是 MyBatis-Spring？**

MyBatis-Spring 会帮助你将 MyBatis 代码无缝地整合到 Spring 中。

**知识基础**

在开始使用 MyBatis-Spring 之前，你需要先熟悉 Spring 和 MyBatis 这两个框架和有关它们的术语。这很重要

MyBatis-Spring 需要以下版本：

| MyBatis-Spring | MyBatis | Spring 框架 | Spring Batch | Java    |
| :------------- | :------ | :---------- | :----------- | :------ |
| 2.0            | 3.5+    | 5.0+        | 4.0+         | Java 8+ |
| 1.3            | 3.4+    | 3.2.2+      | 2.1+         | Java 6+ |

如果使用 Maven 作为构建工具，仅需要在 pom.xml 中加入以下代码即可：

```xml
<dependency>
   <groupId>org.mybatis</groupId>
   <artifactId>mybatis-spring</artifactId>
   <version>2.0.2</version>
</dependency>
```

要和 Spring 一起使用 MyBatis，需要在 Spring 应用上下文中定义至少两样东西：一个 SqlSessionFactory 和至少一个数据映射器类。

在 MyBatis-Spring 中，可使用SqlSessionFactoryBean来创建 SqlSessionFactory。要配置这个工厂 bean，只需要把下面代码放在 Spring 的 XML 配置文件中：

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
 <property name="dataSource" ref="dataSource" />
</bean>
```

注意：SqlSessionFactory需要一个 DataSource（数据源）。这可以是任意的 DataSource，只需要和配置其它 Spring 数据库连接一样配置它就可以了。

在基础的 MyBatis 用法中，是通过 SqlSessionFactoryBuilder 来创建 SqlSessionFactory 的。而在 MyBatis-Spring 中，则使用 SqlSessionFactoryBean 来创建。

在 MyBatis 中，你可以使用 SqlSessionFactory 来创建 SqlSession。一旦你获得一个 session 之后，你可以使用它来执行映射了的语句，提交或回滚连接，最后，当不再需要它的时候，你可以关闭 session。

SqlSessionFactory有一个唯一的必要属性：用于 JDBC 的 DataSource。这可以是任意的 DataSource 对象，它的配置方法和其它 Spring 数据库连接是一样的。

一个常用的属性是 configLocation，它用来指定 MyBatis 的 XML 配置文件路径。它在需要修改 MyBatis 的基础配置非常有用。通常，基础配置指的是 < settings> 或 < typeAliases>元素。

需要注意的是，这个配置文件并不需要是一个完整的 MyBatis 配置。确切地说，任何环境配置（< environments >），数据源（< DataSource >）和 MyBatis 的事务管理器（< transactionManager >）都会被忽略。SqlSessionFactoryBean 会创建它自有的 MyBatis 环境配置（Environment），并按要求设置自定义环境的值。

SqlSessionTemplate 是 MyBatis-Spring 的核心。作为 SqlSession 的一个实现，这意味着可以使用它无缝代替你代码中已经在使用的 SqlSession。

模板可以参与到 Spring 的事务管理中，并且由于其是线程安全的，可以供多个映射器类使用，你应该总是用 SqlSessionTemplate 来替换 MyBatis 默认的 DefaultSqlSession 实现。在同一应用程序中的不同类之间混杂使用可能会引起数据一致性的问题。

可以使用 SqlSessionFactory 作为构造方法的参数来创建 SqlSessionTemplate 对象。

```xml
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
 <constructor-arg index="0" ref="sqlSessionFactory" />
</bean>
```

现在，这个 bean 就可以直接注入到你的 DAO bean 中了。你需要在你的 bean 中添加一个 SqlSession 属性，就像下面这样：

```java
public class UserDaoImpl implements UserDao {

 private SqlSession sqlSession;

 public void setSqlSession(SqlSession sqlSession) {
   this.sqlSession = sqlSession;
}

 public User getUser(String userId) {
   return sqlSession.getMapper...;
}
}
```

按下面这样，注入 SqlSessionTemplate：

```xml
<bean id="userDao" class="org.mybatis.spring.sample.dao.UserDaoImpl">
 <property name="sqlSession" ref="sqlSession" />
</bean>
```

## 12.4整合实现一

1、引入Spring配置文件spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    
</beans>
```

2、配置数据源替换mybaits的数据源

```xml
<!--配置数据源：数据源有非常多，可以使用第三方的，也可使使用Spring的-->
<!--DaraSource:使用Spring的数据源代替MyBatis的配置 C3P0 dbcp druid
我们这里使用Spring提供的JDBC-->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
    <property name="url"
              value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf8&amp;useSSL=false&amp;serverTimezone=UTC&amp;allowPublicKeyRetrieval=true"/>
    <property name="username" value="root"/>
    <property name="password" value="132736602xm824"/>
</bean>
```

3、配置SqlSessionFactory，关联MyBatis

```xml
<!--配置SqlSessionFactory-->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <!--绑定mybatis配置文件-->
    <property name="configLocation" value="classpath:mybatis-config.xml"/>
    <property name="mapperLocations" value="classpath:com/SpringTest/mapper/*.xml"/>
</bean>
```

4、注册sqlSessionTemplate，关联sqlSessionFactory；

```xml
<!--SqlSessionTemplate就是我们使用的SqlSession，注册sqlSessionTemplate , 关联sqlSessionFactory-->
<bean id="SqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    <!--只能使用构造器注入sqlSessionFactory，因为他没有set方法-->
    <constructor-arg index="0" ref="sqlSessionFactory"/>
</bean>
```

5、增加Dao接口的实现类；私有化sqlSessionTemplate

```java
public class UserMapperImpl implements UserMapper{

    //之前所有的操作都是用sqlSession来执行，现在都由SqlSessionTemplate，sqlSession不用我们自己创建了，Spring来管理
    private SqlSessionTemplate sqlSession;

    public void setSqlSession(SqlSessionTemplate sqlSession) {
        this.sqlSession = sqlSession;
    }

    public List<User> getUser() {
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.getUser();
    }

}
```

6、注册bean实现

```xml
<bean id="userMapper" class="com.SpringTest.mapper.UserMapperImpl">
    <property name="sqlSession" ref="SqlSession"/>
</bean>
```

7、测试

```java
@Test
public void test() throws IOException {
    
    ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
    
    UserMapper mapper = context.getBean("userMapper", UserMapper.class);
    
    List<User> users = mapper.getUser();
    
    for (User user : users) {
        System.out.println(user);
    }
    
}
```

结果成功输出！现在我们的Mybatis配置文件的状态！发现都可以被Spring整合！

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <typeAliases>
        <package name="com.SpringTest.Pojo"/>
    </typeAliases>

</configuration>
```

spring-dao,xml(可活用)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--DaraSource:使用Spring的数据源代替MyBatis的配置 C3P0 dbcp druid
    我们这里使用Spring提供的JDBC-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url"
                  value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf8&amp;useSSL=false&amp;serverTimezone=UTC&amp;allowPublicKeyRetrieval=true"/>
        <property name="username" value="root"/>
        <property name="password" value="132736602xm824"/>
    </bean>

    <!--配置SqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--绑定mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/SpringTest/mapper/*.xml"/>
    </bean>

    <!--SqlSessionTemplate就是我们使用的SqlSession，注册sqlSessionTemplate , 关联sqlSessionFactory-->
    <bean id="SqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!--只能使用构造器注入sqlSessionFactory，因为他没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>

</beans>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <import resource="spring-dao.xml"/>

    <!--注册bean实现-->
    <bean id="userMapper" class="com.SpringTest.mapper.UserMapperImpl">
        <property name="sqlSession" ref="SqlSession"/>
    </bean>

</beans>
```

## 12.5整合实现二

mybatis-spring1.2.3版以上的才有这个 .

官方文档截图 :

dao继承Support类 , 直接利用 getSqlSession() 获得 , 然后直接注入SqlSessionFactory . 比起方式1 , 不需要管理SqlSessionTemplate , 而且对事务的支持更加友好 . 可跟踪源码查看

![image-20211012203812105](SpringPicture.assets\image-20211012203812105.png)

测试：

1、将我们上面写的UserDaoImpl修改一下（第二个类实现了UserMapper

，并继承了SqlSessionDaoSupport）

```java
public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper {

    public List<User> getUser() {
        return getSqlSession().getMapper(UserMapper.class).getUser();
    }
    
}
```

2、修改bean的配置

```xml
<bean id="userMapper2" class="com.SpringTest.mapper.UserMapperImpl2">
    <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
</bean>
```

3、测试

```java
@Test
public void test02() throws IOException {
    ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

    UserMapper mapper = context.getBean("userMapper2", UserMapper.class);

    List<User> users = mapper.getUser();

    for (User user : users) {
        System.out.println(user);
    }
}
```

spring-dao,xml(可活用)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--DaraSource:使用Spring的数据源代替MyBatis的配置 C3P0 dbcp druid
    我们这里使用Spring提供的JDBC-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url"
                  value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf8&amp;useSSL=false&amp;serverTimezone=UTC&amp;allowPublicKeyRetrieval=true"/>
        <property name="username" value="root"/>
        <property name="password" value="132736602xm824"/>
    </bean>

    <!--配置SqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--绑定mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/SpringTest/mapper/*.xml"/>
    </bean>

</beans>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <import resource="spring-dao.xml"/>

    <!--注册bean实现-->
    <bean id="userMapper" class="com.SpringTest.mapper.UserMapperImpl">
        <property name="sqlSession" ref="SqlSession"/>
    </bean>

    <bean id="userMapper2" class="com.SpringTest.mapper.UserMapperImpl2">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>

</beans>
```

**总结 : 整合到spring以后可以完全不要mybatis的配置文件，除了这些方式可以实现整合之外，我们还可以使用注解来实现，这个等我们后面学习SpringBoot的时候还会测试整合！**

## 过程

**pojo -> XXXMapper -> XXXMapper.xml -> mybatis-config.xml整合到spring-dao.xml -> XXXMapperImpl(用来注入bean) -> 注入到applicationContext.xml中 ->... **

## 12.6 Druid的管理工具

打开tomcat服务器后，进入http://localhost:8080/druid/sql.html

# 13.声明式事务

## 13.1 回顾事务

- 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎！
- 事务管理是企业级应用程序开发中必备技术，用来确保数据的完整性和一致性。

- 事务就是把一系列的动作当成一个独立的工作单元，这些动作要么全部完成，要么全部不起作用。


**事务四个属性ACID**

- 原子性（atomicity）
  - 事务是原子性操作，由一系列动作组成，事务的原子性确保动作要么全部完成，要么完全不起作用

- 一致性（consistency）
  - 一旦所有事务动作完成，事务就要被提交。数据和资源处于一种满足业务规则的一致性状态中

- 隔离性（isolation）
  - 可能多个事务会同时处理相同的数据，因此每个事务都应该与其他事务隔离开来，防止数据损坏

- 持久性（durability）
  - 事务一旦完成，无论系统发生什么错误，结果都不会受到影响。通常情况下，事务的结果被写到持久化存储器中

## 13.2 测试

将上面的代码拷贝到一个新项目中

在之前的案例中，我们给userDao接口新增两个方法，删除和增加用户；

```java
public interface UserMapper {

    List<User> selectUser();

    //添加一个用户
    int addUser(User user);

    //根据id删除用户
    int deleteUser(int id);

}
```

mapper文件，我们故意把 deletes 写错，测试！

```xml
<mapper namespace="com.SpringTest.Mapper.UserMapper">

    <select id="selectUser" resultType="user">
        select *
        from mybatis.user;
    </select>

    <insert id="addUser" parameterType="user">
        insert into mybatis.user(id, name, pwd)
        VALUES (#{id}, #{name}, #{pwd});
    </insert>

    <delete id="deleteUser" parameterType="int">
        deletes
        from mybatis.user
        where id = #{id};
    </delete>

</mapper>
```

编写接口的实现类，在实现类中，我们去操作一波

```java
public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper {

    //增加一些操作
    public List<User> selectUser() {
        User user = new User(5,"小明","123456");
        UserMapper mapper = getSqlSession().getMapper(UserMapper.class);
        mapper.addUser(user);
        mapper.deleteUser(5);
        return getSqlSession().getMapper(UserMapper.class).selectUser();
    }

    //新增
    public int addUser(User user) {
        return getSqlSession().getMapper(UserMapper.class).addUser(user);
    }

    //删除
    public int deleteUser(int id) {
        return getSqlSession().getMapper(UserMapper.class).deleteUser(id);
    }

}
```

测试

```java
@Test
public void SelectUserTest(){
    ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
    UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
    
    List<User> users = userMapper.selectUser();
   
    for (User user : users) {
        System.out.println(user);
    }
}
```

报错：sql异常，delete写错了

结果 ：插入成功！

没有进行事务的管理；我们想让他们都成功才成功，有一个失败，就都失败，我们就应该需要**事务！**

以前我们都需要自己手动管理事务，十分麻烦！

但是Spring给我们提供了事务管理，我们只需要配置即可；

## 13.3 Spring中的事务管理

Spring在不同的事务管理API之上定义了一个抽象层，使得开发人员不必了解底层的事务管理API就可以使用Spring的事务管理机制。Spring支持编程式事务管理和声明式的事务管理。

### **编程式事务管理**

- 将事务管理代码嵌到业务方法中来控制事务的提交和回滚
- 缺点：必须在每个事务操作业务逻辑中包含额外的事务管理代码

### **声明式事务管理**

- 一般情况下比编程式事务好用。
- 将事务管理代码从业务方法中分离出来，以声明的方式来实现事务管理。
- 将事务管理作为横切关注点，通过aop方法模块化。Spring中通过Spring AOP框架支持声明式事务管理。

**使用Spring管理事务，注意头文件的约束导入 : tx**

```xml
xmlns:tx="http://www.springframework.org/schema/tx"

http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd">
```

**事务管理器**

- 无论使用Spring的哪种事务管理策略（编程式或者声明式）事务管理器都是必须的。
- 就是 Spring的核心事务管理抽象，管理封装了一组独立于技术的方法。

**JDBC事务**

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

**配置好事务管理器后我们需要去配置事务的通知**

```xml
    <!--结合AOP实现事务的切入-->
    <!--配置事务通知-->
    <tx:advice id="txadvice" transaction-manager="transactionManager">
        <!--给哪些方法配置事务-->
        <!--
        isolation 事务的隔离级别： 如果值是默认或者DEFAUILT的话，与数据库的隔离级别一致，默认就行

        propagation  配置事务的传播特性 ：
            SIPPORTS:（适合查询）
                不存在外部事务，则不开启新事务
                    eg. 在A中调用B方法 A没有事务 B有事务 B事务失效
                        如果这个方法被没有事务的方法调用，则事务失效
                若存在外部事务，则合并到外部事务中
                    eg. A有事务，B有事务，B合并到A
            REQUIRED：（适合增删改）
                不存在外部事务，则开启新事务
                存在外部事务，则合并到外部事务中

        timeout 等待超时时间：-1走的是数据库的设置，或者可以自己设置时间 单位：秒

        no-rollback-for 发生设置的异常时，不回滚

        read-only 只读

        -->
        <!---->
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED" isolation="DEFAULT"
                       timeout="-1"
                       no-rollback-for="ArithmeticException" />
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="query"  propagation="SUPPORTS" read-only="true"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

```

**tx:method中的配置说明**

> isolation 事务的隔离级别： 如果值是默认或者DEFAUILT的话，与数据库的隔离级别一致，默认就行
>
> propagation  配置事务的传播特性 ：
>     SIPPORTS:（适合查询）
>         不存在外部事务，则不开启新事务
>             eg. 在A中调用B方法 A没有事务 B有事务 B事务失效
>                 如果这个方法被没有事务的方法调用，则事务失效
>         若存在外部事务，则合并到外部事务中
>             eg. A有事务，B有事务，B合并到A
>     REQUIRED：（适合增删改）
>         不存在外部事务，则开启新事务
>         存在外部事务，则合并到外部事务中
>
> timeout 等待超时时间：-1走的是数据库的设置，或者可以自己设置时间 单位：秒
>
> no-rollback-for 发生设置的异常时，不回滚
>
> read-only 只读

**spring事务传播特性：**

事务传播行为就是多个事务方法相互调用时，事务如何在这些方法间传播。spring支持7种事务传播行为：

- propagation_requierd：如果当前没有事务，就新建一个事务，如果已存在一个事务中，加入到这个事务中，这是最常见的选择。（Spring默认选择）（适合增删改）
- propagation_supports：支持当前事务，如果没有当前事务，就以非事务方法执行。（适合查询）
- propagation_mandatory：使用当前事务，如果没有当前事务，就抛出异常。
- propagation_required_new：新建事务，如果当前存在事务，把当前事务挂起。
- propagation_not_supported：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
- propagation_never：以非事务方式执行操作，如果当前事务存在则抛出异常。
- propagation_nested：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与propagation_required类似的操作

Spring 默认的事务传播行为是 PROPAGATION_REQUIRED，它适合于绝大多数的情况。

假设 ServiveX#methodX() 都工作在事务环境下（即都被 Spring 事务增强了），假设程序中存在如下的调用链：Service1#method1()->Service2#method2()->Service3#method3()，那么这 3 个服务类的 3 个方法通过 Spring 的事务传播机制都工作在同一个事务中。

就好比，我们刚才的几个方法存在调用，所以会被放在一组事务当中！

**事务的隔离级别（isolation）：**

| 名称            | 描述                                   |
| --------------- | -------------------------------------- |
| default         | （默认值）采用数据库默认的设置（建议） |
| read-uncommited | 读未提交                               |
| read-communited | 读提交（Oracle数据库默认的隔离级别）   |
| repeatable      | 可重复度（MySQL数据库默认的隔离级别）  |
| serialized      | 序列化读                               |

**并发问题**

| 问题       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| 脏读       | 一个事物读取另一个事务还未提交的数据，大于等于read-commited可防止 |
| 不可重复读 | 一个事物内多次读取一行数据的相同内容，其结果不一致，大于等于repeatable-read可防止 |
| 幻读       | 一个事务内多次读取同一张表中的相同内容，其结果不一致。serialized-read可防止 |

**配置AOP**

导入aop的头文件！

```xml
<!--配置事务切入-->
<aop:config>
    <aop:pointcut id="txPointCut" expression="execution(* com.SpringTest.Mapper.*.*(..))"/>
    <aop:advisor advice-ref="txadvice" pointcut-ref="txPointCut"/>
</aop:config>
```

**进行测试**

删掉刚才插入的数据，再次测试！

```java
@Test
public void SelectUserTest(){
    ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
    UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
    
    List<User> users = userMapper.selectUser();
   
    for (User user : users) {
        System.out.println(user);
    }
}
```

配置事务完整代码

```xml
<!--配置声明式事务-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!--结合AOP实现事务的切入-->
<!--配置事务通知-->
<tx:advice id="txadvice" transaction-manager="transactionManager">
    <!--给哪些方法配置事务-->
    <!--配置事务的传播特性 propagation-->
    <tx:attributes>
        <tx:method name="add" propagation="REQUIRED"/>
        <tx:method name="delete" propagation="REQUIRED"/>
        <tx:method name="query" read-only="true"/>
        <tx:method name="update" propagation="REQUIRED"/>
        <tx:method name="*" propagation="REQUIRED"/>
    </tx:attributes>
</tx:advice>

<!--配置事务切入-->
<aop:config>
    <aop:pointcut id="txPointCut" expression="execution(* com.SpringTest.Mapper.*.*(..))"/>
    <aop:advisor advice-ref="txadvice" pointcut-ref="txPointCut"/>
</aop:config>
```

spring-dao.xml(可活用)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--DaraSource:使用Spring的数据源代替MyBatis的配置 C3P0 dbcp druid
    我们这里使用Spring提供的JDBC-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url"
                  value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf8&amp;useSSL=false&amp;serverTimezone=UTC&amp;allowPublicKeyRetrieval=true"/>
        <property name="username" value="root"/>
        <property name="password" value="132736602xm824"/>
    </bean>

    <!--配置SqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--绑定mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/SpringTest/Mapper/*.xml"/>
    </bean>

    <!--SqlSessionTemplate就是我们使用的SqlSession，注册sqlSessionTemplate , 关联sqlSessionFactory-->
    <bean id="SqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!--只能使用构造器注入sqlSessionFactory，因为他没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>

    <!--配置声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!--结合AOP实现事务的切入-->
    <!--配置事务通知-->
    <tx:advice id="txadvice" transaction-manager="transactionManager">
        <!--给哪些方法配置事务-->
        <!--配置事务的传播特性 propagation-->
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

    <!--配置事务切入-->
    <aop:config>
        <aop:pointcut id="txPointCut" expression="execution(* com.SpringTest.Mapper.*.*(..))"/>
        <aop:advisor advice-ref="txadvice" pointcut-ref="txPointCut"/>
    </aop:config>

</beans>
```

## 13.4思考问题

为什么需要配置事务？

- 如果我们不在Spring中配置声明式事务，就需要我们手动提交控制事务；
- 如果不配置事务，可能出现数据提交不一致的情况；
- 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎！

## 13.5 举例——转账

数据库信息

![image-20211019230550605](SpringPicture.assets\image-20211019230550605.png)

Transfer.java

```java
import lombok.Data;

@Data
public class Transfer {

    private Integer id;

    private String transferName;

    private Double price;
}
```

TransferDao.java 

```java
@Repository
public interface TransferDao {

    int  receiveMoney(@Param("transferName")String name,@Param("price")Double pice);

    int addMoney(@Param("TotransferName")String name,@Param("price")Double pice);
}
```

TransterMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.qf.dao.TransferDao">

    <update id="receiveMoney" >
        update tb_transfer set money=money-#{price} where tranfer_name=#{transferName}
    </update>

    <update id="addMoney" >
        update tb_transfer set money=money+#{price} where tranfer_name=#{TotransferName}
    </update>
</mapper>
```

TransferService.java

```java
public interface TransferService {

    Boolean transfer(String tansferName,String toTransferName,Double money);
}
```

TransferServiceImpl.java（配置版）

```java
@Service
public class TransferServiceImpl  implements TransferService {

    @Override
    public Boolean transfer(String tansferName, String toTransferName, Double money) {
       	transferDao.receiveMoney(tansferName,money);
        transferDao.addMoney(toTransferName,money);
        return true;
    }
}
```

TransferServiceImpl.java（注解版）

```java
@Service
@Transactional//加入注解 
public class TransferServiceImpl  implements TransferService {

    @Autowired
    TransferDao  transferDao;

    @Override
    public Boolean transfer(String tansferName, String toTransferName, Double money) {
       	transferDao.receiveMoney(tansferName,money);
        transferDao.addMoney(toTransferName,money);
        return true;
    }
}
```

TestTransfer.java

```java
public class TestTransfer {

    @Autowired
    TransferService transferService;

    @Test
    public void  transfer(){
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring-context.xml");
        Transferervice bean = context.getBean(TransferService.class);
        Boolean transfer = transferService.transfer("张老师", "苗老师", 500.0);
        System.out.println(transfer);
    }
}
```

spring-context.xml（配置版）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd">

<context:component-scan base-package="com.qf"></context:component-scan>
    <!--开启aop的注解 -->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>

    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
    <!-- dataSource-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${driver}"></property>
        <property name="url" value="${url}"></property>
        <property name="username" value="${name}"></property>
        <property name="password" value="${password}"></property>
    </bean>

    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        <property name="typeAliasesPackage" value="com.qf.pojo"></property>
        <property name="mapperLocations" value="classpath:mapper/*Mapper.xml"></property>
    </bean>

    <bean id="mapperScannerConfigurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
        <property name="basePackage" value="com.qf.dao"></property>
    </bean>

    <!-- 引入事务管理器-->
    <bean id="dataSourceTransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
	
    <!--事务控制-->
 	<tx:advice transaction-manager="dataSourceTransactionManager" id="tx">
   		<tx:attributes>
        	<!--transfer 发生异常以后 进行回滚操作-->
            <tx:method name="transfer" rollback-for="Exception"/>-->
        </tx:attributes>
    </tx:advice>
    
    <aop:config>
       	<!--交给aop来进行回滚-->
       	<aop:advisor advice-ref="tx" pointcut="execution(* *.*.service.impl.*.*(..))"></aop:advisor>
    </aop:config>

</beans>
```

spring-context.xml（注解版）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd">

<context:component-scan base-package="com.qf"></context:component-scan>
    
    <!--开启aop的注解 -->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>

    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
    <!-- dataSource-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${driver}"></property>
        <property name="url" value="${url}"></property>
        <property name="username" value="${name}"></property>
        <property name="password" value="${password}"></property>
    </bean>

    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        <property name="typeAliasesPackage" value="com.qf.pojo"></property>
        <property name="mapperLocations" value="classpath:mapper/*Mapper.xml"></property>
    </bean>

    <bean id="mapperScannerConfigurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
        <property name="basePackage" value="com.qf.dao"></property>
    </bean>

    <!-- 引入事务管理器-->
    <bean id="dataSourceTransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!--开启注解管理异常-->
    <tx:annotation-driven transaction-manager="dataSourceTransactionManager"></tx:annotation-driven>

</beans>
```

Spring管理异常，是根据抛出的异常进行处理。当异常被处理（try{}catch）时，Spring则不会处理

**利用注解直接创建IOC对象**

导入依赖

```xml
<dependency>
   	<groupId>org.springframework</groupId>
   	<artifactId>spring-test</artifactId>
    <version>5.1.6.RELEASE</version>
</dependency>
```

```java
@RunWith(SpringRunner.class)
@ContextConfiguration("classpath:spring-context-anno.xml")
/*
相当于这两步操作
 ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring-context.xml");
        Transferervice bean = context.getBean(TransferService.class);
*/
public class TestTransfer {

    //因为注解已将将IOC对象创建，所以可以直接获取到被管理的对象
    @Autowired
    TransferService transferService;

    @Test
    public void  transfer(){
        Boolean transfer = transferService.transfer("张老师", "苗老师", 500.0);
        System.out.println(transfer);
    }
}
```

## 13.6 隔离级别

###  读未提交 脏读

新建一个查询，一个事务

```sql
select @@tx_isolation;
SELECT @@autocommit;
set AutoCommit=0; -- 0 手动提交

set SESSION TRANSACTION ISOLATION LEVEL read UNCOMMITTED;

START TRANSACTION;

UPDATE t_student set sname='000' where sid=1 ;
commit;
ROLLBACK;
```

再建立一个查询，另一个事务

```sql
select @@tx_isolation;
SELECT @@autocommit;
set AutoCommit=0; -- 0 手动提交

set SESSION TRANSACTION ISOLATION LEVEL read UNCOMMITTED;

START TRANSACTION;

select * from t_student where sid=1;

commit;
ROLLBACK; 
```

### 读已提交，不可重复读 幻读

新建一个查询

```sql
select @@tx_isolation;
SELECT @@autocommit;
set AutoCommit=0; -- 0 手动提交
set SESSION TRANSACTION ISOLATION LEVEL read COMMITTED;
START TRANSACTION;

UPDATE t_student set sname='000' where sid=1 ;

commit;
ROLLBACK;
```

再建一个查询

```sql
select @@tx_isolation;
SELECT @@autocommit;
set AutoCommit=0; -- 0 手动提交

set SESSION TRANSACTION ISOLATION LEVEL read COMMITTED;
START TRANSACTION;

select * from t_student where sid=1;
UPDATE t_student set sex='女' where sid=1 ;
select * from t_student where sid=1;
```

### 可重复读（MySQL默认）

### serializable:串行化

新建一个查询

```sql
select @@tx_isolation;

set SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;

start TRANSACTION

select * from t_student_test ; 
insert into t_student_test(id,`name`) VALUES(4444,"aaa");
commit;
```

再建一个查询

```sql
select @@tx_isolation;

set SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;

start TRANSACTION

select * from t_student_test ; 
commit;
```

注意：隔离级别从小到大安全性越来越高，但是效率越来越低

# 14、补充——Spring入门

## 14.1 模拟Spring

### FactoryConfig.java工具类的创建

#### 问题：

测试类

```java
@Test
public void test01(){
    UserService userService = new UserServiceImpl();
    String name = userService.getName();
    System.out.printLn(name);
}
```

**以上这种创建方式，当要调用多个Service层对象时，会声明过多的对象，占用了较多的内存空间。**

#### 解决方法：

bean.properties

```properties
userService=com.Spring.serviceImpl.UserServiceImpl
```

FactoryConfig.java

```java
public class FactoryConfig(){
    private Properties properties = new Properties();
    
    //key是beanName,value是当前对象的实例
    private static Map map = new HashMap();
    
    public FactoryConfig(){}
    
    //获取properties文件
    public FactoryConfig(String config){
       try{ 	
           //加载properties配置文件
			properties.load(FactoryConfig.class.getClassLoader().getResourceAsStream(config));
          }catch(IoException e){
           e.printStackTrace();
       }
    }
    
    //对象的实例化
    public Object getBean(String beanName){
        String property = properties.getProperty(beanName);
        if(properties!=null){
            Class<?> aClass = Class.forName(property);
            if(aClass!=null){
                //为了确保生成的当前类实例唯一，进来后进行判断
                Boolean singleTon = isSingleTon(beanName);
                if(singleTon){
                    //通过构造器对象创建当前类的实例
                    Object o = aClass.newInstance();
                    map.put(beanName,o);
                    return o;
                }
                Object o = map.get(beanName);
                return o;
            }
        }
        return null;
    }
    
    //判断是否
    public Boolean isSingleTon(String beanName){
        Object o = map.get(beanName);
        //判断key:value中value是否为空
        if (o==null)
            //为空则说明不存在,创建实例后加入到map的value中
            return true;
        //不为空这说明存在，直接获取value
        return false;
    }
}
```

测试类

```java
public void Test02()throws Exception{
	FactoryConfig factoryConfig = new FactoryConfig("bean.properties");
    UserService userService1 = (UserSerice)factorConfig.getBean("userService");
    UserService userService2 = (UserSerice)factorConfig.getBean("userService");
    
    String name = userService1.getName();
    System.out.printLn(name);
    System.out.printLn(userService1==userService2);//true
}
```

## 14.2 构造器注入(较常用)

```xml
<bean id="user" class="com.Spring.Pojo.User">
	<constructor-arg name="id" value="1" />
    <constructor-arg name="name" value="大毛" />
    <constructor-arg name="age" value="18" />
    <constructor-arg name="sex" value="男" />
</bean>
```

三种注入方式：

```xml
    <!-- 实例化/初始化对象
    初始化对象时 三种赋值方式
    1.get/set方式赋值，当前对象中必须有属性的get/set方法 <property name="属性名" value="值">
    2.构造器赋值。<constructor-arg name="属性名" value="值">
    3.p标签 p:属性值=value
    -->
<!--get/set-->
    <bean id="tbUser" class="com.Spring.pojo.TbUser">
        <property name="id" value="1"></property>
        <property name="name" value="张宁"></property>
        <property name="age" value="2"></property>
    	<property name="tbUserInfo" ref="tbUserInfo1"></property>
    </bean>
	<bean id="tbUserInfo1" class="com.Spring.pojo.TbUserInfo" p:addr="南窑头国际城" p:idCard="1111" p:phone="134" p:userId="1"></bean>

<!--构造器-->
	<bean id="tbUser" class="com.Spring.pojo.TbUser">
		<constructor-arg name="id" value="1"></constructor-arg>
 		<constructor-arg name="name" value="扎根"></constructor-arg>
       	<constructor-arg name="tbUserInfo" ref="tbUserInfo2"></constructor-arg>
	</bean>
    <bean id="tbUserInfo2" class="com.qf.pojo.TbUserInfo" p:addr="南窑头国际城" p:idCard="1111" p:phone="134" p:userId="1"></bean>

   <!--c/p标签-->
	<bean id="tbUser" class="com.Spring.pojo.TbUser"  p:name="张三" p:age="11" p:id="12" p:tbUserInfo-ref="tbUserInfo3"></bean>-->
    <bean id="tbUserInfo3" class="com.Spring.pojo.TbUserInfo" p:addr="南窑头国际城" p:idCard="1111" p:phone="134" p:userId="1"></bean>
```

## 14.3 DI

当前以来的对象必须全部交由spring管理，如果spring不管理该对象，则无法进行注入操作，会爆出空指针异常（NullpointException)

``` java
public interface UserDao(){
    String userName();
}
```

```java
public class UserDaoImpl implements UserDao(){    
    public String userName(){
        return "java";
    }  
}
```

```java
public class UserService(){
    String getName();
}
```

```java
public calss UserServiceImpl implements UserService(){
    
    public String getName(){
        UserDao userDao = new UserDaoImpl();
        return userDao.userName;
    }
}
```

**但是对象的声明应该由Spring去做**

```java
public calss UserServiceImpl implements UserService(){
    
    private UserDao userDao;
    
    public UserDao getUserDao(){
        return userDao;
    }
    
    public void setUserDao(UserDao userDao){
        this.userDao = userDao;
    }
    
    public String getName(){
        return userDao.userName;
    }
}
```

Spring-context.xml

```xml
<bean id="userSerice" class="com.Spring.service.impl.UserServiceImpl">
    <property name="userDao" ref="userDao"></property>
</bean>
```

```xml
<bean id="userDao" class="com.Spring.dao.UserDao">
</bean>
```

将UserDao注入到UserService中

## 14.4 bean属性

### 1.init-method

对象初始化时执行的方法

```xml
<bean id="userDao" class="com.Spring.dao.UserDao" init-method="">
</bean>
```

### 2.destory-method

在销毁时执行的方法

```xml
<bean id="userDao" class="com.Spring.dao.UserDao" init-method="" destory-method="">
</bean>
```

且需要手动在测试类中关闭

```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

context.close();
```

### 3.scope

表示当前对象生命是是单例还是多例 默认是单例

> 单例：当前IOC容器创建时就已经创建完成
>
> 多例：什么时候使用，什么时候才创建
>
> singleton 表示单例
>
> prototype 表示多例

### 4.lazy-init

延时创建对象，单例模式下，不想直接创建；当我们使用时才创建

### 5.primay

当一个接口有两个相同的实现类，需要一个唯一表示

## 14.5 Spring工厂的特性

### 1.饿汉式创建优势

工厂创建之后，会将Spring配置中的所有对象都创建完成（饿汉式）

提高程序运行效率。避免多次IO，减少对象创建时间。(概念接近连接池，一次性创建好，使用直接获取)

### 2.生命周期方法

- 自定义初始化方法:添加"init-method"属性， Spring则会在创建对象之后，调用此方法。
- 自定义销毁方法:添加“destroy-method"属性， Spring则会在销毁对象之前，调用此方法。
- 销毁:工厂的close()方法被调用之后，Spring会毁掉所有已创建的单例对象。
- 分类: Singleton对象由Spring容器销毁、Prototype对象由JVM销毁。

### 3.生命周期注解

```java
import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

@PostConstruct //初始化
public void init(){
	System. out. println("init method executed");
}

@PreDestroy //销毁
public void destroy(){
    System. out. println("destroy method executed") ;
}
```

### 4.生命周期阶段

> 单例bean: singleton
>
> 随工厂启动创建==》构造方法 ==》set方法(注入值) ==》init(初始化) ==》构建完成 ==》随工厂关闭销毁

> 多例bean: prototype
>
> 被使用时创建== 》构造方法== 》set方法(注入值) == 》init(初始化) == 》构建完成== 》JVM垃圾回收销毁

## 14.6 测试

### 1 导入依赖

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>4.3.6.RELEASE</version>
</dependency>
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
```

### 2 编码

> 可以免去工厂的创建过程；
>
> 可以直接将要测试的组件注入到测试类。

```java
@RunWith(SpringJUnit4ClassRunner.class) //由SpringJUnit4ClassRunner启动测试
@ContextConfiguration("classpath:applicationContext.xml") //spring的配置文件位置
public class SpringTest{//当前测试类也会被纳入工厂中，所以其中属性可以注入

    @Autowired // 注入要测试的组件
    @Qualifier("userDAO")
    private UserDAO userDAO;

    @Test
    public void test(){
        // 测试使用userDAO
        userDAO.queryUser();
        ....
    }
}
```

## 14.7 Druid管理工具

web.xml配置

```xml
<servlet>
	<servlet-name>DruidstatView</servlet-name>
	<servlet-class>com.alibaba.druid.support.http.statViewServlet</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>DruidstatView</servlet-name>
	<url-pattern>/druid/*</ur1-pattern>
</servlet-mapping>
```

配置tomcat，并访问http://localhost:8888/druid/sql.html

# 15 IOC和AOP

[(55条消息) Spring常用注解，注解 IOC ,AOP，MVC 的理解_ljm_99的博客-CSDN博客_aop和mvc](https://blog.csdn.net/lijiaming_99/article/details/120835843?ops_request_misc=%7B%22request%5Fid%22%3A%22164266724916780366586102%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=164266724916780366586102&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~times_rank-1-120835843.first_rank_v2_pc_rank_v29&utm_term=spring+ioc+aop注解&spm=1018.2226.3001.4187)

