# Spring事务实现原理
#框架/spring/笔记



PlatformTransactionManager： 事务管理器

DataSourceTransactionObject : 代表一个事务，其也是一个connection 的Holder

TransactionDefinition：事务行为的定义特性

DefaultTransactionStatus：表示一个事物的状态


AOP 入口  TransactionProxyFactoryBean


< aop:aspect>：定义AOP通知器
< aop:advisor>：定义切面

< aop:aspect>定义切面时，只需要定义一般的bean就行，
而定义< aop:advisor>中引用的通知时，通知必须实现Advice接口。

< aop:advisor>大多用于事务管理。
< aop:aspect>大多用于日志，缓存



# SpringBoot(Spring)启动过程中的重要的接口和类
	1）ConfigurationClassParser：解析@Configuration的Java配置类










AopAutoConfiguration—->
::ProxyTransactionManagementConfiguration:: —>
Web、tomcat、SpringMVC相关的—>
ApushSenderAutoConfiguration—>
HsfConsumerAutoConfiguration—>
::TddlAutoConfiguration::—>
DataSourcePoolMetadataProvidersConfiguration—>
::DataSourceAutoConfiguration::—>
HealthIndicatorAutoConfiguration—>
HsfHealthIndicatorAutoConfiguration—>
TairAutoConfiguration—>
TairHealthIndicatorAutoConfiguration—>
TddlHealthIndicatorAutoConfiguration—>
EndpointsAutoConfiguration—>
TairCacheManagerAutoConfiguration—>
MybatisAutoConfiguration—>
PandoraInfoContributorAutoConfiguration—>
AuditAutoConfiguration—>
CacheStatisticsAutoConfiguration—>
ProjectInfoAutoConfiguration—>
InfoContributorAutoConfiguration—>
ServerPropertiesAutoConfiguration—>
HttpMessageConvertersAutoConfiguration—>
FreeMarkerAutoConfiguration—>

::DataSourceTransactionManagerAutoConfiguration::—>
JdbcTemplateAutoConfiguration—>
::TransactionAutoConfiguration::—>
HttpEncodingAutoConfiguration—>
MultipartAutoConfiguration—>
WebClientAutoConfiguration—->

DelegatingWebMvcConfiguration—>
PandoraEndPointManagementContextConfiguration—>
TddlEndPointManagementContextConfiguration—>
HsfEndPointManagementContextConfiguration—>
TairEndPointManagementContextConfiguration—>








