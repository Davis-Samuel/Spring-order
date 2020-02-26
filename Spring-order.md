# 1.IOC简单原型

## *<u>父工程spring-study，模块spring-01-ioc1</u>*

- 引入依赖：

  ```xml
   <dependencies>
          <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-webmvc</artifactId>
              <version>5.2.3.RELEASE</version>
          </dependency>
      </dependencies>
  ```


- 结构：
- ![image-20200216140006815](C:\Users\91566\AppData\Roaming\Typora\typora-user-images\image-20200216140006815.png)

- ServiceImpl代码：

  ```java
  package com.it.service;
  import com.it.Dao.User;
  import java.util.DoubleSummaryStatistics;
  public class ServiceImpl implements Service{
      private User user;
      //动态的set注入，控制反转，主动权都交给客户端。“service不自己种树了，客户端给我钱，我去给你买”
      public void setUser(User user){
          this.user = user;
      }
      
      @Override
      public void getUser() {
          user.getUser();
      }
  }
  ```

- 测试：

  ```java
  package MyTest;
  import com.it.Dao.User;
  import com.it.Dao.UserImpl;
  import com.it.service.Service;
  import com.it.service.ServiceImpl;
  public class MyTest {
      public static void main(String[] args) {
          ServiceImpl service = new ServiceImpl();
          service.setUser(new UserImpl());
          service.getUser();
      }
  }
  ```

# 2.HelloSpring

## *<u>模块spring-02-hellospring</u>*

-  使用xml的方式创建对象

- new一个包，再new一个Hello类：

  ```java
  package com.it.pojo;
  
  public class Hellow {
          private String name;
  
          public String getName() {
              return name;
          }
      //核心是set方法，不能缺少！！
          public void setName(String name) {
              this.name = name;
          }
  
          public void show(){
              System.out.println("Hello,"+ name );
          }
  }
  
  ```

  

- 在resource资源文件中，new一个beans.xml，内容：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  <!--    使用spring创建对象，这些spring就都叫做beans
          java中 Hello hello = new Hello()
          spring中 id 就是变量名，class=new Hello()
          property name就是属性名 value相当于给属性设置具体的值；
  		ref 就是引用spring容器中已经创建好的对象，用于(private User user)
  -->
  <bean id="hello" class="com.it.pojo.Hellow">
      <property name="name" value="Spring"/>
  </bean>
  </beans>
  ```

- 测试：

  ```java
  import com.it.pojo.Hellow;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  public class MyTest {
      public static void main(String[] args) {
  //        获取spring的上下文对象
          ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
          //getBean()对应beans的id
          //后面跟上User.class就不需要强转了。
          User user= context1.getBean("user",User.class);
          //之后就是调用方法
          System.out.println("hello = " + hello.getName().toString());
      }
  }
  ```

- 练习：修改IOC简单模型

# 3.IOC创建对象方式

- 默认无参构造创建

- 如果是有参构造，方法：

  ```xml
  <!--下标赋值。index为有参构造的第一个参数-->
  <bean id = "" class = "">
  <constructor-org index="0" value="赋值"/>
  </bean>
  <!--直接通过参数名赋值。name为有参构造中的参数-->
  <bean id = "" class = "">
  <constructor-org name = "name" value="赋值"/>
  </bean>
  ```

# 4.spring配置

## *<u>模块spring-03-ioc2</u>*

## bean的配置+别名

- beans.xml代码:

  ```xml
  <bean id="user" class="com.it.pojo.User">
      <property name="id" value="111"/>
      <property name="name" value="li"/>
  </bean>
  <!--    1.给上方id=“user”起一个别名叫u-->
      <alias name="user" alias="u"/>
  <!--    2.name起多个个别名。名字分隔符可以为空格逗号分号-->
  	<bean id="user" class="com.it.pojo.User" name="u1","u2">
  ```

## import

- 其引用的内容将会合并在一起
- ![image-20200216172942339](C:\Users\91566\AppData\Roaming\Typora\typora-user-images\image-20200216172942339.png)

# 5.DI依赖创建

## *<u>模块spring-04-di</u>*

## 构造器注入

- 3中已经说过

## Set方式注入(重点)

- 搭建环境(类：student.java，Address.java。resource：applicationContext.xml)

- student：

  ```java
  package com.it.pojo;
  import java.util.*;
  public class Student {
      private String name;
      private Address address;
      private String[] books;
      private List<String> hobbies;
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
      public List<String> getHobbies() {
          return hobbies;
      }
      public void setHobbies(List<String> hobbies) {
          this.hobbies = hobbies;
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
                  ", address=" + address.toString() +
                  ", books=" + Arrays.toString(books) +
                  ", hobbies=" + hobbies +
                  ", card=" + card +
                  ", games=" + games +
                  ", wife='" + wife + '\'' +
                  ", info=" + info +
                  '}';
      }
  }
  ```

- Address：

  ```java
  package com.it.pojo;
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

