![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/51ddc8027255459db21753260549bd1f~tplv-k3u1fbpfcp-watermark.image)

# Java常用注解(Annotation)详解汇总

## 一，元注解（用来修饰注解的注解）

## 二，Spring中的注解

- SpringMVC注解
- IOC容器注解
- Bean的范围的注解：
- Bean的生命周期注解：
- Spring启动类注解(开箱即用)：
- 请求Mapping注解
- 动态赋值注解
- 缓存注解

## 三，AOP切面注解

## 四，常用插件注解

- Lombok 插件
- MybatisPlus 注解
- Shiro 注解

## 五，其他注解

- Async异步注解
- 注释注解
- 其他注解

先来说说什么是注解：

> 注解其实就是代码里的特殊标记，它用于替代配置文件，有了注解技术后，开发人员可以通过注解告诉类如何运行。通过元注解来定义（修饰）自定义注解并定义所需要实现的功能。

注解可以标记在包、类、属性、方法，方法参数以及局部变量上，且同一个地方可以同时标记多个注解。 在Java技术里注解的典型应用是：可以通过反射技术去得到类里面的注解，以决定怎么去运行类。

# 一，元注解（用来修饰注解的注解）

从JDK 1.5开始, Java增加了对元数据(MetaData)的支持，提供了4个标准的用来对注解类型进行注解的注解类，我们称之为 meta-annotation（元注解）
**@Target(ElementType.)** 描述注解的使用范围（即：被修饰的注解可以用在什么地方）

> 取值（ElementType）有：
> 1.CONSTRUCTOR:用于描述构造器
> 2.FIELD:用于描述域
> 3.LOCAL_VARIABLE:用于描述局部变量
> 4.METHOD:用于描述方法
> 5.PACKAGE:用于描述包
> 6.PARAMETER:用于描述参数
> 7.TYPE:用于描述类、接口（包括注解类型）或enum声明

**@Retention(RetentionPolicy.)** 描述注解的生命周期（即：被修饰的注解被保留到何时）

> 取值（RetentionPoicy）有：
> 1.SOURCE:在源文件中有效（即源文件保留）
> 2.CLASS:在class文件中有效（即class保留）
> 3.RUNTIME:在运行时有效（即运行时保留）

**@Documented** 会被javadoc工具动态提取成文档。
**@Inherited** 允许子类继承父类中的注解。

# 二，Spring中的注解

## SpringMVC注解

这些注解描述的类 Spring会创建原生对象或代理对象并交给 IOC容器 管理，这些对象称之为bean。用时直接 @Autowired 注入即可。

**@Mapper** 描述数据层 (Mapper)
**@Service** 描述业务层 (Service)
**@Repository** 标识持久层 / 数据访问层组件(Dao)
**@Component** 可以描述各种组件(当组件不好归类时)
**@RestController** 描述控制层(Controller)并返回json数据类型，但不会再执行SpringMVC中的视图解析器。
/* 该注解等同于：@Controller + @ResponseBody */
**@Controller** 描述控制层 接收用户请求 执行 视图解析器 (进行路径拼接 前缀+后缀 ViewResolver解析 view渲染)。
**@ResponseBody** 将java对象转为json格式的数据。

## IOC容器注解

IOC(Inversion of Control) 是控制反转，也叫依赖注入(DI)。
把复杂系统分解成相互合作的对象，这些对象类通过封装以后，内部实现对外部是透明的，从而降低了解决问题的复杂度，而且可以灵活地被重用和扩展。
简单来说：IOC意味着将你设计好的对象交给容器控制，需要的时候通过注解来注入（获取），而不是传统的在你的对象内部直接控制（new 对象）。从而降低了程序的耦合性。

**@Bean** 描述 方法 的返回值 交给容器管理，不需要再手动调用该方法。
**@Autowired** 注入对象（按byType自动注入）
**@Resource** 注入对象（按byName自动注入）
**@Value** 注入普通类型属性。

如果容器中有多个相同类型的 bean，则框架将抛出 NoUniqueBeanDefinitionException， 以提示有多个满足条件的
bean 进行自动装配。程序无法正确做出判断使用哪一个时，可以使用以下注解👇

**@Qualifier("")** 在相同类型bean上命名后，可以按不同名称注入 配合@Autowired 使用。
**@Primary** 当存在多个相同类型的 bean 时，首选被@Primary注解过的bean。

Bean的范围的注解：
@Scope(value="") 默认生成的类是单例的。

> 取值（value） 有：
> 1.singleton ：单例
> 2.prototype ：多例
> 3.request ：request域，需要在web环境
> 4.session ：session域，需要在web环境
> 5.application： context域，需要在web环境
> 6.globalsession 集群环境的session域，需要在web环境

Bean的生命周期注解：
@PostConstruct 相当于init-method
@PreDestroy 相当于destroy-method

## Spring启动类注解(开箱即用)：

只需要导入简单的jar包文件,就可以实现对应的功能,无需(少量)繁琐的配置。
**@SpringBootAppliction** 用在启动类上，主要目的是开启自动配置 组合了：

> 1）**@SpringBootConfiguration** 定义了SpringBoot的配置类 通过@Configuration 来定义配置信息
> 2）**@EnableAutoConfiguration** 自动化配置, 该注解里面有@AutoConfigurationPackage 自动配置的包扫描 @Import调用选择器去加载pom.xml文件中的启动项
> 3）**@ComponentScan** 定义包扫描 指定路径 哪些包中的对象交给IOC容器管理。

> 请求Mapping注解
> @RequestMapping("/xxx") 注解类上 通过"/xxx"来指定控制器可以处理哪些URL请求。

请求方式：

