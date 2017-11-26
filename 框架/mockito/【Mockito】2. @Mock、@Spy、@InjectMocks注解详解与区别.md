# 【Mockito】2. @Mock、@Spy、@InjectMocks注解详解与区别
#框架/mockito

[【Mockito】@Mock @Spy @InjectMocks等注解初始化方式](bear://x-callback-url/open-note?id=87D597B5-7387-4C17-BCBB-3BA00C60B0E8-1362-00001C6B2527F50A)    这个文章里面已经说过了怎么配置可以初始化所有通过@Mock等注解配置的Mock对象。 


# 1. @Mock 和 @InjectMocks区别
```java
@Mock:  创建一个Mock.

@InjectMocks:  创建一个实例，其余用@Mock（或@Spy）注解创建的mock将被注入到用该实例中。
```
注意：必须使用@RunWith(MockitoJUnitRunner.class) 或 Mockito.initMocks(this)进行mocks的初始化和注入。


# 2. @Mock 和 @Spy的区别
当我们对@Mock的类（@Mock private OrderDao dao;）进行模拟方法时，会像下面这样去做：
```java
when(dao.getOrder()).thenReturn("returened by mock "); // 或者使用更为推荐的given方法
```

但如果想对@Spy的类（@Spy private PriceService ps;）进行模拟方法时，需要像下面一样去做：
```
doReturn("twotwo").when(ps).getPriceTwo();
```
原因：
1）使用@Mock生成的类，所有方法都不是真实的方法，而且返回值都是NULL。
2）使用@Spy生成的类，所有方法都是真实方法，返回值都是和真实方法一样的。

所以，你用when去设置模拟返回值时，它里面的方法（dao.getOrder()）会先执行一次。
使用doReturn去设置的话，就不会产生上面的问题，因为有when来进行控制要模拟的方法，所以不会执行原来的方法。














