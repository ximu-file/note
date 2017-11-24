# 【maven】基本命令
#技术积累/工具/maven

# 基本命令
1. 查询Maven版本   `mvn -v`
本命令用于检查maven是否安装成功。
Maven安装完成之后，在命令行输入mvn -v，若出现maven信息，则说明安装成功。

2. 编译  `maven compile`
将java源文件编译成class文件

编译测试代码 `mvn test-compile`

3. 测试项目  `mvn test`
执行test目录下的测试用例

4. 打包 `mvn package`
将项目打成jar包

5. 删除target文件夹   `mvn clean`

6. 安装   `mvn install`
将当前项目放到Maven的本地仓库中。供其他项目使用

7. `mvn -e `           显示详细错误信息. 

# 运行参数：
* -Dmaven.test.skip=true   给任何目标添加maven.test.skip 属性就能跳过测试 

* jetty:run   调用 Jetty 插件的 Run 目标在 Jetty Servlet 容器中启动 web 应用 

* mvn dependency:resolve 打印出已解决依赖的列表 

* mvn dependency:tree 打印整个依赖树 

* -DdownloadSources=true   下载源码

* -DdownloadJavadocs=true   下载JavaDoc文档


# maven运行test
## （1）跳过测试用例：
* -DskipTests=true  依然处理测试代码的resource和编译测试代码，但是不运行测试用例。

* -Dmaven.test.skip=true  跳过测试资源文件处理、测试代码编译和执行三个阶段

## （2）动态指定要执行的测试用例
使用test参数可以在命令行指定要执行的测试用例的类名。
```
mvn test -Dtest=demomavenTest         #指定单个测试执行类
mvn test -Dtest=demo*Test             #指定所有以demo开头以Test结尾的测试类
mvn test -Dtest=demo*Test,heheTest   #以逗号隔开多个指定的测试类
```


# maven的jar依赖树分析
```
mvn dependency:resolve    # 打印出已解决依赖的列表 
mvn dependency:tree       # 打印整个依赖树 
```








