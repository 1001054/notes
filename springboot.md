# SpringBoot

springBoot是不需配置文件的spring+springMVC，开发效率高。



## 一、javaConfig

1. 是xml配置文件的替代，使用纯java的方式配置spring容器

2. 在这个java类中创建对象，把对象注入到spring容器中

   

### 相关的注解：

1. @Configuration:  放在配置类上面，表示这个类是作为配置文件使用的。

2. @Bean: 放在配置类的方法上，返回值是对象，该对象会被注入到容器中。（可以指定该对象的名称，若不指定，默认为方法名）

3. @ImportResource: 放在配置类上，将xml配置文件导入配置类。（以{“”，“”}来导入多个配置文件）

4. @PropertyResource:  放在配置类上，读取properties属性配置文件，实现外部化配置，程序代码外提供数据。（使用时，用${tiger.name}）

5. @ComponentScan: 放在配置类上，组件扫描器，扫描通过注解注入到容器中的类。

6. @ConfigurationProperties(prefix=""): 放在任何类上，表示该类可以使用主配置文件中的相关配置。并且自动byName自动给相关属性自动赋值。

## 二、SpringBoot

### 创建springboot项目：

* Spring Initializr: Custom: https://start.springboot.io (默认或使用此国内地址)

  

### springBoot配置文件

1. 文件名称：application

2. 扩展名：properties(k=v);      yml(k: v)

3. 内容：可以修改服务器的相关配置（server.port;      server.servlet.context-path）

   

### 多环境配置

1. 有开发环境，测试环境，上线环境

2. 每个环境有不同的配置信息，例如端口，上下文件，数据库url，用户名，密码等等

3. 使用多环境配置文件，可以方便的切换不同的配置

4. 使用方式：创建多个配置文件

5. 名称规则：application-环境名称.properties(yml)

6. 激活方式：在主配置文件中写入：spring.profiles.active=环境名称



 ### jsp相关配置

1. 需要加入一个处理jsp的依赖，负责编译jsp文件

   ```xml
   <dependency>
       <groupId>org.apache.tomcat.embed</groupId>
       <artifactId>tomcat-embed-jasper</artifactId>
   </dependency>
   ```

2. 如果需要使用servlet，jsp，jstl的功能，加入相关依赖

   （javax.servlet-jstl; javax.servlet-api; javax.servlet.jsp-api）

   ```xml
   <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>javax.servlet.jsp-api</artifactId>
         <version>2.3.1</version>
   </dependency>
   ```

   

3. 创建一个存放jsp的目录，一般叫做webapp

4. 点击项目，点击webapp，点击+，选择刚建好的文件夹

5. 需要在pom.xml指定jsp文件编译后的存放目录

   ```xml
   <bulid>
       <resources>
           <directory>src/main/webapp</directory>
           <targetPath>META-INF/resources</targetPath>
           <includes>
               <include>**/*.*</include>
           </includes>
       </resources>
   </bulid>
   ```

6. 创建Controller，访问jsp

7. 在application.properties文件中配置视图解析器

   ```properties
   spring.mvc.view.prefix=/
   spring.mvc.view.suffix=.jsp
   ```

   

### 使用容器

* 返回值获得容器：

```java
SpringApplication.run(Application.class, args);
```

* 返回值ConfigurableApplicationContext 接口是ApplicationContext的子接口。



### 两个Runner接口

1. CommandLineRunner和ApplicationRunner接口

2. 都有run方法，执行时间是在容器对象创建好之后，自动执行run方法。

   

## 三、web组件

### 拦截器：

1. 是springMVC中的一种对象，能拦截对Controller的请求。

2. 框架中有系统的拦截器，也可以自定义拦截器，实现对请求预先处理。

3. 步骤：

   创建新类实现SpringMVC框架的HandlerInterceptor接口，重写方法。

   ```java
   public class LoginInterceptor implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           return false;
       }
   }
   ```

   在SpringMVC的配置文件中，声明拦截器。配置类可以继承WebMvcConfigurer。

   ```java
   @Configuration
   public class MyAppConfig implements WebMvcConfigurer {
       @Override
       public void addInterceptors(InterceptorRegistry registry) {
           HandlerInterceptor interceptor = new LoginInterceptor();
           String[] path = {"/user/**"};
           String[] epath = {"/user/login"};
           registry.addInterceptor(interceptor).addPathPatterns(path).excludePathPatterns(epath);
       }
   }
   ```

   

