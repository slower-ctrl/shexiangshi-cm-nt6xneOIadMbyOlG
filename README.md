
# Spring


## 简介


  一般来说，Spring指的是SpringFramework，它提供了很多功能，例如：**控制反转（IOC）、依赖注入**


**（DI）、切面编程（AOP）、事务管理（TX）**


## 主要 jar 包


* **org.springframework.core：**Spring的核心工具包，其他包依赖此包
* **org.springframework.beans：**所有应用都用到，包含访问配置文件，创建和管理bean等
* **org.springframework.aop：**Spring的面向切面编程，提供AOP的实现
* **org.springframework.context：**提供在基础IOC功能上的扩展服务，此外还提供许多企业级服务的支持（邮件服务、任务调度、JNDI定位、EJB集成、远程访问、缓存等）
* **org.springframework.web.mvc：**包含SpringMVC应用开发时所需的核心类
* **org.springframework.transaction：**为JDBC、Hibernate、JPA提供一致的声明式和编程式事务管理
* **org.springframework.web：**包含web应用开发时，用到的Spring框架时所需的核心类
* **org.springframework.aspect：**Spring提供的对Aspect框架的整合
* **org.springframework.test：**对JUNIT等测试框架的简单封装
* **org.springframework.context.support：**Spring context的扩展支持，用于MVC方面
* **org.springframework.expression：**Spring表达式语言
* **org.springframework.jdbc：**对JDBC的简单封装
* **org.springframework.web.servlet：**对J2EE6\.0 servlet3\.0的支持


## IOC（控制反转）


### 组件和组件管理


 整个项目由各种组件搭建而成