- applicationContext：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
      <bean id="adress" class="com.it.pojo.Address">
          <property name="address" value="山东"/>
      </bean>
      
      <bean id="student" class="com.it.pojo.Student">
  <!--        第一种普通value注入-->
          <property name="name" value="Samuel"/>
  <!--        第二种bean注入，ref-->
          <property name="address" ref="adress"/>
  <!--        第三种数组注入-->
          <property name="books">
              <array>
                  <value>《红楼梦》</value>
                  <value>《西游记》</value>
                  <value>《水浒传》</value>
              </array>
          </property>
  <!--        第四种list注入-->
          <property name="hobbies">
              <list>
                  <value>唱跳</value>
                  <value>rap</value>
                  <value>篮球</value>
              </list>
          </property>
  <!--        第五种map注入-->
          <property name="card">
              <map>
                  <entry key="身份证" value="6300xxxx"/>
                  <entry key="银行卡" value="784560180xxx"/>
              </map>
          </property>
  <!--        第六种set注入-->
          <property name="games">
              <set>
                  <value>LOL</value>
                  <value>幻想神域</value>
              </set>
          </property>
  <!--        第七种注入null或空.如果设置为空，就直接在name后面跟上value=" ".-->
          <property name="wife">
              <null/>
          </property>
  <!--        第七种properties注入，类似key-value-->
          <property name="info">
              <props>
                  <prop key="driver">202000xx</prop>
                  <prop key="username">Samuel</prop>
                  <prop key="url">http://mail.ju.edu.cn</prop>
                  <prop key="password">1111111xx</prop>
              </props>
          </property>
      </bean>
  </beans>
  ```

- 测试：

  ```java
  import com.it.pojo.Student;
  import com.it.pojo.User;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  public class MyTest {
      public static void main(String[] args) {
          ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
          Student stu = (Student)context.getBean("student");
          System.out.println(stu.toString() );
  
          ApplicationContext context1 = new ClassPathXmlApplicationContext("userbeans.xml");
          
  //        后面跟上User.class就不需要强转了。
          User user= context1.getBean("user",User.class);
          System.out.println(user.toString());
      }
  }
  ```

  

## 拓展(类User,userbeans.xml)

### p命名空间

- new一个User.java类：

  ```java
  package com.it.pojo;
  public class User {
      private String name;
      private int age;
      @Override
      public String toString() {
          return "User{" +
                  "name='" + name + '\'' +
                  ", age=" + age +
                  '}';
      }
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
  }
  ```

- 加一个userbeans.xml :

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:p="http://www.springframework.org/schema/p"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">
  
  <!--    直接通过p：name的方式进行属性的赋值操作。p:spouse-ref引用复杂类型-->
  <bean id="user" class="com.it.pojo.User" p:name="张三" p:age="100"/>
  </beans>
  ```

### c命名空间

- ```xml
  xmlns:c="http://www.springframework.org/schema/c"
  ```

- 利用有参构造器，给构造器中的属性传值，必须有有参构造。形式与p命名空间相似