### Servlet

1. 使用步骤：

   1. 创建Servlet类，创建类继承HttpServlet，重写doGet()和doPost()方法。

   2. 注册Servlet，让框架能找到Servlet

      ```java
      @Configuration
      public class ServletConfig {
          @Bean
          public ServletRegistrationBean servletRegistrationBean(){
              MyServlet servlet = new MyServlet();
              ServletRegistrationBean bean = new ServletRegistrationBean(servlet,"/myservlet");
              return bean;
          }
      }
      ```



### 过滤器Filter

是Servlet规范中的过滤器，可以处理请求，对请求参数，属性进行调整，常常在过滤器中处理字符编码。

（1）框架中使用自定义过滤器：

1. 创建自定义过滤器类

   ```java
   public class MyFilter implements Filter {
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           System.out.println("myFilter is running!");
           filterChain.doFilter(servletRequest,servletResponse);
       }
   }
   ```

   

2. 注册Filter过滤器对象

   ```java
   @Configuration
   public class FilterConfig {
       @Bean
       public FilterRegistrationBean register(){
           FilterRegistrationBean bean = new FilterRegistrationBean();
           bean.setFilter(new MyFilter());
           bean.addUrlPatterns("/user/*");
           return bean;
       }
   }
   ```


（2）使用系统提供的字符集过滤器类

注册系统过滤器类

```java
@Configuration
public class FilterConfig {
    @Bean
    public FilterRegistrationBean register(){
        FilterRegistrationBean bean = new FilterRegistrationBean();
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        filter.setEncoding("utf-8");
        filter.setForceEncoding(true);
        bean.setFilter(filter);
        bean.addUrlPatterns("/*");
        return bean;
    }
}
```

 同时要在主配置文件中修改相关配置

```properties
#因为SpringBoot中默认已经配置了CharacterEncodingFilter
#默认编码是ISO-8859-1
#此配置是取消该默认配置
server.servlet.encoding.enabled=false
```



或者采用默认编码配置，并对其经行修改，无需创建过滤器类。

```properties
server.servlet.encoding.enabled=true
server.servlet.encoding.charset=utf-8
server.servlet.encoding.force=true
```



## 四、ORM操作MySQL

使用MyBatis框架操作数据，在SpringBoot框架集成MyBatis

### 使用步骤

1. mybatis起步依赖，完成mybatis对象自动配置，对象放在容器中

2. pom.xml 指定把 src/main/java 目录中的 xml 文件包含到 classpath中

   ```xml
   <build>
           <resources>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.xml</include>
                   </includes>
               </resource>
           </resources>
   </build>
   ```

   

3. 创建实体类Student

4. 创建 Dao 接口 StudentDao，创建一个查询学生的方法

5. 创建 Dao 接口对应的 Mapper 文件，xml 文件，写 sql 语句

6. 创建 Service 层对象，创建 StudentService 接口和他的实现类。去dao 对象的方法，完成数据库的操作

7. 创建 Controller 对象，访问 Service

8. 写 application.properties 文件

   ```properties
   spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
   spring.datasource.url=jdbc:mysql://localhost:3306/student?useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
   spring.datasource.username=root
   spring.datasource.password=333
   ```

   

9. 配置数据库的连接信息

   

### mapper改进

1. mapper过多时，可以在启动类上使用MapperScan注解

```java
@SpringBootApplication
@MapperScan(basePackages = {"com.xj.dao","com.xj.mapper"})
public class Practise2Application {

    public static void main(String[] args) {
        SpringApplication.run(Practise2Application.class, args);
    }
    
}
```

