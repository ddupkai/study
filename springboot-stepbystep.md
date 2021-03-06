## 一步一步学springboot2 微服务项目实战（黄文毅）

> create by sunyongguo  2018ISBN

> 本书偏于实践，根据作者工作经验而来，使用软件版本：win10 idea2016.2 jdk1.8 tomcat1.8 springboot 2.0.0RCI

> 源代码 ： git@github.com:huangwenyi10/stepbystep-learn-springboot.git  教学视频:https://pan.baidu.com/s/1A5xjEcvE5A2T6I5bmAmkYQ
> 如果在下载中遇到问题，可发送邮件至booksage@126.com获取帮助，邮件标题：一步一步学SpringBoot2:微服务项目实战。
> 笔者邮箱：huangwenyi10@163.com  博客：http://blog.csdn.net/huangwenyi1010


### 第一章 第一个springboot项目
>  侧重工具安装，简单使用，在工作中基本要求目测可以达到，都是比较基础的内容；
      springboot是目前流行的微服务框架，倡导“约定大于配置”（理解，如springboot默认的项目路径规则，即体现了这个理念），主要是简化spring应用的初始化搭建以及开发过程，
   * 介绍安装jdk,idea,maven到本地； idea配置maven；
   * idea中使用springInitializr新建项目，搭建springboot项目，springboot默认目录结构，springboot会默认在src/main/resources下找到application.properties 或application.yml配置文件，可以2者并存
   * springboot的maven依赖
   * maven helper插件的安装和使用， 在idea中安装插件，查看依赖，查看冲突，exclude排除冲突包，跳转到源码。

### 第二章 集成mysql数据库
> mysql安装，idea中pom配置依赖包，test连接数据库，亮点是最后使用阿里druid数据库连接池，使用步骤就是pom.xml配置依赖包，配置初始化项。
   * pom.xml 引入依赖，mysql-connector-java spring-boot-starter-jdbc,集成测试连接数据库demo
   * 使用navicat连接数据库，使用IDEA自身连接数据库，查询；
   * 集成druid, pom导入依赖，application.properities配置内容，使用java文件实现注册配置bean（bean相当于xml里的bean，java文件替代xml），进行测试
      使用druid依赖，貌似要先在spring-boot-starter-boot排除tomcat-jdbc，然后再导入druid依赖，druid是jdbc的一个组件，包括3部分，1.DruidDriver代理driver，能够提供基于Filter-chain模式的插件体系；2
      DruidDataSource高效可管理的数据库连接池，3.SQLParser，支持所有jdbc兼容的数据库 ；监控功能看监控数据库连接池和sql查询工作，提高数据库访问性能
  
### 第三章 集成spring data JPA
  jap定义的查询关键字： https://docs.spring.io/spring-data/data-jpa/docs/current/reference/html
> spring data jpa 核心接口和继承关系，在springdata 中集成springdatajpa 以及通过spring data jpa 实现增删改查及自定义查询
   spring data jpa是sun 官方提出的规范，是规范而不是产品； 
####1. *注解解释：*
      @component：泛指组件，如果组件不归类，可以使用这个组件， @service， 组件，更加生命是服务层类，@repository， 持久层组件，即dao组件。
      @resource：属于j2ee，默认按照name进行装配，如果写在setter方法上，默认取属性名进行装配，如果找不到与名称匹配的bean时，才会按照类型进行装配。如 @Resource(name = "ayUserRepository")
      @autowired: 属于spring注解，默认按类型装配，默认情况下要求依赖对象必须存在，如果允许null，那么设置它的required属性为false； @autowired(required=false);
       如果想使用名称装配，可以结合@qualifier注解使用。如
       @Autowired
       @Qualifier("ayUserRepository")
       private AyUserRepository ayUserRepository;

####2. demo中的主要类
      AyUserRepository,然后在AyUserService,AyUserServiceImpl中调用， 集成测试过程中，使用断言测试；Assert，主要方法Assert.isTrue; Assert.isNull;
   
   
### 第四章 使用thymeleaf模板引擎
thymeleaf 官网： http://www.thymeleaf.org/
> spring boot中使用thymeleaf, idea插件Rest Client工具测试restful 接口
#### 1.使用themeleaf  
      themeleaf 是面向java的xml/xhtml/html5的页面引擎，包含表达式，标签，函数；  themeleaf在pom文件导入依赖，配置thymeleaf的一些配置项；   在controller层进行开发，返回数据，在thymeleaf中开发，接受数据；
#### 2.测试
      测试能否在页面看到上面写的代码，返回数据；
      使用Rest Client工具，测试RESTful Web Servcie，在idea-> tools-> test restful web service测试请求。


### 第五章 springboot 事务管理
> 本章是重点，也是自己的弱项
#### 5.1 spring 事务管理
      spring 在不同的事务管理API上定义了一层抽象层，开发人员不必要了解底层的事务管理API，就可以使用Spring事务管理机制。spring既支持编程式事务管理，也支持声明式事务管理。
    spring声明式事务
      spring配置文件中关于事务配置由3部分组成，分别是DataSource,TransactionManager和代理机制。不论哪种配置方式，一般变化的只有代理机制部分，
      DataSource和transactionManager这2部分只会根据数据访问方式变化。
      spring 声明式事务配置提供5中配置方式，而实际使用基于Annotation方式的注解目前比较流行。
      @Transactional注解在类上或方法上声明一个事务，被调用时spring开启一个事务，当方法正常运行时，spring会提交这个事务。
      spring提供@EnableTransactionManagement注解在配置类上来开启声明式事务的支持，使用@EnableTransactionManagement后，spring会自动扫描注解@Transactional的类和方法。
#### 5.2.1 事务的传播行为；  
    spirng 当事务被另一个事务调用时，必须指明事务如何传播，传播行为可在@Transactional的属性中指定，spirng定义了7种传播行为。具体见官网。 使用@Transactional的propagation属性定义事务传播行为。
#### 5.2.2 事务的隔离级别：
    并发事务导致的问题分为3类，脏读，幻读，不可重复读。
    针对此，spring提供5种事务的隔离级别，spring @Transaction的isolation属性定义隔离级别，对应具体的5中事务隔离级别。
      @transaction timeout  属性设置事务过期时间。
      @transaction readOnly  指定当前事务是否是只读事务
      @transaction rollbackfor(noRollbackFor)  指定哪个或哪些异常可以引起事务回滚。
    
#### 5.3 springboot事务使用
    springboot 默认开启了事务，使用@Transaction即可。springboot默认对jpa，jdbc，mybatis开启了事务。直接使用注解即可，如果使用其他orm框架，要自己配置事务管理器。 springboot 不需要像spring一样无须显式声明@enableTransactionManagement.
       spirngboot 用于配置事务的类为TransactionAutoConfiguration;依赖于JtaAutoConfiguration和 DataSourceTransactionManagerAutoConfiguration. 
       由于datasourceTransactionManagerAutoConfuguration有了对事务的支持（@enableTransactionManagement）,所以springboot中直接使用事务即可。

    类级别的事务： @Transactional在类上，以为着此类的所有public方法都是开启事务的。
    方法级别事务： @tracsactional 方法上加@Transactional； 如果报异常，则回滚。自行测试






## 参考文献
 * javaee开发的颠覆者springboot实战
   * spring源码深度`解析`
 * springboot揭秘
 * https://spring.io/guides
 * https://docs.spring.io/spring-boot/docs/current-SNAPSHOP/reference/htmlsingle
