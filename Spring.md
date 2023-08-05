# Spring

IOC+DI=>使用对象时不仅可以从ioc容器中直接获取，而且获取到的bean已经绑定所有的依赖关系

IOC:控制反转（使用ioc容器管理bean）

使用对象时，由主动new产生对象转换为由外部提供对象，此过程中对象创建控制权由程序转移到外部

Spring对ioc的实现：

Spring提供一个容器（IOC容器/Spring容器）来充当ioc思想的外部

DI：依赖注入（在ioc容器内将有依赖关系的bean进行关系绑定）

在容器中建立bean与bean之间依赖关系的整个过程

配置bean

<bean id="名" name=“别名（空格，逗号，分号区分都可以）” class="bean的路径" scope="(prototype->非单例 singleton->单例)"/>

获取IOC容器：

ApplicationContext ctx = new ClassPathXmlApplicationContext(configLocation:"applicationContext.xml");

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

## 获取bean的方式：

### 1.     使用bean的名称，强转

BookDao bookDao = (BookDao)ctx.getBean(bookDao);

### 2.     使用bean名称获取并指定类型

BookDao bookDao = ctx.getBean(bookDao,BookDao.class);

### 3.     使用bean类型获取

BookDao bookDao = ctx.getBean(BookDao.class);

DI:

在<bean>中加<property name="引用对象名" ref="ioc容器中的名字">

工厂静态方法模式

<bean id="" class="bean工厂的地址" factory-method="new bean 的方法"/?

实例工厂模式

<bean id="factory别名" class="factory路径"/>

<bean id="bean别名" factory-method="factory中实例化bean的方法" factory-bean="factory的别名，和上面的tag名一样 />"

factoryBean实例化bean

<bean id = “” class=”factoryBean的路径”/>

关闭IOC容器

ClassPathXmlApplicationContext.registerShutdownHook();

Bean生命周期

\1.  初始化

-创建对象（内存分配）

-执行构造方法

-执行属性注入（set操作）

-执行bean初始化方法

\2.  使用bean

-执行业务操作

\3.  关闭/销毁容器

-执行bean销毁方法

Bean生命周期的控制

\1.  配置

--init-method

--destroy-method

\2.  关闭容器

ConfigurableApplicationContext

--close();

--registerShutdownHook();

两个方法close更暴力，立即关闭，

registerShutdownHook等虚拟机关闭之前关闭容器，勾子方法

依赖自动装配：

IOC容器根据bean所依赖的资源在容器中自动查找并注入到bean中的过程成为自动装配

自动装配用于引用类型依赖注入，不能对简单类型进行操作

使用按类型装配时（byType）必须保障容器中相同类型的bean唯一，推荐使用

使用按名称装配时（byName）必须保障容器中具有指定名称的bean，因为变量名和配置高耦合，不推荐使用

Spring管理第三方数据源

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

Druid不用引入mysql坐标

C3p0需要引入mysql坐标

Spring配置文件读取其他配置文件

<上方命名的namespace名:property-placeholder location=”配置文件路径”/>

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image006.jpg)

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image008.jpg)

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image010.jpg)

Spring总结：

BeanFactory是ioc容器的顶层接口，初始化BeanFcatory对象时加载的bean是延迟加载。

ApplicationContext接口是Spring容器的核心接口，初始化bean时立即加载（可以在配置文件中<bean>加入init-leazy=”true”）,

ApplicationContext接口提供基础的bean操作相关方法，通过其他接口拓展其功能。

ApplicationContext接口常用的实现类：

\1.   ClasspathXmlApplicationContext(“配置文件名xxx.xml”);

\2.   FilesystemXmlApplicationContext(“配置文件的绝对路径”);

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image012.jpg)

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image014.jpg)

@Autowired是根据类型装配

@Qualifier+@Autowired根据Name注入

Qualifier不能单独使用

自动装配基于反射设计创建对象并暴力反射对应属性为私有属性初始化数据，

 

 

读取外部properties文件

在配置类中加@PropertySource(classPath:”配置文件名”);

添加@Bean表示方法返回值是个bean

第三方bean管理：