2. 可以将mapper的配置文件放入resources中，使得Java文件与配置文件分离

   a. 在resources目录中创建子目录mapper

   b. 把mapper配置文件放到mapper目录中

   c. 在application.properties文件中，指定mapper配置文件的位置

   ```properties
   #mapper配置文件位置
   mybatis.mapper-locations=classpath:mapper/*.xml
   #指定myBatis的日志
   mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
   ```

   d. 在pom.xml中指定把resources目录中的文件，编译到目标目录中

   ```xml
   <build>
           <resources>
               <resource>
                   <directory>src/main/resources</directory>
                   <includes>
                       <include>**/*.*</include>
                   </includes>
               </resource>
           </resources>
   </build>
   ```

   

## 五、事务

### Spring框架中的事务

1. 管理事务的对象：事务管理器（接口，接口有很多实现类）

   例如：使用jdbc或mybatis访问数据库，使用的事务管理器：DataSourceTransactionManager

2. 声明式事务：在xml配置文件或者使用注解说明事务控制的内容

   控制事务：隔离级别，传播行为，超时时间

3. 事务处理方式：

   a. spring框架中的@Transactional

   b. aspectj框架可以在xml配置文件中，声明事务控制的内容

SpringBoot中使用事务：以上两种方法都可以使用

1. 在业务方法的上面加入@Transactional，加入注解后，方法就有事务功能了

2. 明确的在主启动类上面，加入@EnableTransactionManager

   

## 六、接口架构风格(RESTful)

API（Application Programming Interface）是一些预先定义的接口（如函数、HTTP接口），或指软件系统不同组成部分衔接的约定。用来提供应用程序与开发人员基于某软件或硬件得以访问的一组例程，而又无需访问源码，或理解内部工作机制的细节。

### REST

1. REST：（Representational State Transfer，表现层状态转移）是一种接口的架构风格和设计理念，不是标准。

2. 优点：更简洁，更有层次。

3. REST中的要素：用REST表示资源和对资源的操作，在互联网中，表示一个资源或者一个操作。资源使用url表示的，在互联网，使用的图片，视频，文本，网页等等都是资源。资源是用名词表示的。

4. 使用http中的动作（请求方式），表示对资源的操作（CRUD）

   GET：查询资源

   POST：创建资源

   PUT：更新资源

   DELETE：删除资源

5. 注解：

   @PathVariable：从url中获取数据

   @GetMapping: 支持的get请求方式，等同于@RequestMapping(method=RequestMethod.GET)

   @PostMapping: 支持post请求方式

   @PutMapping：支持put请求方式

   @DeleteMapping：支持delete请求方式

   @RestController：复合注解是@Controller和@ResponseBody组合

   ```java
   //例如：
   @RestController
   public class MyController{
       @GetMapping("/student/{stuId}")
       public String queryStudent(@PathVariable("stuId") Integer studentId){
           return studentId;
       }
   }
   ```

   （当路径变量名称和形参名相同时，@PathVariable可以不加括号）

### 页面中或者ajax中，支持put，delete请求

在SpringMVC中，有一个过滤器，支持post请求转为put，delete

实现步骤：

1. application.properties(yml) 开启使用HiddenHttpMethodFilter过滤器

   ```properties
   #启用支持put，delete
   spring.mvc.hiddenmethod.filter.enabled=true
   ```

2. 在请求页面中，包含_method参数，他的值是put，delete，发起这个请求使用的post方式

   ```html
   <form action="student/test" method="post">
       <input type="hidden" name="_method" value="put">
       <input type="submit" value="测试请求方式">
   </form>
   ```

## 七、Redis

是一个NoSQL数据库，常用作缓存（cache）

数据类型：string，hash，set，zset，list

Redis是一个中间件，是一个独立的服务器。

java中著名的客户端：Jedis，lettuce，Redisson

Spring，SpringBoot中有一个RedisTemplate（StringRedisTemplate）处理和redis交互

RedisTemplate使用的lettuce客户端库

```properties
#指定redis
spring.redis.host=localhost
spring.redis.port=6379
```

### StringRedisTemplate

把k，v都是作为String处理，使用的是String的序列化，可读性较好

### RedisTemplate

把k，v经过了序列化存到redis，kv是序列化的内容，不能直接识别

### 序列化

序列化：把对象转化为可传输的子接序列过程叫做序列化

反序列化：把字节序列还原为对象的过程称为反序列化