- userbeans.xml :

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:p="http://www.springframework.org/schema/p"
         xmlns:c="http://www.springframework.org/schema/c"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">
  
  <!--    直接通过p：name的方式进行属性的赋值操作.p:spouse-ref引用复杂类型。-->
  <bean id="user" class="com.it.pojo.User" p:name="张三" p:age="100" />
  
  <!--    给有参构造里的参数传值-->
  <bean id="user2" class="com.it.pojo.User" c:age="200" c:name="李四"/>
  </beans>
  ```


## bean的作用域

- 单例：![image-20200217122305529](C:\Users\91566\AppData\Roaming\Typora\typora-user-images\image-20200217122305529.png)

- 原型：![image-20200217122802058](C:\Users\91566\AppData\Roaming\Typora\typora-user-images\image-20200217122802058.png)

# 6.bean的自动装配(复杂类型)

## *<u>模块spring-05-autowired</u>*

## 环境搭建

- 一个人有两个猫狗宠物

- 一个人有两只狗(People)：

  ```java
  package com.it.pojo;
  public class People {
      private String name;
      private Dog dog;
      private Cat cat;
      public People() {
      }
      public String getName() {
          return name;
      }
      public void setName(String name) {
          this.name = name;
      }
      public Dog getDog() {
          return dog;
      }
      public void setDog(Dog dog) {
          this.dog = dog;
      }
      public Cat getCat() {
          return cat;
      }
      public void setCat(Cat cat) {
          this.cat = cat;
      }
      @Override
      public String toString() {
          return "People{" +
                  "name='" + name + '\'' +
                  ", dog=" + dog +
                  ", cat=" + cat +
                  '}';
      }
  }
  ```

- 一只猫：

  ```java
  package com.it.pojo;
  public class Cat {
      public void shout(){
          System.out.println("喵喵喵~");
      }
  }
  ```

- 一只狗：

  ```java
  package com.it.pojo;
  public class Dog {
      public void shout(){
          System.out.println("汪汪汪~");
      }
  }
  ```

- applicationContext.xml：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:p="http://www.springframework.org/schema/p"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <!--    正常装配-->
  <!--    <bean id="cat" class="com.it.pojo.Cat"/>-->
  <!--    <bean id="dog" class="com.it.pojo.Dog"/>-->
  <!--    <bean id="people" class="com.it.pojo.People">-->
  <!--        <property name="name" value="张三"/>-->
  <!--        <property name="cat " ref="cat"/>-->
  <!--        <property name="dog" ref="dog"/>-->
  <!--    </bean>-->
  
  <!--    使用p命名空间-->
      <bean id="cat" class="com.it.pojo.Cat"/>
      <bean id="dog" class="com.it.pojo.Dog"/>
      <bean id="people" class="com.it.pojo.People" p:name="张三" p:cat-ref="cat" p:dog-ref="dog"/>
      </beans>
  ```

- 测试：

  ```java
  import com.it.pojo.People;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  public class MyTest {
      public static void main(String[] args) {
          ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
          People people = context.getBean("people", People.class);
          people.getDog().shout();
          people.getCat().shout();
          System.out.println(people.getName().toString());
      }
  }
  ```

## byName自动装配

- applicationContext.xml:

  ```xml
  <!--使用ByName自动装配,原理会在容器上下文中寻找，
  找和自己set方法后面的对应的bean id(例：setDog方法，bean id=dog，因为id=dog对应setDog，所以自动装配)-->
      <bean id="people" class="com.it.pojo.People" p:name="张三" autowire="byName"/>
  ```

## byType自动装配

- applicationContext.xml:

  ```xml
  <!--    使用ByName自动装配,原理会在容器上下文中寻找，要有唯一的dog对象和cat对象，不能有两个。不加id也行-->
      <bean id="people" class="com.it.pojo.People" p:name="张三" autowire="byType"/>
  ```

## 使用注解自动装配

- 使用注解需做：使用官方给定的既导入约束又配置了注解支持的applicationContext.xml：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd">
  
      <context:annotation-config/>
  
  </beans>
  ```

### @Autowired的使用(名字一致)

- applicationContext.xml：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd">
  <!--开启自动装配注解-->
  <context:annotation-config/>
      <bean id="people" class="com.it.pojo.People"/>
      <bean id="cat" class="com.it.pojo.Cat"/>
      <bean id="dog" class="com.it.pojo.Dog"/>
  </beans>
  ```

- People.java：

  ```java
  package com.it.pojo;
  
  import org.springframework.beans.factory.annotation.Autowired;
  
  public class People {
      private String name;
      @Autowired
      private Dog dog;
      @Autowired
      private Cat cat;
  
      public People() {
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public Dog getDog() {
          return dog;
      }
   //  @Autowired  也可以放在set方法上使用。也可以省略set方法！！！
      public void setDog(Dog dog) {
          this.dog = dog;
      }
  
      public Cat getCat() {
          return cat;
      }
   //  @Autowired  也可以放在set方法上使用。也可以省略set方法！！！
      public void setCat(Cat cat) {
          this.cat = cat;
      }
  
      @Override
      public String toString() {
          return "People{" +
                  "name='" + name + '\'' +
                  ", dog=" + dog +
                  ", cat=" + cat +
                  '}';
      }
  }
  ```

### @Nullable和@Autowired(required=false)的使用

- ```java
  //required = false表明该字段可以为空
  @Autowired(required = false)
      private Dog dog;
  //表明该字段可以为空 
  public People(@Nullable String name) {
          this.name = name;
      }
  ```

### @Autowired和@Qualifier(value = " ")组合使用