1.[在SpringConfig.java中引入其他配置类@import（其他类配置类.class）](mailto:在SrpingConfig.java中引入其他配置类@import（其他类配置类.class）)

@import只能写一次，如果写多个就{}

\3.   在SpringConfig.java中扫描其他配置类，@componentscan（扫描配置包），并且两个配置类都需要有@configuration

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image016.jpg)

注解开发总结

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image018.jpg)

 

 

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image019.jpg)

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image021.jpg)

 

 

 

 

 

 

Spring整合Mybatis

\1.   pom文件中加入两个坐标

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image023.jpg)

\2.   在mybatis配置文件中

（1）配置SqlsessionFactoryBean扫描model包

（2）配置mapperScannerConfigurer扫描mapper映射

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image025.jpg)

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image027.jpg)

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image029.jpg)



Spring整合Junit

在测试类用注解标记

@runwith(SpringJunit4ClassRunner.class)->类运行器

@contextConfiguration(classes=SpringCoinfig.class)

 

AOP：面向切面编程->在不惊动原始设计的基础上为其进行功能增强=>无侵入式编程

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image031.jpg)

连接点：所有的方法

切入点：选择被加强的方法

通知：共性功能=>加强的方法

通知类：定义通知的类

切面：切点和通知的对应关系

 

## 实现AOP

1. 在Spring配置类中加@EnableAspectjProxy注解，声明用注解配置aop

2. 在通知类上加入@Component注解，将通知类放到ioc容器中被扫描

3. 在类上加上@Aspect

4. 定义一个无参数无返回值的空方法，用@pointcut标记(“execution(切入点的返回值类型 切入点的绝对路径->到方法上)”)

可以描述接口也可以描述实现类------切入点表达式

5. 在通知上用@before等注解修饰在什么时候执行

### AOP工作流程

1. Spring容器启动

2. 读取所有切面配置的切入点

3. 初始化bean，判定bean对应的类中的方法是否匹配到任意的切入点->

匹配失败，创建对象

匹配成功，创建原始对象（目标对象）的代理对象

4. 获取bean执行方法->

获取bean，调用方法并执行，完成操作

获取的bean是代理对象时，根据代理对象的运行模式运行原始方法和通知，完成操作；



切入点表达式：

Execution(public 返回值类型 包类或者接口.方法（）)

动作关键字：描述切入点的行为动作，例如execution表示执行到切入点

访问修饰符：可以省略

返回值：

包：

类或者接口：

方法名：

参数：

异常：可以省略

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image033.jpg)

*是任意一个，..任意可以多个

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image035.jpg)

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image037.jpg)

![对话气泡: 矩形: 调用原有方法体](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image038.gif)![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image039.gif)![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image041.jpg)

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image043.jpg)

 

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image045.jpg)

 

AOP总结：

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image047.jpg)

作用：在不惊动原有代码设计的基础上对功能的增强

核心概念：

1. 代理：核心是采用代理模式实现

2. 连接点：所有方法都叫连接点

3. 切入点：织入通知的方法叫切入点

4. 通知：共同方法

5. 切面：通知{切点=>切面

6. 目标对象：被代理的原始对象

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image049.jpg)

前置通知和后置通知依赖JoinPoint才能实现对原始方法的参数，返回值调用

形参（ProceedingJoinPoint和JoinPoint）都必须作为第一个参数引用，否则会报非法参数异常

#### Spring管理事务

1. 在类或者接口或里面的方法上用@transaction注解

2. 设置事务管理器

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image051.jpg)

3. 开启注解式事务驱动->在springConfig.java类上加@enableTransactionManager

事务的传播行为：

事务协调员对事务管理员所携带事务的处理态度

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image053.jpg)

Propagation：事务的传播行为↑属性



# SpringMVC

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image055.jpg)

@RequestMapping设置访问路径

@responseBody设置当前控制器方法相应内容为当前返回值

因为controller层的bean在springmvc容器中加载，所以要避免controller的bean被springIOC容器扫描到：

\1.     spring容器扫描精确定位，只扫描service包和dao包，不扫controller包

