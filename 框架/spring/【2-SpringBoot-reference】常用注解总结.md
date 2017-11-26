# 【2-SpringBoot-reference】常用注解总结
#框架/spring/注解

# @EnableAutoConfiguration
启动自动装载：使用了这个注解之后，所有引入的jar的starters都会被自动注入。这个类的设计就是为starter工作的。

# @RestController
这个注解专门用于写RESTful的接口的，里面集成了@Controller和@ResponseBody注解。
@ResponseBody 这个注解会自动利用默认的Jackson将return的对象序列化成json格式。

# @RequestMapping 、@GetMapping、@PostMapping
这些注解主要是配置路由信息。

# @Import、@ImportResource、@Configuration 、@PropertySources 
@Configuration ：标识当前类是一个Java配置类
@Import：在Spring4.2以前只用于手动注入Java config类。 在Spring4.2之后，支持导入普通的类并作为一个普通的bean
@ImportResource：用于注入XML配置类。
@PropertySources ：用于注入properties的配置文件。

# 查看自动加载的类：
If you need to find out what auto-configuration is currently being applied, and why, start your application with the --debug switch. This will enable debug logs for a selection of core loggers and log an auto- configuration report to the console.

# @SpringBootApplication
@SpringBootApplication注解等价于以默认属性使用 @Configuration ， @EnableAutoConfiguration 和 @ComponentScan 。

> 这里有一点需要注意：如果我们需要disable一些特殊的自动装载的类。可以使用@EnableAutoConfiguration的exclude属性，去掉自动装载的类。当然，这个属性加在@SpringBootApplication注解上也是可以的。  

# @ComponentScan 
组件扫描，如果加载Application这个类上，就不需要参数，自动扫面Application所在的路径和其下面的包下。不然需要加扫描包路径。

自动扫描：@Repository、@Service、@Controller、@Component 组件。

# @Repository、@Service、@Controller、@Component
 组件的标注：在Annotaion配置注解中用@Component来表示一个通用注释用于说明一个类是一个Spring容器管理的类。即该类已经拉入到Spring的管理中了。而@Controller, @Service, @Repository是@Component的细化，这三个注解比@Component带有更多的语义，它们分别对应了控制层、服务层、持久层的类。

@Repository注解：用于标注数据访问组件，即DAO组件
@Service注解：用于标注业务层组件
@Controller注解：用于标注控制层组件（如struts中的action）
@Component注解：泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。

# @Profile
这个注解主要加在@Component or @Configuration上面。标识当前profile环境。对于不同的环境可以可以选择是否加载这些配置。

然后在application.properties中选择环境，用逗号分隔。
spring.profiles.active=xxxx

# @ServletComponentScan
@ServletComponentScan 注解加上后拦截器失效; 去掉后过滤器和监听器失效

# @EnableCaching
 开启Cache缓存支持;

# @PathVariable:
路径变量。

# @Conditional*以及其元注解
> @ Conditional* 注解主要是用来限制auto-configuration 的应用条件  
>   
* @Conditional：满足特定条件创建一个Bean，SpringBoot就是利用这个特性进行自动配置的；
* @ConditionalOnProperty：指定的系统properties属性是否有指定的值；
* @ConditionalOnBean：当容器里面有指定的Bean条件下
* @ConditionalOnMissingBean：当容器里还没有创建执行的Bean的情况下执行
* @ConditionalOnClass：当classpath下有指定的类的条件下才加载auto-configuration
* @ConditionalOnResource：classpath下有指定的的资源文件的情况下
* @ConditionalOnWebApplication：当前项目是Web项目的条件下


# @Autowired和@Resource
@Autowired ：默认按照类型加载
@Resource：  默认按照bean的Name进行加载

不过上面两种都可以通过@qualifier 注解设置注入bean的Name

# @EnableConfigurationProperties
这个注解告诉Spring Boot 使能支持@ConfigurationProperties，如果不指定会抛出异常。

# @Value、 @ConfigurationProperties
@Value 这个注解会通过设定的key自动注入 properties文件里面配置的Property属性值。比如
@Value(“${test.name}”) 会自动引入properties文件里面配置的test.name的值。

@ConfigurationProperties: 将properties里面的key-value自动封装成实体类






.