> @GetMapping 通常注解查询方法
> @PostMapping 通常注解增添保存方法
> @DeleteMapping 通常注解删除方法
> @PutMapping 通常注解更新方法
> @PatchMapping 通常注解更新局部方法

## 动态赋值注解

**@PathVariable** 接收的url动态传给被注解的参数（restFull风格）
**@RequestBody** 将接收的json格式的数据转为java对象参数（适用于post请求）
**@RequestParam(value=“接收的xxx”)** 讲接收的xxx传给被注解的参数 （适用于post，get请求）

## 缓存注解

**@EnableCaching** 启动springboot工程中的内置缓存。
**@Cacheable(value=“缓存值取名”)** 把返回值进行缓存，缓存通过切面自动切入，可用用于方法或者类上。

|   参数    |     描述      |
| :-------: | :-----------: |
|   value   |     名称      |
|    key    |      key      |
| condition | 可判断key条件 |

**@CacheEvict(value=“需要清空的缓存名”)** 方法是一个清缓存的切入点方法，当这个方法被调用后，即会清空缓存。

|       参数       |          描述          |
| :--------------: | :--------------------: |
|      value       |          名称          |
|       key        |          key           |
|    condition     |  缓存的条件，可以为空  |
|    allEntries    |  是否清空所有缓存内容  |
| beforeInvocation | 是否在方法执行前就清空 |

# 三，AOP切面注解

**@Pointcut("@annotation(被切入方法的地址)")** 设置切入点
**@Before(“pointCut()”)** 在切点方法前执行
**@After(“pointCut()”)** 在切点方法后执行
**@Around(“pointCut()”)** 在切点方法外环绕执行，需要执行ProceedingJoinPoint对象的proceed方法来加载需要切入的方法。

# 四，常用插件注解

## Lombok 插件

**@Data** 动态添加get/set/toString/equals/hashcode/构造方法 适用于pojo / VO该注解等同于：

@Getter + @Setter + ToString + @EqualsAndHashCode + @RequiredArgsConstructor

**@Value** 把所有的变量都设成 final 修饰 和 @Data相似
**@AllArgsConstructor** 添加构造方法
**@NoArgsConstructor** 添加无参构造
**@sfl4g** 自动生成该类的 log 静态常量
**@Accessors(chain = true)** 引用链式加载方式 方便做插入操作。

> Lombok插件 官方文档说明：

[projectlombok.org/](https://projectlombok.org/)

## MybatisPlus 注解

**MyBatis-Plus (opens new window)**（简称 MP）是一个 MyBatis (opens new window) 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

**@TableName(value="", resultMap="")** 表名与实体类名不一致时 需要在实体类上加入注解"value=表名"，xml 中 resultMap 的 id 不一致时需要赋值。
**@TableId(value= “”, type = IdType.AUTO)** 表示主键名/属性。 @IdType的值有：

> AUTO 数据库自增
> INPUT 自行输入
> ID_WORKER 分布式全局唯一ID 长整型类型
> UUID 32位UUID字符串
> NONE 无状态
> ID_WORKER_STR 分布式全局唯一ID 字符串类型

**@TableField("…")** 注解新增属性，如果字段名与属性一致(已开启驼峰规则)，则可省略，否则加入"exist=false"参数。V

|                      参数                      |                         描述                         |
| :--------------------------------------------: | :--------------------------------------------------: |
|                     value                      | 字段值，如果字段名与属性一致(已开启驼峰规则)则可省略 |
|                     update                     |              预处理 set 字段自定义注入               |
|                   condition                    |         预处理 WHERE 实体条件自定义运算规则          |
|                     exist                      |                  是否为数据库表字段                  |
|                      fill                      |                       字段填充                       |
| **@TableLogic** 表字段逻辑处理注解（逻辑删除） |                                                      |

> Mybatis-Plus官方文档说明：

[mp.baomidou.com/guide/annot…](https://mp.baomidou.com/guide/annotation.html)

## Shiro 注解

Shiro 提供了相应的注解用于权限控制，如果使用这些注解就需要使用AOP 的功能来进行判断

**@RequiresAuthentication** 表示当前Subject已经通过login 进行了身份验证，即Subject. isAuthenticated()返回true。
**@RequiresGuest** 表示当前Subject没有身份验证或通过记住我登录过，即是游客身份。
**@RequiresUser** 表示当前Subject已经身份验证或者通过记住我登录的。
**@RequiresRoles(value={“admin”, “user”}, logical= Logical.AND)** 表示当前Subject需要角色 admin 和(AND) user ，value表示需要判断的角色，logical表示判断条件。
**@RequiresPermissions(value={“user:a”, “user:b”}, logical= Logical.OR)**
表示当前Subject需要权限 user:a 或(OR) user:b ，value表示需要判断的权限，logical表示判断条件。

> Shiro框架 官方文档说明：

[shiro.apache.org/#](http://shiro.apache.org/#)

# 五，其他注解

## Async异步注解

**@Async** 注解描述的方法为一个异步切入点方法（声明该方法执行异步），启动类上需要加上@EnableAsync才能使其生效。
这个方法会在切面通知方法中通过一个新的线程调用执行,由spring线程池提供。 **@EnableAsync** 可以使用多线程 描述该类支持异步

## 注释注解

**@param** Dao层（Mapper）的注解，作用是用于传递多个参数
**@return** 说明该方法有返回值。只是起到一个说明作用

## 其他注解

不同的的业务文件放在不同的配置文件yml中，所以需要动态加载配置文件 **@PropertySource(value=“classpath:/…”,ebcidubg=“UTF-8”)** 动态加载配置文件 为了给定义的变量赋值
**@Select("…")** 简单的sql语句可以用该注解直接在方法上描述
**@CrossOrigin** 此注解描述的Controller，表示允许跨域访问