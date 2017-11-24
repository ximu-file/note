# 常用JVM虚拟机命令行



> Sun JDK监控和故障处理命令有jps jstat jmap jhat jstack jinfo下面做一一介绍



参考： https://www.cnblogs.com/ityouknow/p/5714703.html



# ==jps==

>  JVM Process Status Tool，显示指定系统内所有的HotSpot虚拟机进程。



## 命令格式

`jps [options] [hostid]`



## option参数

|  -l  |     输出主类全名或jar路径     |
| :--: | :------------------: |
|  -q  |       只输出LVMID       |
|  -m  | 输出JVM启动时传递给main()的参数 |
|  -v  |  输出JVM启动时显示指定的JVM参数  |



## 示例：

```shell
root@louyuting:/opt/zhihu/answer# jps -l -m
2385 zhihu-crawler-1.0.0.jar
27204 sun.tools.jps.Jps -l -m
27022 zhihu-crawler-1.0.0.SNAPSHOT.jar
```



## 最常用

```
列出当前机器所有的JVM虚拟机进程
#jps -l -m
```





# ==jstat==

> jstat(JVM statistics Monitoring)是用于监视虚拟机运行时状态信息的命令，它可以显示出虚拟机进程中的类装载、内存、垃圾收集、JIT编译等运行数据。



## 命令格式

`jstat [option] LVMID [interval] [count]`



## option参数

|  option  |   操作参数    |
| :------: | :-------: |
|  LVMID   | 本地虚拟机进程ID |
| interval | 连续输出的时间间隔 |
|  count   |  连续输出的次数  |



## option 参数总览



|      Option      |              Displays…               |
| :--------------: | :----------------------------------: |
|      class       |          class loader的行为统计。          |
|     compiler     |          HotSpt JIT编译器行为统计。          |
|        gc        |              垃圾回收堆的行为统计              |
|    gccapacity    | 各个垃圾回收代容量(young,old,perm)和他们相应的空间统计。 |
|      gcutil      |              垃圾回收统计概述。               |
|     gccause      | 垃圾收集统计概述（同-gcutil），附加最近两次垃圾回收事件的原因。  |
|      gcnew       |               新生代行为统计。               |
|  gcnewcapacity   |           新生代与其相应的内存空间的统计。           |
|      gcold       |             年老代和永生代行为统计。             |
|  gcoldcapacity   |               年老代行为统计。               |
|  gcpermcapacity  |               永生代行为统计。               |
| printcompilation |            HotSpot编译方法统计。            |



## 使用实例：

###（1）-class

监视类装载、卸载数量、总空间以及耗费的时间

```
root@louyuting:/opt/zhihu/answer/history# jstat -class 32215
Loaded  Bytes  Unloaded  Bytes     Time
  2777  5213.3      198   306.3       2.17
```

- Loaded : 加载class的数量
- Bytes : class字节大小
- Unloaded : 未加载class的数量
- Bytes : 未加载class的字节大小
- Time : 加载时间



###（2）-compiler

输出JIT编译过的方法数量耗时等

```
root@louyuting:/opt/zhihu/answer/history# jstat -compiler 32215
Compiled Failed Invalid   Time   FailedType FailedMethod
    5596      0       0   647.40          0
```

- Compiled : 编译数量
- Failed : 编译失败数量
- Invalid : 无效数量
- Time : 编译耗时
- FailedType : 失败类型
- FailedMethod : 失败方法的全限定名



###（3）-gc

垃圾回收堆的行为统计，**常用命令**

```
root@louyuting:/opt/zhihu/answer/history# jstat -gc 32215
 S0C    S1C    S0U    S1U      EC       EU        OC         OU       MC     MU    CCSC   CCSU   YGC     YGCT    FGC    FGCT     GCT
6208.0 6208.0  0.0   5755.9 50112.0  36534.3   124608.0   37582.2   17152.0 16810.8 1792.0 1737.2   3889   12.942  135    11.291   24.233
```