![](https://img2024.cnblogs.com/blog/3552536/202412/3552536-20241211103518144-1355251511.png)



> **在Spring中，组件交给Spring容器进行管理，代替程序员之前new对象的操作，控制对象的创建，管理对象的生命周期**


### 优势


* **降低了组件之间的耦合性：**Spring IoC容器通过依赖注入机制，将组件之间的依赖关系削弱，减少了程序组件之间的耦合性
* **提高了代码的可重用性和可维护性：**将组件的实例化过程、依赖关系的管理等功能交给Spring IoC容器处理，使得组件代码更加模块化、可重用、更易于维护。
* **方便了配置和管理：**Spring IoC容器通过XML文件或者注解，轻松的对组件进行配置和管理，使得组件的切换、替换等操作更加的方便和快捷


### 容器管理配置方式


* **XML配置文件：**在XML文件中定义Bean及其依赖关系、Bean的作用域等信息，让Spring IoC容器来管理Bean之间的依赖关系
* **注解方式配置：**通过在Bean类上使用注解来代替XML配置文件中的配置信息。通过在Bean类上加上相应的注解（如@Component, @Service, @Autowired等），将Bean注册到Spring IoC容器中
* **Java配置类配置：**通过Java类来定义Bean、Bean之间的依赖关系和配置信息，通过@Configuration、@Bean等注解来实现Bean和依赖关系的配置


### 实例化方式


* **构造器实例化**


	+ 在配置文件中定义`bean`（对应bean中要有空参构造）
	
	
	
	```
	
	    
	
	
	```
	+ 编写构造器
	+ 获取实例化对象
	
	
	
	```
	public void test1(){
	        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
	        UserService userService = context.getBean(UserService.class);
	    }
	
	```
* **静态工厂实例化**



```


```
* **实例化工厂实例化**



```




```


### Spring Bean



> 指被Spring容器管理的对象


#### 将一个类声明为Bean的注解


* `@Component`：通用的注解，可标记任何类为Spring组件
* `@Service`：对应服务层，主要涉及一些复杂的逻辑
* `@Repository`：对应持久层，主要用于数据库相关操作
* `@Controller`：对应SpringMVC控制层，主要用于接收用户请求，并调用Service层，返回数据给前端



> **`@Component`和`@Bean`**
> 
> 
> @Component 作用于类，@Bean作用于方法
> 
> 
> @Bean 比 @Component 的自定义性更强，有些地方只能用 @Bean 注解来注册bean，如：当我们需要引用第三方库中的类，需要装配到Spring容器中时，只能通过 @Bean 注解实现


#### Bean 的作用域


* **singleton：**容器中只有唯一的bean实例，Spring中bean默认都是单例的**（默认）**
* **prototype：**每次获取bean时都会创建一个新的bean实例
* **request（仅web应用可用）：**每次http请求都会产生一个新的bean，仅在该HTTP Request内生效
* **session（仅web应用可用）：**每一次来自新 session 的 HTTP 请求都会产生一个新的 bean，仅在当前 HTTP session 内有效


#### Bean 的生命周期


 实例化 \-\> 属性赋值 \-\> 初始化 \-\> 销毁



> 在实例化阶段，Spring容器通过[反射](https://github.com)机制创建Bean，然后进行依赖注入，如果Bean实现了`BeanNameAware`等接口，容器会调用相应方法
> 
> 
>  在初始化阶段，`BeanPostProcessor`会在前后进行拦截处理，最后调用初始化方法，例如`@PostConstruct`或`afterPropertiesSet`
> 
> 
>  销毁时则会调用`DisposeableBean.destory()`或自定义的销毁方法


## DI（依赖注入）



> 在组件之间传递依赖关系的过程中，将依赖关系放入容器内部进行处理，降低依赖关系，实现了对象间的解耦合


### 实现方式


1. 通过构造器注入**（对应类中必须有对应的构造函数）**



```

    
    
    


```
2. 通过setter方法注入



```

    
    


```
3. 通过字段注入



> 通过字段注入，也就是通过注解进行注入（`@Autowired`、`@Resource`）


 **@Autowired 和 @Resource 的区别**


* `@Autowired`是 Spring 提供的注解，默认的注入方式是按照类型进行匹配，也就是说会优先通过接口类型去匹配并注入bean，当接口存在多个实现类时，byType这种方式就无法正确注入对象了，可以通过结合`@Qualifier`注解来显式指定名称
* `@Resource`是JDK提供的注解，默认注入方式是按照名称进行匹配，如果无法通过名称匹配到对应的bean，就会按照类型来匹配
* `@Autowired`支持在构造方法、方法、字段、参数上使用，`@Resource`用于字段和方法上的使用，不支持在构造方法和参数使用


## AOP（面向切面编程）



> 横向编程，在不改变核心业务逻辑的情况下，对一些功能进行增强（常用在日志管理、权限控制、事务处理、安全控制）


### Spring AOP的底层原理


**基于JDK的动态代理实现、基于cglib的字节码生成实现**


 AOP 的底层是两种动态代理都有，默认情况下，如果目标类实现了一个或多个接口，AOP自动采用的是JDK动态代理进行增强，如果目标类没有实现接口，AOP采用cglib字节码生成的方式来增强，当然，也可用强制使用cglib字节码生成的方式进行增强，这样就不用考虑目标类是否实现了接口


### 基本概念


* **join point（连接点）：**目标类中的所有方法
* **pointcut（切点）：**对哪些方法进行拦截
* **advice（通知）：**拦截到方法要干什么
* **aspect（切面）：**通知和切点的结合
* **target（目标）：**被代理的目标对象


### Spring AOP 和AspectJ AOP


 Spring AOP属于运行时增强，AspectJAOP属于编译时增强，SpringAOP是基于代理，AspectJAOP是基于字节码操作，切面比较少时，没有太大区别，切面较多时，建议使用AspectJAOP


### 通知类型


* **前置通知：**在被代理的目标方法前执行
* **后置通知：**在被代理的目标方法最终结束后执行
* **环绕通知：**使用try...catch...finally结构围绕整个被代理的目标方法，包括上面四种通知对应的所有位置
* **异常通知：**在被代理的目标方法异常结束后执行
* **返回通知：**在被代理的目标方法成功结束后执行



> 可以通过XML配置或者注解的方式实现


### 实现


1. 导包（添加依赖）
2. 通过注解或者XML配置



```
// 通过注解方式
// @Aspect表示这个类是一个切面类
@Aspect
// @Component注解保证这个切面类能够放入IOC容器
@Component
public class LogAspect {

    // @Before注解：声明当前方法是前置通知方法
    // value属性：指定切入点表达式，由切入点表达式控制当前通知方法要作用在哪一个目标方法上
    @Before(value = "execution(public int com.atguigu.proxy.CalculatorPureImpl.add(int,int))")
    public void printLogBeforeCore() {
        System.out.println("[AOP前置通知] 方法开始了");
    }
    
    @AfterReturning(value = "execution(public int com.atguigu.proxy.CalculatorPureImpl.add(int,int))")
    public void printLogAfterSuccess() {
        System.out.println("[AOP返回通知] 方法成功返回了");
    }
    
    @AfterThrowing(value = "execution(public int com.atguigu.proxy.CalculatorPureImpl.add(int,int))")
    public void printLogAfterException() {
        System.out.println("[AOP异常通知] 方法抛异常了");
    }
    
    @After(value = "execution(public int com.atguigu.proxy.CalculatorPureImpl.add(int,int))")
    public void printLogFinallyEnd() {
        System.out.println("[AOP后置通知] 方法最终结束了");
    }
}

```
3. 开启`aspectj`注解支持



```
xml version="1.0" encoding="UTF-8"?


    
    
    
    


```


## TX（事务）



> 一系列动作中，只要有一个出现问题，所有动作全部回滚到事务开始的状态，避免了数据不一致导致的错误


### 对应依赖


* **spring\-tx：**包含声明式事务实现的基本规范（事务管理器规范接口和事务增强等等）
* **spring\-jdbc：**包含DataSource方式事务管理器实现类DataSourceTransactionManager
* **spring\-orm：**包含其他持久层框架的事务管理器实现类例如：Hibernate/Jpa等


![](https://img2024.cnblogs.com/blog/3552536/202412/3552536-20241211153442205-19222329.png)


### 底层原理


 对事务的操作本来是由数据库进行的，但是为了方便用户进行业务逻辑的控制，Spring对事务进行了扩展实现； 在Spring中，事务是通过**AOP代理**实现的，对被代理对象的每个方法进行拦截，在方法执行前启动事务，在方法执行完成后根据是否有异常及异常的类型进行提交或回滚。



> **核心接口：**PlatFormTransactionManager


### 编程式事务和声明式事务


* **编程式事务：**在代码中硬编码，通过`TransactionTemplate`或者`TransactionManager`手动管理事务
* **声明式事务：**在XML文件中进行配置或通过注解实现


### 四个特性


* **原子性：**事务包含的所有数据库操作，要么全部成功，要么失败全部回滚
* **一致性：**事务必须使数据库从一个一致性状态转换为另一个一致性状态
* **持久性：**事务一旦提交，对数据的改变是永久性的
* **隔离性：**当多个用户并发访问数据库时，多个并发事务之间要相互隔离，不被其他事务操作所干扰


### 事务属性


#### 只读（readonly）



> 对一个查询操作来说，如果我们把它设置成只读，就能够明确告诉数据库，这个操作不涉及写操作。



```
// readOnly = true把当前事务设置为只读 默认是false!
@Transactional(readOnly = true)

```

#### 超时（timeout）



> 事务在执行过程中，有可能因为遇到某些问题，导致程序卡住，从而长时间占用数据库资源。设置超时属性，超时回滚，释放资源



```
@Service
public class StudentService {

    @Autowired
    private StudentDao studentDao;

    /**
     * timeout设置事务超时时间,单位秒! 默认: -1 永不超时,不限制事务时间!
     */
    @Transactional(readOnly = false,timeout = 3)
    public void changeInfo(){
        studentDao.updateAgeById(100,1);
        //休眠4秒,等待方法超时!
        try {
            Thread.sleep(4000);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        studentDao.updateNameById("test1",1);
    }
}

```

#### 事务异常（rollbackFor）



> 默认只针对运行时异常，编译时异常不回滚


* **设置回滚异常**（rollbackFor）



```
/**
 * timeout设置事务超时时间,单位秒! 默认: -1 永不超时,不限制事务时间!
 * rollbackFor = 指定哪些异常才会回滚,默认是 RuntimeException and Error 异常方可回滚!
 * noRollbackFor = 指定哪些异常不会回滚, 默认没有指定,如果指定,应该在rollbackFor的范围内!
 */
@Transactional(readOnly = false,timeout = 3,rollbackFor = Exception.class)
public void changeInfo() throws FileNotFoundException {
    studentDao.updateAgeById(100,1);
    //主动抛出一个检查异常,测试! 发现不会回滚,因为不在rollbackFor的默认范围内! 
    new FileInputStream("xxxx");
    studentDao.updateNameById("test1",1);
}

```
* **设置不回滚的异常（norollbackFor）**



```
@Service
public class StudentService {

    @Autowired
    private StudentDao studentDao;

    /**
     * timeout设置事务超时时间,单位秒! 默认: -1 永不超时,不限制事务时间!
     * rollbackFor = 指定哪些异常才会回滚,默认是 RuntimeException and Error 异常方可回滚!
     * noRollbackFor = 指定哪些异常不会回滚, 默认没有指定,如果指定,应该在rollbackFor的范围内!
     */
    @Transactional(readOnly = false,timeout = 3,rollbackFor = Exception.class,noRollbackFor = FileNotFoundException.class)
    public void changeInfo() throws FileNotFoundException {
        studentDao.updateAgeById(100,1);
        //主动抛出一个检查异常,测试! 发现不会回滚,因为不在rollbackFor的默认范围内!
        new FileInputStream("xxxx");
        studentDao.updateNameById("test1",1);
    }
}

```


#### 事务隔离级别


* **读未提交：**事务可以读取未被提交的数据，容易产生脏读、不可重复读和幻读等问题。实现简单但不太安全，一般不用。
* **读已提交：**事务只能读取已经提交的数据，可以避免脏读问题，但可能引发不可重复读和幻读。
* **可重复读：**在一个事务中，相同的查询将返回相同的结果集，不管其他事务对数据做了什么修改。可以避免脏读和不可重复读，但仍有幻读的问题。
* **可串行化：**最高的隔离级别，完全禁止了并发，只允许一个事务执行完毕之后才能执行另一个事务。可以避免以上所有问题，但效率较低，不适用于高并发场景。


 隔离级别的设置**（isolation）**



```
@Service
public class StudentService {

    @Autowired
    private StudentDao studentDao;

    /**
     * timeout设置事务超时时间,单位秒! 默认: -1 永不超时,不限制事务时间!
     * rollbackFor = 指定哪些异常才会回滚,默认是 RuntimeException and Error 异常方可回滚!
     * noRollbackFor = 指定哪些异常不会回滚, 默认没有指定,如果指定,应该在rollbackFor的范围内!
     * isolation = 设置事务的隔离级别,mysql默认是repeatable read!
     */
    @Transactional(readOnly = false,
                   timeout = 3,
                   rollbackFor = Exception.class,
                   noRollbackFor = FileNotFoundException.class,
                   isolation = Isolation.REPEATABLE_READ)
    public void changeInfo() throws FileNotFoundException {
        studentDao.updateAgeById(100,1);
        //主动抛出一个检查异常,测试! 发现不会回滚,因为不在rollbackFor的默认范围内!
        new FileInputStream("xxxx");
        studentDao.updateNameById("test1",1);
    }
}

```

#### 事务传播行为


* **REQUIRED：**如果存在一个事务，则支持当前事务，不存在，则创建一个新事务
* **REQUIRED\_NEW：**开启一个新事务，如果已存在事务，则挂起当前事务
* **SUPPORTS：**如果当前存在事务，则支持当前事务，不存在，则以非事务的方式执行
* **NOT\_SUPPORTED：**总是以非事务的方式执行，并挂起任何已存在的事务
* **NAVER：**总是以非事务的方式执行，若已存在事务，则抛出异常
* **NESTED：**如果一个事务已存在，则运行在一个嵌套事务中
* **MANDATORY：**如果已存在事务，则支持当前事务，如果不存在，则抛出异常



 通过XML进行配置

```

    
        
    

    
        

            

            

            
        
    

```

 通过注解实现



```
@Transactional(rollbackFor = Exception.class,propagation = Propagation.NESTED)
@Override
public void updateBook() {
    bookMapper.updateBook();
}

```

## 懒加载


  默认为false，Spring会在容器初始化时，解析XML或注解，创建配置为单例的Bean并放入map中，懒加载可以使bean在被调用时才被创建（只对单例的生效，因为多例的bean本来就是在被调用时才会创建）


 本博客参考[蓝猫机场加速器](https://dahelaoshi.com)。转载请注明出处！
