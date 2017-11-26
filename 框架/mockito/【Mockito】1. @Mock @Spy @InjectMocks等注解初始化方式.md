# 【Mockito】1. @Mock @Spy @InjectMocks等注解初始化方式
#框架/mockito

> 背景：如果我们想使用@Mock, @Spy, @InjectMocks等注解时，需要进行初始化后才能使用。那么初始化的方式有哪些呢？主要有以下三种：  

# 1. Junit的测试类上面使用@RunWith(MockitoJUnitRunner.class)注解
但如果我们使用的是spring的话，可能我们会使用到Spring的测试类注解（@RunWith(SpringJUnit4ClassRunner.class)）。这样的话，我们就没有办法使用上面的@RunWith(SpringJUnit4ClassRunner.class)。但是我们还可以使用下面的注解：

# 2. 在测试方法被调用之前，使用MockitoAnnotations.initMocks(this)初始化
如下例：
```java
@Before
public void initMocks(){
	  MockitoAnnotations.initMocks(this);
}
```

@Before 保证了在被测试的方法被调用之前，调用@Before所注解的方法。

# 3. 使用MockitoRule 如下：
比如：
```java
public class ExampleTest {
     //Creating new rule with recommended Strictness setting
     @Rule public MockitoRule rule = MockitoJUnit.rule();

     @Mock
     private List list;

     @Test
     public void shouldDoSomething() {
         list.add(100);
     }
}
```


# 4. 手动Mock对象
前面讲解的都是通过配置，然后自动初始化所有通过@Mock等注解的mock对象。 其实我们也可以通过手动调用初始化注解。

比如：

```java
import static org.mockito.Mockito.mock;
import static org.mockito.Mockito.spy;

List mockedList = mock(List.class);

List mockedList = spy(List.class);
```