目的：为了对象可以跨平台存储，进行网络传输

设置序列化机制：

```java
@RestController
public class MyController {

    @Resource
    private RedisTemplate redisTemplate;

    @GetMapping("/set/{name}/{age}")
    public String set(@PathVariable String name, @PathVariable String age){
        reidsTemplate.setKeySerializer(new StringReidsSerializer());
        reidsTemplate.setValueSerializer(new StringReidsSerializer());
        
        //若把值作为json序列化
        //redisTemplate.setValueSerializer( new Jackson2JsonRedisSerizlizer(Student.Class) );
        
        redisTemplate.opsForValue().set(name,age);
        return "succeed!";
    }

}
```

## 八、集成Dubbo

### 创建接口module

1. 按照Dubbo官方开发建议，创建一个接口项目，该项目只定义接口和model类。
2. 项目名称：interface-api
3. 此项目就是一个普通的maven项目

### 服务提供者module

1. 实现api项目中的接口

   ```java
   //使用dubbo中的注解暴露服务
   @DubboService(interfaceClass = StudentService.class, version = "1.0", timeout = 5000)
   public class StudentServiceImpl implements StudentService {
       @Override
       public Student queryStudent(Integer id) {
           Student student = new Student();
           student.setId(001);
           student.setName("Jack");
           student.setAge(18);
           return student;
       }
   }
   ```

2. 项目名称：service-provider

3. 使用Spring Boot Initializer 创建项目，不用选择依赖

4. maven中加入dubbo依赖 (以及排除依赖)

   ```xml
   <!--加入公共项目的gav-->
   <dependency>
       <groupId>com.xj</groupId>
       <artifactId>interface-api</artifactId>
       <version>1.0-SNAPSHOT</version>
   </dependency>
   
   <!--dubbo依赖-->
   <dependency>
       <groupId>org.apache.dubbo</groupId>
       <artifactId>dubbo-spring-boot-starter</artifactId>
       <version>2.7.8</version>
   </dependency>
   
   <!--zookeeper依赖-->
   <dependency>
       <groupId>org.apache.dubbo</groupId>
       <artifactId>dubbo-dependencies-zookeeper</artifactId>
       <version>2.7.8</version>
       <type>pom</type>
       <exclusions>
           <exclusion>
               <artifactId>slf4j-log4j12</artifactId>
               <groupId>org.slf4j</groupId>
           </exclusion>
       </exclusions>
   </dependency>
   ```

5. 配置application.properties

   ```properties
   #配置服务名称 dubbo:application name="名称"
   spring.application.name=studentservice-provider
   
   #配置扫描的包，扫描@DubboService
   dubbo.scan.base-packages=com.xj.service
   
   #配置dubbo协议
   #dubbo.protocol.name=dubbo
   #dubbo.protocol.port=20881
   
   #注册中心
   dubbo.registry.address=zookeeper://localhost:2181
   ```

6. 在启动类上加@EnableDubbo

### 创建消费者module

1. 项目名称：consumer

2. 使用Spring Boot Initializer 创建项目，加入web起步依赖

3. maven中加入与提供者中相同的依赖

4. 构建controller

   ```java
   @RestController
   public class DubboController {
   
       //不加interfaceClass的话，默认是引用类型的数据类型
       @DubboReference(interfaceClass = StudentService.class, version = "1.0")
       private StudentService service;
   
       @GetMapping("/query/{id}")
       public String queryStudent(@PathVariable Integer id){
           Student student = service.queryStudent(id);
           return student.toString();
       }
   }
   ```

5. 在启动类的上面加上@EnableDubbo

6. 设置配置文件application.properties

   ```properties
   #指定服务名称
   spring.application.name=consumer-application
   #指定注册中心
   dubbo.registry.address=zookeeper://localhost:2181
   ```


## 九、springboot打包

### jar

1. 创建了一个包含了jsp 的项目