\2.     扫描最大包，排除controller包componentScan（vaule=”大包”,excloud=”@componentScan.filter(type=fielterType.annotation,classes=”contorller.class”)”）

 

Springmvc实现：

1. 导包

2. 配置前端控制器（servlet）stuts2前端控制器是过滤器

3. 编写controller

4. 将controller使用注解配置到springMVC容器中（@controller）

5. 配置spring-mvc.xml文件，配置组件扫描   

 

在类上用@requestmapping注解作为当前controller请求前缀路径，

Get请求？传参  =>参数名=值

后台不区分get/post请求

## Post中文乱码

在springmvc配置类中加过滤器，且配置字符集UTF-8；

在后台controller方法里加入@requestparam（“前台传的参数”）可以将前台的name转化成后台的参数

传递pojo对象的时候，如果前台传的参数和后台pojo对象的属性值对的上，就可以直接赋值

如果传递的是list集合，需要在参数上加@requestparam

总结：参数对不上就加@requestparam

 

传递json：

1.在springmvc配置文件中加@enablewebmvc

2.在方法里加@requestbody

接受日期类型参数时

用@datetimeFormat（pattern=“格式”）

Web配置类servletconfig过滤器=>过滤中文乱码



## SpringMVC执行流程

 

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image057.jpg)

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image059.jpg)

1. 用户发送请求到前端控制器dispatcherservlet

2. Dispatcherservlet收到请求调用handlerMapping处理器映射器

3. 处理器映射器找到具体的处理器（可以根据xml配置，注解进行查找），生程处理器对象及处理器拦截器并返给dispatcherservlet

4. Dispatcherservlet调用handleradapter处理器适配器
5. Handleradapter经过适配调用具体的处理器（controller，后端控制器）

6. Controller执行返回model and view

7. handler adapter将controller返回结果返回给dispatcherservlet

8. Dispatcherservlet将modelandview传给viewresloer视图解析器

9. 视图解析器返回view

10. Dispatcherservlet根据view进行渲染视图

默认跳转是转发

Springmvc的数据相应方式

\1.     页面跳转

（1）直接返回字符串

（2）通过modelandview对象返回

\2.     回写数据

（1）直接返回字符串

（2）返回对象或集合

 

 

对象转json：ObjctMapper中的writevalueAsString（user）;

在配置文件中配置处理器映射器messageconvert

 

Mvc中处理器映射器，处理器适配器视图解析器是mvc三大组件

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image061.jpg)

 

 

### 获得请求参数

1. 基本类型参数

2. Pojo类型参数

3. 数组类型参数

4. 集合类型参数

获得基本数据类型参数

Controller中的业务方法的参数名称要与请求参数的name一致

获得pojo类型参数

Controller中的方法pojo参数的属性名和请求参数的name一致

获取数组类型参数

Controller中的方法数组名称和请求参数的name一致

获取集合类型参数：

获得集合参数时，要将集合参数包装到一个pojo中才可以

在参数列表用@request body

 

开发资源的访问

