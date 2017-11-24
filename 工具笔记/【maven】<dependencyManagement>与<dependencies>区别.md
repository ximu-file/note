# 【maven】<dependencyManagement>与<dependencies>区别
#技术积累/工具/maven

> 背景：在父项目中的根结点中声明dependencyManagement和dependencies的区别。  

# dependencyManagement
通常会在一个组织或者项目的最顶层的父POM 中看到dependencyManagement 元素。使用pom.xml 中的dependencyManagement 元素能让所有在子项目中引用一个依赖而不用显式的列出版本号。Maven 会沿着父子层次向上走，直到找到一个拥有dependencyManagement 元素的项目，然后它就会使用在这个dependencyManagement 元素中指定的版本号。

比如：
```xml
<dependencyManagement>  
	<dependencies>  
		<dependency>  
			<groupId>mysql</groupId>  
			<artifactId>mysql-connector-java</artifactId>  
			<version>5.1.2</version>  
		</dependency>  
	...  
	<dependencies>  
</dependencyManagement>  
```

然后在子项目里就可以添加mysql-connector时可以不指定版本号，例如：
```xml
<dependencies>  
	<dependency>  
		<groupId>mysql</groupId>  
		<artifactId>mysql-connector-java</artifactId>  
	</dependency>  
</dependencies>  
```


这样做的好处就是：如果有多个子项目都引用同一样依赖，则可以避免在每个使用的子项目里都声明一个版本号，这样当想升级或切换到另一个版本时，只需要在顶层父容器里更新，而不需要一个一个子项目的修改 ；另外如果某个子项目需要另外的一个版本，只需要声明version就可。
 
dependencyManagement里只是声明依赖，并不实现引入，因此子项目需要显式的声明需要用的依赖。

# dependencies
相对于dependencyManagement，所有声明在dependencies里的依赖都会自动引入，并默认被所有的子项目继承。