2. 修改pom.xml

   指定打包后的文件名称

   ```xml
   <build>
       <!--打包后文件的名称-->
       <finalName>myboot</finalName>
   </build>
   ```

   指定springboot-maven-plugin版本

   ```xml
   <plugins>
       <plugin>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-maven-plugin</artifactId>
           <!--打包jar，有jsp文件时，必须指定maven-plugin插件的版本是1.4.2.Release-->
           <version>1.4.2.RELEASE</version>
       </plugin>
   </plugins>
   ```

   执行maven clean package，在target目录中，生成jar文件，例子是myboot.jar

   执行独立的springboot项目，在cmd中 java -jar myboot.jar

### war

1. 创建了一个jsp应用

2. 修改pom.xml

   指定打包后的文件名称

   ```xml
   <build>
       <!--打包后的文件名称-->
       <finalName>myboot</finalName>
   </build>
   ```

   指定jsp编译的目录

   ```xml
   <resources>
       <resource>
           <directory>src/main/webapp</directory>
           <targetPath>META_INF/resources</targetPath>
           <includes>
               <include>**/*.*</include>
           </includes>
       </resource>
       
       <!--使用了mybatis，而且mapper文件放在src/main/java目录-->
       <resource>
           <directory>src/main/java</directory>
           <includes>
               <include>**/*.xml</include>
           </includes>
       </resource>
       
       <!--把src/main/resources下面的所有文件都包含到classes目录-->
       <resource>
           <directory>src/main/resources</directory>
           <includes>
               <include>**/*.*</include>
           </includes>
       </resource>
   </resources>
   ```

   执行打包类型是war

   ```xml
   <!--打包类型-->
   <packaging>war</packaging>
   ```

3. 主启动类继承SpringBootServletInitializer

   (继承这个类，才能使用独立的tomcat服务器)

   ```java
   @SpringBootApplication
   public class JspApplication extends SpringBootServletInitializer{
       public static void main(String[] args){
           SpringApplication.run(JspApplication.class, args);
       }
       
       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder builder){
           return builder.sources(JspApplicatin.class);
       }
   }
   ```

4. 部署war

   把war放到tomcat等服务器的发布目录中，tomcat为例，myboot.war放到tomcat/webapps目录。

## 十、Thymeleaf模板引擎

是java开发的模板技术，在服务器端运行，把处理后的数据发给浏览器，模板是作为视图层工作的，显示数据的，Thymeleaf是基于html语言，Thymleaf语法是应用在html标签中。springBoot框架集成Thymeleaf，使用Thymeleaf代替Jsp.

构建项目时，模板引擎选择Thymeleaf

Thymeleaf官方网站：http://www.thymeleaf.org

Thymeleaf官方手册：https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html

命名空间：<html lang="en" xmlns:th="http://www.thymeleaf.org">

### 表达式

1. 标准变量表达式：

   语法：${key}

   作用：获取Key对于的文本数据，key是request作用域中的key，使用request.setAttribute()

   在页面中的html标签中，使用 th:text="${key}"

   ```html
   <p th:text="${user.name}">姓名</p>
   ```

2. 选择变量表达式 (星号表达式)

   语法：*{key}

   作用：获取这个key对应的数据，*{key}需要和 th:object 这个属性一起使用。

   目的是简单获取对象的属性值。

   ```html
   <div th:object="${user}">
       <p th:text="*{name}">姓名</p>
   </div>
   <!--等同于-->
   <p th:text="*{user.name}">姓名</p>
   ```

3. 链接表达式

   语法：@{url}

   作用：表示链接，可以用于

   ```html
   <a href=""></a>
   <form action=""></form>
   <link href="">
   <img src="">
   ```

   ```html
   <a th:href="@{/tpl/queryAccount}">相对地址</a>
   <a th:href="@{'/tpl/queryAccount?id=' + ${user.name}}">获取model中的数据</a>
   <a th:href="@{/tpl/queryAccount(name=${user.name})}">传参</a>
   ```

### Thymeleaf属性

属性是放在html元素中的，就是html元素的属性，加入了th: 前缀，属性的作用不变，但是它的值由模板引擎处理，属性可以使用变量表达式。

```html
<form th:action="/loginServlet" th:method="${methodAttr}"></form>
```

### th:each

1. 可以循环List，Array，Map

2. 语法:  