**C即Capacity 总容量，U即Used 已使用的容量**

- S0C : survivor0区的总容量

- S1C : survivor1区的总容量

- S0U : survivor0区已使用的容量

- S1C : survivor1区已使用的容量

- EC : Eden区的总容量

- EU : Eden区已使用的容量

- OC : Old区的总容量

- OU : Old区已使用的容量

- MC 当前perm的容量 (KB)

- MU perm的使用 (KB)

- YGC : 新生代垃圾回收次数

- YGCT : 新生代垃圾回收时间

- FGC : (Full GC)老年代垃圾回收次数

- FGCT : 老年代垃圾回收时间

- GCT : 垃圾回收总消耗时间

  ​

###（4）-gcutil

同-gc，不过输出的是已使用空间占总空间的百分比

```
root@louyuting:/opt/zhihu/answer/history# jstat -gcutil 32215
  S0     S1     E      O      M     CCS    YGC     YGCT    FGC    FGCT     GCT
 90.35   0.00  10.15  33.63  98.01  96.94   3890   12.947   135   11.291   24.238
```



### （5）-gccause

垃圾收集统计概述（同-gcutil），附加最近两次垃圾回收事件的原因

```
root@louyuting:/opt/zhihu/answer/history# jstat -gccause 32215
  S0     S1     E      O      M     CCS    YGC     YGCT    FGC    FGCT     GCT    LGCC                 GCC
  0.00  71.52  79.48  47.50  98.28  97.06   4281   14.951   148   12.543   27.494 Allocation Failure   No GC
```

- LGCC：最近垃圾回收的原因
- GCC：当前垃圾回收的原因



### （6）-gcnew

统计新生代的行为

```
root@louyuting:/opt/zhihu/answer/history# jstat -gcnew 32215
 S0C    S1C    S0U    S1U   TT MTT  DSS      EC       EU     YGC     YGCT
4672.0 4672.0 3607.8    0.0  1  15 2336.0  37568.0  29874.8   4282   14.955
```

- TT：Tenuring threshold(提升阈值)
- MTT：最大的tenuring threshold
- DSS：survivor区域大小 (KB)



# ==jmap==

> jmap(JVM Memory Map)命令用于生成heap dump文件，如果不使用这个命令，还可以使用
>
> -XX:+HeapDumpOnOutOfMemoryError参数来让虚拟机出现OOM的时候·自动生成dump文件。

> jmap不仅能生成dump文件，还阔以查询finalize执行队列、Java堆和永久代的详细信息，如当前使用率、当前使用的是哪种收集器等。

##命令格式

```
jmap [option] LVMID
```

##option参数

> - dump:[live] format=b, file=filename ：生成堆转储快照
> - finalizerinfo : 显示在F-Queue队列等待Finalizer线程执行finalizer方法的对象
> - heap : 显示Java堆详细信息
> - histo : 显示堆中对象的统计信息
> - clstats : to print permanent generation statistics
> - F : 当-dump没有响应时，强制生成dump快照

## 示例

###（1）-heap

打印heap的概要信息，GC使用的算法，heap的配置及wise heap的使用情况,可以用此来判断内存目前的使用情况以及垃圾回收情况