- 如果存在多个猫对象但是还想注解装配，使用指定猫中的指定一个对象：

  ```java
  @Autowired
  @Qualifier(value = "cat1") //指定猫中的指定一个对象
  private Cat cat;
  ```

### @Resource的使用

- @Resource直接装配。@Resource(name="cat1"),指定某一个对象

# 7.使用注解开发

## *<u>模块spring-06-anno</u>*

- 使用注解开发要有包：![image-20200217145514280](C:\Users\91566\AppData\Roaming\Typora\typora-user-images\image-20200217145514280.png)

- 要有官方给定的xml约束文件：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd">
      
  <!--    指定要扫描的包，这个包下的注解就会生效-->
  <context:component-scan base-package="com.it"/>
      <context:annotation-config/>
  
  </beans> 
  ```

- 自动装配注解：上方6.👆

- 这四个注解作用一样：等价于 <bean id="user" class="com.it.pojo.User"/>

  ![image-20200217153300836](C:\Users\91566\AppData\Roaming\Typora\typora-user-images\image-20200217153300836.png)都是注册到spring中，装配bean。

  ```java
  @Component //等价于 <bean id="user" class="com.it.pojo.User"/>
  public class User {
      @Value("jjj") //属性赋值
      public String name;
  }
  ```

- ```java
  @Value("jjj") //属性赋值
  public String name;
  ```

- 作用域注解：

  ```java
  @Scope("singleton") //标注为单例模式,原型模式"proptotype"
  @Component //等价于 <bean id="user" class="com.it.pojo.User"/>创建对象
  public class User {
  @Value("jjj") //属性赋值
  public String name;
  }
  //创建完对象才可以装配！！！
  ```

- 基本格式：![image-20200217161903826](C:\Users\91566\AppData\Roaming\Typora\typora-user-images\image-20200217161903826.png)

# 8.使用Java方式配置spring

## *<u>模块spring-07-appconfig</u>*

- 完全脱离xml文件，去交给java来做
- 结构：![image-20200217182804853](C:\Users\91566\AppData\Roaming\Typora\typora-user-images\image-20200217182804853.png)

- MyConfig.Java代码:

  ```java
  package com.it.config;
  
  import com.it.pojo.User;
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.ComponentScan;
  import org.springframework.context.annotation.Configuration;
  import org.springframework.context.annotation.Import;
  
  //创建出对象的过程，转移到了config包的Myconfig.java中,叫配置类，等价于配置文件
  
  @Configuration //相当于代替了xml配置文件,也会被spring容器保存既创建了对象,里面的东西都可以通过注解实现
  @ComponentScan("com.it") //扫描包，包下的注解都有效
  @Import(MyConfig2.class) //将两个配置类合并在了一起
  public class MyConfig {
  
  //    @bean就相当于xml中bean标签
  //    方法的名字就是id
  //    返回值类型就是class
      @Bean
      public User user (){
          return new User();
      }
  }
  ```

- MyConfig.Java代码:空

- User.java代码：

  ```java
  package com.it.pojo;
  import org.springframework.beans.factory.annotation.Value;
  import org.springframework.stereotype.Component;
  
  @Component
  public class User {
      private String name;
      @Override
      public String toString() {
          return "User{" +
                  "name='" + name + '\'' +
                  '}';
      }
      public String getName() {
          return name;
      }
      @Value("ssa")
      public void setName(String name) {
          this.name = name;
      }
  }
  ```

- 测试代码：

  ```java
  import com.it.pojo.User;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.annotation.AnnotationConfigApplicationContext;
  public class MyTest {
      public static void main(String[] args) {
  
          ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
          //getBean(填写id也就是MyConfig.java中的bean的方法名)
          User getuser = context.getBean("user", User.class);
          System.out.println(getuser.getName());
  
      }
  }
  ```

# 9.上阶段总结

- 目前用目录中的“7.使用注解开发” ，如果有字段是复杂类型，就先给复杂类一个@Component ，然后在别的类中的字段上加上@Autowired自动装配，也就是给复杂类型赋值，赋的值就是@Component出来的对象。如果是set等复杂类型就要写配置文件

# 10.代理模式

## *<u>模块spring-08-proxy</u>*

## 静态代理

- 真实主题接口：

  ```java
  package com.it.demo01;
  //租房
  public interface Rent {
      public void rent();
  }
  ```

- 真实角色：

  ```java
  package com.it.demo01;
  //房东
  public class House implements Rent{
      public void rent() {
          System.out.println("房东出租房子");
      }
  }
  ```

- 代理角色：

  ```java
  package com.it.demo01;
  public class Proxy implements Rent{
      private House house;
      public Proxy() {
      }
      public Proxy(House house) {
          this.house = house;
      }
      //代理可以做的事情
      public void seehouse(){
          System.out.println("看很多房子");
      }
      //代理可以做的事情
      public void hetong(){
          System.out.println("签合同");
      }
      public void rent() {
          house.rent();
          seehouse();
          hetong();
      }
  }
  ```

- 客户端访问代理做事：

  ```java
  package com.it.demo01;
  public class Client {
      public static void main(String[] args) {
          House house = new House();
          Proxy proxy = new Proxy(house);
          proxy.rent();
      }
  }
  ```

## 动态代理

- 基于接口--JDK动态代理

- demo2模板，需要反射基础，先new接口和接口实现类，利用模板，改写客户端代码

- 接口UserService

  ```java
  package com.it.demo02;
  public interface UserService {
      public void add();
      public void delete();
      public void update();
      public void query();
  }
  ```

- 实现类UserServiceImpl：

  ```java
  package com.it.demo02;
  //真实对象
  public class UserSeiviceImpl implements UserService {
      public void add() {
          System.out.println("增加了一个用户！");
      }
      public void delete() {
          System.out.println("删除了一个用户！");
      }
      public void update() {
          System.out.println("更新了一个用户！");
      }
      public void query() {
          System.out.println("查询了一个用户！");
      }
  }
  ```

- 动态代理模板ProxyInvocationHandler：

  ```java
  package com.it.demo02;
  import java.lang.reflect.InvocationHandler;
  import java.lang.reflect.Method;
  import java.lang.reflect.Proxy;
  
  public class ProxyInvocationHandler implements InvocationHandler {
      //被代理的接口
      private Object target;
      public void setTarget(Object target) {
          this.target = target;
      }
      //生成得到代理类
      public Object getProxy() {
          return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(), this);
      }
  
      //处理代理实例，返回代理结果
      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
          //动态代理的本质，就是使用反射机制实现
         //添加的日志功能
          log(method.getName());
          Object result = method.invoke(target, args);
          return result;
      }
  
      //    增加功能，添加一个日志功能
      public void log(String msg){
          System.out.println("执行了"+msg+"方法");
      }
  }
  ```

- 客户端：

  ```java
  package com.it.demo02;
  public class Client {
      public static void main(String[] args) {
          //真实角色
          UserSeiviceImpl userService = new UserSeiviceImpl();
          //代理角色，不存在
          ProxyInvocationHandler pih = new ProxyInvocationHandler();
  
          pih.setTarget(userService);//设置要代理的对象
          //动态生成代理类
          UserService proxy = (UserService) pih.getProxy();
  
          proxy.add();
          proxy.delete();
      }
  }
  ```

# 11.spring实现AOP

## *<u>模块spring-09-aop</u>*

- 我的理解就是从已经包装好的代码里面，再进行新增方法或者功能且不改变原来代码，用aop从外部写一个代码，然后作为切面或将其引入切入点插进原来要添加方法的那个类中。

- 导入依赖包：

  ```xml
  <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
  <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.9.4</version>
  </dependency>
  ```


## 方式一：使用spring的API接口

- 结构:![image-20200218121015817](C:\Users\91566\AppData\Roaming\Typora\typora-user-images\image-20200218121015817.png)

- AfterLog.java:

  ```java
  package com.it.log;
  import org.springframework.aop.AfterReturningAdvice;
  import java.lang.reflect.Method;
  
  //添加后置日志
  //实现了AfterReturningAdvice方法，意味执行完UserServiceImpl方法后在执行
  public class AfterLog implements AfterReturningAdvice {
      public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
          System.out.println("执行了" +method.getName()+"方法，返回结果为：" + returnValue);
      }
  }
  ```

- BeforeLog.java：

  ```java
  package com.it.log;
  
  import org.springframework.aop.MethodBeforeAdvice;
  
  import java.lang.reflect.Method;
  
  //增加前置日志
  
  //method：要执行的目标对象的方法
  //args：参数
  //target：目标对象
  //实现了MethodBeforeAdvice方法，意味在执行UserServiceImpl方法之前执行
  public class BeforeLog implements MethodBeforeAdvice {
      public void before(Method method, Object[] args, Object target) throws Throwable {
          System.out.println(target.getClass().getName() + "的" + method.getName() + "被执行了");
      }
  }
  ```

- 接口UserService.java:

  ```java
  package com.it.service;
  public interface UserService {
      public void add() ;
      public void delete();
      public void update();
      public void query();
  }
  ```

- 接口实现UserServiceImpl.java：

  ```java
  package com.it.service;
  
  //真实对象！！！
  public class UserServiceImpl implements UserService{
      public void add() {
          System.out.println("增加了一个用户！");
      }
      public void delete() {
          System.out.println("删除了一个用户！");
      }
      public void update() {
          System.out.println("更新了一个用户！");
      }
      public void query() {
          System.out.println("查询了一个用户！");
      }
  }
  ```

- applicationContext.xml:

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:aop="http://www.springframework.org/schema/aop"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
  
  <!--    注册到spring-->
      <bean id="userservice" class="com.it.service.UserServiceImpl"/>
      <bean id="afterlog" class="com.it.log.AfterLog"/>
      <bean id="beforelog" class="com.it.log.BeforeLog"/>
  
  
  <!--方法一，使用原生spring API接口。定义的AfterLog和BeforeLog类都需要实现接口，才能指定执行的前后循序。
  规律：aop:config→→→→(要增加业务的类中的各种方法，一般是所有方法也就是*)切入点→→→→ 环绕增强(将以上两个实现接口的类添加到UserServiceImpl类中)   -->
  
  <!-- 配置AOP：需要导入AOP的约束-->
  <!--    <aop:config>-->
  <!--配置切入点pointcut：需要在那地方执行.expression:表达式,execution()要执行的位置
      execution(* com.it.service.UserSeiviceImpl.*(..))
      👆：格式：(*表示返回的所有类型)* 要执行的类全路径.方法(*就是全部方法)(..)(括号点点就是方法中的参数)-->
  <aop:pointcut id="pointcut" expression="execution(* com.it.service.UserServiceImpl.*(..))"/>
  
  <!--配置执行环绕增强.将afterLog,beforelog加入到上面那个切面，也就是两个log类加到UserService类里面
  advice-ref="afterlog":将要插入的类的bean 。pointcut-ref="pointcut"：切入点的id-->
  <aop:advisor advice-ref="afterlog" pointcut-ref="pointcut"/>
  <aop:advisor advice-ref="beforelog" pointcut-ref="pointcut"/>
  </aop:config>
  </beans>
  ```