```html
<div th:each="集合循环成员[，循环的状态变量]：${key}">
    <p th:text="${集合循环成员}"></p>
</div>
<!--集合循环成员，循环的状态变量：两个名称都是自定义的，“循环的状态变量”这个名称可以不定义，默认是“集合循环成员Stat”-->
```

```html
<table>
    <thead>
        <tr>
            <td>count</td>
            <td>id</td>
            <td>name</td>
            <td>age</td>
        </tr>
    </thead>
    <tbody>
        <tr th:each="user:${myusers}">
            <td th:text="${userStat.count}"></td>
            <td th:text="${user.id}"></td>
            <td th:text="${user.name}"></td>
            <td th:text="${user.age}"></td>
        </tr>
    </tbody>
</table>
```

3. userStat

   是循环体的信息，通过该变量可以获取如下信息：

   index: 当前迭代对象的index（从0开始）

   count: 当前迭代对象的个数（从1开始计算）

   size: 被迭代对象的大小

   current: 当前迭代变量

   even/odd: 布尔值，当前循环是否是偶数/基数

   first: 布尔值，当前循环是否是第一个

   last: 布尔值，当前循环是否是最后一个

### th:if

判断句，当条件为true ，显示html标签体内，反之不显示。没有else语句

```html
<div th:if="${user.age > 20}">
    显示文本内容
</div>
```

还有一个判断："th:unless" 只有判断为假时，才执行内容。

### th:switch

```html
<div th:switch="${sex}">
    <p th:case="f">
        性别是女
    </p>
    <p th:case="m">
        性别是女
    </p>
    <p th:case="*">
        默认内容
    </p>
</div>
```

### th:inline

1. 内联text: 在html标签外，获取表达式的值

   ```html
   <!--th:inline="text"可写可不写-->
   <div th:inline="text">
      <p>
          我是[[${name}]],年龄是[[${age}]]。
      </p>
   </div>
   ```

2. 内联javaScript

   ```html
   <script type="text/javascript" th:inline="javascript">
       var myname=[[${name}]];
       var myage=[[${age}]];
       
       function fun(){
           alert("姓名:" + myname + ",年龄" + myage);
       }
   </script>
   ```

### 文本字面量

用单引号‘...’包围的字符串为文本字面量

```html
<p th:text="'姓名是'+${name}+'年龄是'+'${age}'">
    要显示的内容
</p>
```

### 字符串连接

连接字符串有两种语法

1. 语法使用单引号括起来字符串，使用+连接其他的字符串或者表达式（如上）

2. 使用双竖线，|字符串和表达式|

   ```html
   <p th:text="|我是${name},年龄${city}|">
       显示数据
   </p>
   ```

### 运算符

算数运算符： + - * / %

关系比较：> < >= <= (gt, lt, ge, le)

相等判断：== != (eq ne)

### Thymeleaf 内置对象

1. #request 表示HttpServletRequest
2. #session 表示HttpSession
3. session对象，表示Map对象，是#session的简单表示方式，用来获取session中指定的key的值
4. #dates，表示与时间相关的工具类
5. #numbers, 表示与数字相关的工具类
6. #strings，操作字符串的工具类
7. #lists，操作list的工具类

（这些内置对象，可以在模板文件中直接使用）

### 自定义模板

模板是内容复用，定义一次，在其他的模板文件中多次使用。

模板使用：

1. 定义模板
2. 使用模板

定义语法：

```html
<!--th:fragment="模板自定义内容"-->
```

引用语法：

```html
<!--top: 模板自定义内容
    head: 该模板所在html的名称-->
<!--插入两种方法-->
<div th:insert="~{head :: top}"></div>
<div th:insert="head :: top"></div>
<!--包含两种方法-->
<div th:include="~{head :: top}"></div>
<div th:include="head :: top"></div>
```

引用整个html: (同级目录)

```html
<div th:include="footer :: html"></div>
<div th:include="footer"></div>
```

使用其他目录中的模板文件

```html
<div th:insert="common/left :: html"></div>
<div th:insert="common/left"></div>
```

### javaScript

button的用法