```
root@louyuting:/opt/zhihu/answer/history# jmap -heap 32215
Attaching to process ID 32215, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.40-b25

using thread-local object allocation.
Mark Sweep Compact GC

Heap Configuration:
   MinHeapFreeRatio         = 40
   MaxHeapFreeRatio         = 70
   MaxHeapSize              = 262144000 (250.0MB)
   NewSize                  = 5570560 (5.3125MB)
   MaxNewSize               = 87359488 (83.3125MB)
   OldSize                  = 11206656 (10.6875MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
New Generation (Eden + 1 Survivor Space):
   capacity = 43253760 (41.25MB)
   used     = 3386088 (3.2292251586914062MB)
   free     = 39867672 (38.020774841308594MB)
   7.828424627130682% used
Eden Space:
   capacity = 38469632 (36.6875MB)
   used     = 1536696 (1.4655075073242188MB)
   free     = 36932936 (35.22199249267578MB)
   3.9945690148530666% used
From Space:
   capacity = 4784128 (4.5625MB)
   used     = 1849392 (1.7637176513671875MB)
   free     = 2934736 (2.7987823486328125MB)
   38.656825235445204% used
To Space:
   capacity = 4784128 (4.5625MB)
   used     = 0 (0.0MB)
   free     = 4784128 (4.5625MB)
   0.0% used
tenured generation:
   capacity = 95666176 (91.234375MB)
   used     = 54236400 (51.72386169433594MB)
   free     = 41429776 (39.51051330566406MB)
   56.693391821159445% used

9837 interned Strings occupying 886472 bytes.
```



###（2）-finalizerinfo

打印等待回收对象的信息

```
root@louyuting:/opt/zhihu/answer/history# jmap -finalizerinfo 32215
Attaching to process ID 32215, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.40-b25
Number of objects pending for finalization: 0
```



### （3）-histo

打印堆的对象统计，包括对象数、内存大小等等 （因为在dump:live前会进行full gc，如果带上live则只统计活对象，因此不加live的堆大小要大于加live堆的大小 ）

```
root@louyuting:/opt/zhihu/answer/history# jmap -histo 32215

 num     #instances         #bytes  class name
----------------------------------------------
   1:         95376       47012832  [C
   2:         38113       20091600  [B
   3:         75986        1823664  java.lang.String
   4:         13317        1132376  [I
   5:          9782         532824  [Ljava.lang.Object;
   6:          6545         358864  [Ljava.lang.String;
   7:         10545         337440  java.util.concurrent.ConcurrentHashMap$Node
   8:          8346         333840  java.util.TreeMap$Entry
   9:          8274         330960  java.util.LinkedHashMap$Entry
  10:         10257         328224  java.util.HashMap$Node
  。。。。。
  。。。。
```



仅仅打印了前10行，

`xml class name`是对象类型，说明如下：

```
B  byte
C  char
D  double
F  float
I  int
J  long
Z  boolean
[  数组，如[I表示int[]
[L+类名 其他对象
```



###（4）-clstats

打印Java堆内存的永久保存区域的类加载器的智能统计信息。对于每个类加载器而言，它的名称、活跃度、地址、父类加载器、它所加载的类的数量和大小都会被打印。此外，包含的字符串数量和大小也会被打印。

```
root@louyuting:/opt/zhihu/answer/history# jmap -clstats 32215
Attaching to process ID 32215, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.40-b25
finding class loader instances ..done.
computing per loader stat ..done.
please wait.. computing liveness.liveness analysis may be inaccurate ...
class_loader    classes bytes   parent_loader   alive?  type

<bootstrap>     1656    2933299   null          live    <internal>
0x00000000f668bd68      1       1471      null          dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f7803f80      1       1471    0x00000000f59be788      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f78045c0      1       1471    0x00000000f59be788      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f5999ad8      1125    1988331 0x00000000f59be788      dead    sun/misc/Launcher$AppClassLoader@0x000000010000f668
0x00000000f5d38b10      1       1499    0x00000000f5999ad8      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f7804048      1       1471    0x00000000f59be788      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f7804688      1       1471    0x00000000f59be788      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f5d38bd8      1       1483    0x00000000f5999ad8      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f7804110      1       1471    0x00000000f59be788      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f7804750      1       1471    0x00000000f59be788      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f59be788      133     277607    null          dead    sun/misc/Launcher$ExtClassLoader@0x000000010000fa10
0x00000000f5d38980      1       1475    0x00000000f5999ad8      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f5e080b8      1       1471    0x00000000f5999ad8      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f78041d8      1       1471    0x00000000f59be788      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f7804818      1       1471      null          dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f5d38a48      1       1485    0x00000000f5999ad8      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f5a6e678      0       0       0x00000000f5999ad8      dead    java/util/ResourceBundle$RBClassLoader@0x00000001000acd08
0x00000000f5e08180      1       881     0x00000000f5999ad8      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f78042a0      1       1471      null          dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f78048e0      1       1471    0x00000000f59be788      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f5bdf058      1       1472    0x00000000f5999ad8      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f5e08248      1       1471    0x00000000f5999ad8      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f7804368      1       1471      null          dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f78049a8      1       1471      null          dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f5f3ca18      1       1494    0x00000000f5999ad8      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f5e08310      1       1494    0x00000000f5999ad8      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f7804430      1       1471      null          dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f7804a70      1       1471      null          dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f5d38ca0      1       1473    0x00000000f5999ad8      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f78044f8      1       1471      null          dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8
0x00000000f5d38d68      1       1485    0x00000000f5999ad8      dead    sun/reflect/DelegatingClassLoader@0x0000000100009df8

total = 32      2942    5239956     N/A         alive=1, dead=31            N/A
```