- 测试：

  ```java
  import com.it.service.UserService;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  
  public class MyTese {
      public static void main(String[] args) {
       ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
          //动态代理，代理的是接口，不能写实现类
          UserService service = context.getBean("userservice", UserService.class);
          service.add();
      }
  }
  ```

  

## 方法二：自定义类，不实现接口(切面)

- 新增结构：![image-20200218122806260](C:\Users\91566\AppData\Roaming\Typora\typora-user-images\image-20200218122806260.png)

- Diy.java:

  ```java
  package com.it.diy;
  public class Diy {
      public void seeBefore(){
          System.out.println("===============方法执行前");
      }
      public void seeAfter(){
          System.out.println("===============方法执行后");
      }
  }
  ```

- applicationContext.xml:

  ```xml
  <!--方法二。自定义类，不实现任何接口。将此类作为切面。类似于切入点就是植入很多方法，而且面就是植入整个类，方法属性组成类。规律：aop:config→→→→切面(aop:aspect)→→→→切入点→→→→通知(类似环绕增强)-->
  <bean id="diy" class="com.it.diy.Diy"/>
  
  <aop:config>
  <!--自定义切面，ref引用自定义的类-->
  <aop:aspect ref="diy">
  <!--切入点-->
  <aop:pointcut id="point" expression="execution(* com.it.service.UserServiceImpl.*(..))"/>
  <!--通知method:要加入的方法的名字。pointcut-ref="pointcut"：切入点的id-->
  <aop:before method="seeBefore" pointcut-ref="point"/>
  <aop:after method="seeAfter" pointcut-ref="point"/>
  </aop:aspect>
  </aop:config>
  ```