```html
以前写法(请放弃)：
方式一：
<button class="btn" th:onclick="'getName(\'' + ${person.name} + '\');'">获得名字</button>
方式二：
<button class="btn" th:onclick="'getName(' + ${person.name} + ');'">获得名字</button>
方式三：
<button th:onclick="|getName(${person.name} )|">获得名字</button>

现在的写法：
<button class="btn" th:onclick="getName([[${person.name}]]);">获得名字</button> 3.0.10版本
```



## 十一、总结

### 重点注解（sss）

创建对象：

1. @Controller: 放在类的上面，创建控制器对象，注入到容器中
2. @RestController：放在类的上面，创建控制器，注入到容器中。是@Controller和@ResponseBody的复合，表示该控制器里的方法返回值都是数据。
3. @Service: 放在业务层实现类上面，创建service对象，注入到容器中。
4. @Repository: 放在dao层的实现类上面，创建dao对象，放入到容器中。没有使用这个注解，是因为现在使用MyBatis框架，dao对象是MyBatis通过代理生成的，不需要使用@Repository，所以没有使用。
5. @Component: 放在类的上面，创建此类的对象，放入到容器中。

赋值：

1. @Value: 简单类型的赋值，例如在属性的上面使用。还可以获取配置文件的数据（springboot的application.properties或yml）,例如@Value("${server.port}")

2. @Autowired: 引用类型赋值自动注入，支持byName，byType。默认是byType，放在属性的上面，也可以放在构造方法的上面（推荐）。
3. @Qualifer: 给引用类型赋值，使用byName方式。(2和3都是spring框架提供的)
4. @Resource: 在属性上使用。来自jdk中的定义，javax.annotation。实现引用类型的自动注入，默认byName, 失败再使用byType。

其他：

1. @Configuration: 放在类的上面，表示是个配置类，相当于xml配置文件。
2. @Bean: 放在方法上面，把方法的返回值对象，注入到spring容器中。
3. @ImportResource: 加载其他的xml配置文件，把文件中的对象注入到spring容器中。
4. @PropertySource：读取其他的properties属性配置文件。
5. @ComponentScan: 扫描器，指定包名，扫描注解的。
6. @ResponseBody：放在方法的上面，表示方法的返回值是数据，不是视图
7. @RequestBody: 把请求体中的数据，读取出来，转为java对象使用。
8. @ControllerAdvice: 控制器增强，放在类的上面，表示此类提供了方法，可以对controller增强功能。
9. @ExceptionHandler：处理异常的，放在方法的上面
10. @Transactional: 处理事务的，放在service实现类的public方法上面，表示此方法有事务。

SpringBoot中的注解

1. @SpringBootApplication: 放在启动类上面，包含了@SpringBootConfiguration@EnableAutoConfiguration@ComponentScan

MyBatis相关的注解

1. @Mapper：放在类的上面，让MyBatis找到接口，创建它的代理对象
2. @MapperScan：放在主类上面，指定扫描的包，把这个包中所有的接口都创建代理对象，对象注入到容器中
3. @Param：放在dao接口方法的形参前面，作为命名参数使用的。

Dubbo注解

1. @DubboService: 在提供者端使用的，暴露服务的，放在接口的实现类上面。

2. @DubboReference: 在消费者端使用，引用远程服务，放在属性上面使用。

3. @EnableDubbo: 放在主类上面，表示当前引用启用Dubbo功能。

### 汇总配置文件

提供者

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/student?useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=333

#mapper配置文件位置
mybatis.mapper-locations=classpath:mapper/*.xml
#指定myBatis的日志
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl

#指定redis
spring.redis.host=localhost
spring.redis.port=6379

#配置服务名称 dubbo:application name="名称"
spring.application.name=studentservice-provider
#配置扫描的包，扫描@DubboService
dubbo.scan.base-packages=com.xj.service
#注册中心
dubbo.registry.address=zookeeper://localhost:2181
```

消费者

```properties
#指定端口
server.port=9001
server.servlet.context-path=/demo

#指定服务名称
spring.application.name=consumer-application
#指定注册中心
dubbo.registry.address=zookeeper://localhost:2181
```







@ComponentScan默认扫描所在类所在包及其子包中的类。