### （5）-dump

常用格式

```
jmap -dump:live,format=b,file=dump.hprof pid 
```

dump.hprof这个后缀是为了后续可以直接用MAT(Memory Anlysis Tool)打开。

```
root@louyuting:/opt/zhihu/answer/history# jmap -dump:live,format=b,file=dump.hprof 32215
Dumping heap to /opt/zhihu/answer/history/dump.hprof ...
Heap dump file created
```



生成的dump文件一般用 jhat 命令分析

# jhat

> jhat(JVM Heap Analysis Tool)命令是与jmap搭配使用，用来分析jmap生成的dump，jhat内置了一个微型的HTTP/HTML服务器，生成dump的分析结果后，可以在浏览器中查看。在此要注意，一般不会直接在服务器上进行分析，因为jhat是一个耗时并且耗费硬件资源的过程，一般把服务器生成的dump文件复制到本地或其他机器上进行分析。



### 命令格式

```
jhat [dumpfile]
```



然后就可以通过浏览器分析



# ==jstack==

> jstack用于生成java虚拟机当前时刻的线程快照。 线程快照是当前java虚拟机内每一条线程正在执行的方法堆栈的集合，生成线程快照的主要目的是定位线程出现长时间停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间等待等。 线程出现停顿的时候通过jstack来查看各个线程的调用堆栈，就可以知道没有响应的线程到底在后台做什么事情，或者等待什么资源。 如果java程序崩溃生成core文件，jstack工具可以用来获得core文件的java stack和native stack的信息，从而可以轻松地知道java程序是如何崩溃和在程序何处发生问题。另外，jstack工具还可以附属到正在运行的java程序中，看到当时运行的java程序的java stack和native stack的信息, 如果现在运行的java程序呈现hung的状态，jstack是非常有用的。



## 命令格式

```
jstack [option] LVMID
```

## option参数

> - -F : 当正常输出请求不被响应时，强制输出线程堆栈
> - -l : 除堆栈外，显示关于锁的附加信息
> - -m : 如果调用到本地方法的话，可以显示C/C++的堆栈



## 实例

`jstack -l pid`



## 线程堆栈文件解析

http://www.hollischuang.com/archives/110



# jinfo

jinfo(JVM Configuration info)这个命令作用是实时查看和调整虚拟机运行参数。
之前的jps -v口令只能查看到显示指定的参数，如果想要查看未被显示指定的参数的值就要使用jinfo口令

### 命令格式

```
jinfo [option] [args] LVMID
```

### option参数

> - -flag : 输出指定args参数的值
> - -flags : 不需要args参数，输出所有JVM参数的值
> - -sysprops : 输出系统属性，等同于System.getProperties()