## 方法三：使用注解实现AOP

- 新增结构：![image-20200218132740204](C:\Users\91566\AppData\Roaming\Typora\typora-user-images\image-20200218132740204.png)

- AnnocationPointCut.java：

  ```java
  package com.it.annotationpointcut;
  
  //使用注解实现AOP
  
  import org.aspectj.lang.ProceedingJoinPoint;
  import org.aspectj.lang.annotation.After;
  import org.aspectj.lang.annotation.Around;
  import org.aspectj.lang.annotation.Aspect;
  import org.aspectj.lang.annotation.Before;
  
  @Aspect  //使用注解标记为这是一个切面
  public class AnnotationPointCut {
  
      @Before("execution(* com.it.service.UserServiceImpl.*(..))") //加入切入点
      public void seeBefore(){
          System.out.println("===============方法执行前");
      }
      @After("execution(* com.it.service.UserServiceImpl.*(..))")
      public void seeAfter(){
          System.out.println("===============方法执行后");
      }
      //在环绕增强中，我们可以给定一个参数，代表我们要获取处理切入的点
      @Around("execution(* com.it.service.UserServiceImpl.*(..))")
      public void seearound(ProceedingJoinPoint pj) throws Throwable {
          System.out.println("执行前");
          Object proceed = pj.proceed();
          System.out.println("1111");
      }
  }
  ```