<mvc:resources mapping=”/js/**” location=”/js/”/>

Tomcat容器默认的开发资源访问

<mvc:default-servlet-handler/>

 

@requestheader获取请求头  useragent

@cookievalue可以获取cookie得值

 

 

 

文件上传

文件上传客户端三要素

1)    input框要type=file

2)    Post提交

3)    表单的enctype属性是多部份表单形式。Enctype=“multipart/form-data”

 配置文件上传解析器

​                                                            

导入坐标 io和fileupload

配置文件上传解析器

 

 

Mvc获取数据细节

@requestparam：当请求参数和形参名称不一样时，可以转名

@pathvariable ：restful风格读取路径参数

自定义类型转换器：日期

1.创建类型转换器实现converter接口

2.在springmvcxml文件注册

3.annotation-driven注解

获得servlet相关api：形参，springmvc框架调用时框架进行注入

@Requestheader和@cookievalue



Jdbc模板

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image063.jpg)

Jdbc中封装了模板，tx中封装了事务

项目异常：

规范的用户行为产生的异常

不规范的用户行为产生得异常

※发送对应消息传递给用户，提醒规范操作

系统异常：

项目运行中可预计且无法避免的异常

※发送固定消息传递给用户，安抚用户

※发送特定消息给运维人员，提醒维护

※记录日志

其他异常：

※发送固定消息传递给用户，安抚用户

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image065.jpg)

@restcontrolleradvice生命处理rest风格得controller

@exceptionhandler（拦截异常种类.class）

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image067.jpg)

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image069.jpg)

过滤器是在tomcat配置，可以过滤所有请求

拦截器是能对springmvc得访问做拦截

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image071.jpg)

### 配置拦截器

1.声明拦截器的bean 实现handlerinterceptor接口，重写三个方法

定义

2.一个配置类继承WebMvcConfigurationSupport实现addinterceptors方法的

3.添加拦截器所设定的访问路径，路径可以通过可变参数设置多个

4.使用标准接口WebMvcConfigurer简化开发（侵入性较强）

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image073.jpg)

 

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image075.jpg)

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image077.jpg)

多个拦截器执行顺序

当配置多个拦截器时，形成拦截器链，

拦截器链的运行顺序参照拦截器添加顺序为准

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image079.jpg)

当拦截器中出现对原始处理器的拦截，后面的拦截器均终止运行

当拦截器运行终端，仅运行配置在前面的拦截器的aftercompletion

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image081.jpg)

# Spring源码分析

## Bean实例化的流程

Spring容器再进行初始化的时候，会将xml配置的<bean>信息封装成一个BeanDefinition对象，所有的BeanDefinition存储到一个名为beanDefinitionMap的Map钟，Spring对其进行遍历，使用反射创建Bean实例，创建好的bean对象存储在一个名为singletonObjects的Map种，当调用getbean方法时，最终从singletonObject种获取bean实例返回



·加载xml文件，解析每个<bean>,封装成一个个的beandefinition对象

·将beandefinition存储在一个beandefinitionMap中

·applicationContext遍历beandefinitionMap，创建bean实例

·将bean实例放到名为singletonObjects的map中

·getbean()，从singletonObjects中获取

 

## Spring后处理器

·BeanFactoryPostProcessor：Bean工厂后处理器，在BeanDefinitionMap填充后，Bean实例创建前

子接口BeanDefinitionReigistryPostProcessor专门用于注册BeanDefinition操作

·BeanPostProcessor：Bean后处理器，一般在Bean实例化之后，填充到SingletonObjects之前



## SpringBean的生命周期

判断当前bean是否是单例，延迟加载，factorybean

将bean放到beanDefinitionMap中

执行BeanFactoryPostProcessor方法

通过反射创建bean对象

属性填充，涉及到依赖注入，三级缓存

Aware接口方法调用

执行BeanPostProcessor中PostProcessorBeforeInitialization方法

执行InitializingBean中的初始化方法

执行自定义init-method方法

执行BeanPostProcessor中PostProcessorAfterInitialization方法

将创建完的bean放到singletonObjects

 

Spring在进行属性注入时，会分为如下几种情况：

·注入普通属性，String、int或存储基本类型的集合时，直接通过set方法的反射设置进去

·注入单向对象引用属性时，从容器中getBean获取后通过set方法反射设置进去，如果容器没有，则先创建注入对象bean实例，在进行注入。

·注入双向对象引用属性时，涉及循环依赖

 

### 解决循环依赖->三级缓存

在DefaultListableBeanFactory的上四级父类DefaultSingletonBeanRegistry中提供三个map

singletonObjects->bean初始化都完毕了

earlySingletonObjects->当前对象已经被其他对象引用，半成品bean

singletonFactories->半成品bean，对象未被引用，使用时在工厂创建bean，getObject（）;

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image083.jpg)

AOP相关概念

目标对象 Target 被增强的方法所在对象

代理对象 Proxy 对目标对象进行增强后的对象，客户端实际调用的对象

连接点   Joinpoint 目标对象中可以被增强的方法

切入点   Pointcut  目标对象实际被增强的方法

通知     Advice   增强部分的代码逻辑

切面     Aspect   增强和切入点的组合

织入     Weaving  将通知和切入点组合动态组合的过程 

 

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image085.jpg)

 