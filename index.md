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

3. 创建一个存放jsp的目录，一般叫做webapp

4. 需要在pom.xml指定jsp文件编译后的存放目录

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

5. 创建Controller，访问jsp

6. 在application.properties文件中配置视图解析器

   spring.mvc.view.prefix=/  (表示src/main/webapp)

   

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

序列化：把对象转化为可传输的子接序列过程叫做序列化

反序列化：把字节序列还原为对象的过程称为反序列化

目的：为了对象可以跨平台存储，进行网络传输















@ComponentScan默认扫描所在类所在包及其子包中的类。