- applicationContext.xml:

  ```xml
  <!--方式三：使用注解实现AOP-->
      <bean id="annotationpointcut" class="com.it.annotationpointcut.AnnotationPointCut"/>
  <!--    开启注解支持！！-->
      <aop:aspectj-autoproxy/>
  ```

# 12.spring整合mybatis

## *<u>模块spring-10-mybatis-spring</u>*

- 导入依赖包：

  ```xml
  <dependencies>
  
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>8.0.16</version>
      </dependency>
  
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.5.3</version>
      </dependency>
  
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
          <scope>test</scope>
      </dependency>
      
      
      <dependency>
              <groupId>org.projectlombok</groupId>
              <artifactId>lombok</artifactId>
              <version>1.18.8</version>
          </dependency>
      
      
      <dependency> <!--spring各种包-->
              <groupId>org.springframework</groupId>
              <artifactId>spring-webmvc</artifactId>
              <version>5.2.3.RELEASE</version>
          </dependency>
      
      
      <dependency> <!--AOP织入包-->
              <groupId>org.aspectj</groupId>
              <artifactId>aspectjweaver</artifactId>
              <version>1.9.4</version>
          </dependency>
      
      <dependency> <!--spring和mybatis整合包-->
              <groupId>org.mybatis</groupId>
              <artifactId>mybatis-spring</artifactId>
              <version>2.0.3</version>
          </dependency>
      
      <dependency> <!--spring和JDBC整合包-->
          <groupId>org.springframework</groupId>
          <artifactId>spring-jdbc</artifactId>
          <version>5.2.2.RELEASE</version>
      </dependency>
  
  </dependencies>
  ```

- 写mybatis模板结构。直接参考之前程序。

## 方法一：mybatis-spring

- **结构**：![image-20200218175041027](C:\Users\91566\AppData\Roaming\Typora\typora-user-images\image-20200218175041027.png)

- **按创建编写顺序来。**

- **spring-dao.xml：**

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:p="http://www.springframework.org/schema/p"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  
  <!--    到此为止，这个文件已经固定写死了。只需要改，"绑定mybatis配置文件"，部分的路径即可-->
  
  
  
  <!--1.DataSource:使用spring的数据源，代替mybatis的配置文件 直接使用spring的JDBC-->
      <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
          <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
          <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=Asia/Shanghai"/>
          <property name="username" value="root"/>
          <property name="password" value="3105311"/>
      </bean>
  
  
  <!--2.获取SqlSessionFactory-->
      <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
          <property name="dataSource" ref="dataSource" />
  <!--        绑定mybatis配置文件，mybatis配置文件里面可以做的事情，在这都可以完成-->
          <property name="configLocation" value="classpath:mybatis-config.xml"/>
  <!--        相当于<mapper class="com.it.dao.UserMapper"/>-->
          <property name="mapperLocations" value="classpath:com/it/dao/*.xml"/>
      </bean>
  
      
  <!--3.获取SqlSessionTemplate，也就是sqlSession-->
      <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
  <!--        只能使用构造器注入SqlSessionFactory，因为它没有set方法-->
          <constructor-arg index="0" ref="sqlSessionFactory"/>
      </bean>
  
  
  </beans>
  ```

- **mybatis-config.xml：**

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
          <!DOCTYPE configuration
                  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
                  "http://mybatis.org/dtd/mybatis-3-config.dtd">
  
  <configuration>
  
  
  <!--    mybatis文档只留，别名和一些setting，例如日志-->
  
  
  <settings>
      <setting name="logImpl" value="STDOUT_LOGGING"/>
  </settings>
  
  
  <typeAliases>
      <typeAlias type="com.it.pojo.User" alias="user"/>
  </typeAliases>
  
  
  </configuration>
  ```

- **applicationContext.xml：**

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:p="http://www.springframework.org/schema/p"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  <!--    它专注于注册。-->
  
  <!--导入spring-dao.xml-->
      <import resource="spring-dao.xml"/>
  
  
      <!--    注册UserMapperImpl-->
      <bean id="usermapperimpl" class="com.it.dao.UserMapperImpl">
          <property name="sqlSession" ref="sqlSession"/>
      </bean>
  
  </beans>
  ```

- **User.java：**

  ```java
  package com.it.pojo;
  import lombok.Data;
  @Data
  public class User {
      private int id;
      private String name;
      private String pwd;
  }
  ```

- **接口UserMapper.java：**

  ```java
  package com.it.dao;
  import com.it.pojo.User;
  import java.util.List;
  public interface UserMapper {
      List<User> select();
  }
  ```

- **UserMapper.xml：**

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="com.it.dao.UserMapper">
  <select id="select" resultType="user">
      select * from mybatis.user
  
  </mapper>
  ```

- **UserMapperImpl.java：**

  ```java
  package com.it.dao;
  import com.it.pojo.User;
  import org.mybatis.spring.SqlSessionTemplate;
  import java.util.List;
  
  public class UserMapperImpl implements UserMapper {
  //    原来的操作都是使用sqlSession来执行，现在使用sqlSessionTemplate来实现
      private SqlSessionTemplate sqlSession;
  //添加set方法，将其注册到spring,注册之后就存在对象了
      public void setSqlSession(SqlSessionTemplate sqlSession) {
          this.sqlSession = sqlSession;
      }
  
      public List<User> select() {
          UserMapper mapper = sqlSession.getMapper(UserMapper.class);
          List<User> select = mapper.select();
          return select;
      }
  }
  ```

## 方法二：mybatis-spring

- **修改UserMapperImpl.java :**

  ```java
  package com.it.dao;
  import com.it.pojo.User;
  import org.mybatis.spring.SqlSessionTemplate;
  import java.util.List;
  
  public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper {
  	SqlSession sqlSession = getSqlSession();
      return sqlSession.getMapper(UserMapper.class).select();
      }
  }
  ```

- **修改applicationContext.xml：**

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:p="http://www.springframework.org/schema/p"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  <!--    它专注于注册。-->
  
  <!--导入spring-dao.xml-->
      <import resource="spring-dao.xml"/>
  
  
      <!--    注册UserMapperImpl-->
      <bean id="usermapperimpl" class="com.it.dao.UserMapperImpl">
          <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
      </bean>
  
  </beans>
  ```

- **修改spring-dao.xml：**

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:p="http://www.springframework.org/schema/p"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  
  <!--    到此为止，这个文件已经固定写死了。只需要改，绑定mybatis配置文件，部分的路径即可-->
  
  
  
  <!--DataSource:使用spring的数据源，代替mybatis的配置文件 直接使用spring的JDBC-->
      <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
          <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
          <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=Asia/Shanghai"/>
          <property name="username" value="root"/>
          <property name="password" value="3105311"/>
      </bean>
  
  
  <!--获取SqlSessionFactory-->
      <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
          <property name="dataSource" ref="dataSource" />
  <!--        绑定mybatis配置文件，mybatis配置文件里面可以做的事情，在这都可以完成-->
          <property name="configLocation" value="classpath:mybatis-config.xml"/>
  <!--        相当于<mapper class="com.it.dao.UserMapper"/>-->
          <property name="mapperLocations" value="classpath:com/it/dao/*.xml"/>
      </bean>
  
  
  </beans>
  ```

- **其余不变**

# 13.声明式事务

- 在spring-dao.xml中：

  ```xml
  
  <!--    设置声明事务-->
      <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <constructor-arg ref="dataSource"/>
      </bean>
  <!--    结合AOP实现事务的织入-->
  <!--    配置事务通知-->
      <tx:advice id="txAdvice" transaction-manager="transactionManager">
  <!--        要给那些方法配置事务-->
          <tx:attributes>
  <!--            *是所有方法-->
  <!--            配置事务的特性，propagation，read-only......-->
              <tx:method name="add" propagation="REQUIRED"/>
              <tx:method name="delete" propagation="REQUIRED"/>
              <tx:method name="update" propagation="REQUIRED"/>
              <tx:method name="insert" propagation="REQUIRED"/>
              <tx:method name="query" read-only="true"/>
              <tx:method name="*" propagation="REQUIRED"/>
          </tx:attributes>
      </tx:advice>
  
  <!--    配置事务切入-->
      <aop:config>
          <aop:pointcut id="txPointCut" expression="execution(* com.it.dao.*.*(..))"/>
          <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
      </aop:config>
  ```

  
