---
title: Java 基础博文Collections
tags: [Java,Collections]
---
## 面向对象  
#### 什么是面向对象  
[面向对象、面向过程](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/object-oriented-vs-procedure-oriented.md)

[面向对象的三大基本特征](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/characteristics.md)

[五大基本原则](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/principle.md)

#### 平台无关性  
[Java如何实现的平台无关性的](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/platform-independent.md)

[JVM还支持哪些语言（Kotlin、Groovy、JRuby、Jython、Scala）](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/jvm-language.md)

#### 值传递  
[值传递、引用传递](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/java-pass-by.md)

[为什么说Java中只有值传递](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/java-pass-by.md)

#### 封装、继承、多态  
[什么是多态](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/polymorphism.md)

[方法重写与重载](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/overloading-vs-overriding.md)

Java的继承与实现

[Java的继承与组合](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/inheritance-composition.md)

[构造函数与默认构造函数](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/constructor.md)

[类变量、成员变量和局部变量](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/variable.md)

[成员变量和方法作用域](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/scope.md)

## Java基础知识  
#### 基本数据类型
[7种基本数据类型：整型、浮点型、布尔型、字符型](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/basic-data-types.md)

[整型中byte、short、int、long的取值范围](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/integer-scope.md)

[什么是浮点型？](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/float.md)

[什么是单精度和双精度？](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/single-double-float.md)

[为什么不能用浮点型表示金额？](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/float-amount.md)
#### 自动拆装箱
[什么是包装类型、什么是基本类型、什么是自动拆装箱](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/boxing-unboxing.md)

[Integer的缓存机制](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/integer-cache.md)
#### String
[字符串的不可变性](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/final-string.md)

[JDK 6和JDK 7中substring的原理及区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/substring.md)

replaceFirst、replaceAll、replace区别、

[String对“+”的重载](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/string-append.md)

[字符串拼接的几种方式和区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/string-concat.md)

[String.valueOf和Integer.toString的区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/value-of-vs-to-string.md)

[switch对String的支持](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/switch-string.md)

字符串池、常量池（运行时常量池、Class常量池）、intern
#### 熟悉Java中各种关键字
[transient](https://www.cnblogs.com/chenpi/p/6185773.html)

[instanceof](https://www.runoob.com/java/method-instanceof.html)

[volatile](https://www.infoq.cn/article/java-memory-model-4/?utm_source=infoq&%253Butm_medium=related_content_link&%253Butm_campaign=relatedContent_articles_clk)

synchronized

final

static

[const](https://howtodoinjava.com/tutorials/java-keywords/const-keyword-in-java/)
#### 集合类
常用集合类的使用

[ArrayList和LinkedList和Vector的区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/arraylist-vs-linkedlist-vs-vector.md)

[SynchronizedList和Vector的区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/synchronizedlist-vector.md)

[HashMap、HashTable、ConcurrentHashMap区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/HashMap-HashTable-ConcurrentHashMap.md)

[Set和List区别？](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/set-vs-list.md)

[Set如何保证元素不重复?](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/set-repetition.md)

[Java 8中stream相关用法、](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/stream.md)

apache集合处理工具类的使用

不同版本的JDK中HashMap的实现的区别以及原因

[Collection和Collections区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/Collection-vs-Collections.md)

[Arrays.asList获得的List使用时需要注意什么](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/Arrays-asList.md)

[Enumeration和Iterator区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/Enumeration-vs-Iterator.md)

[fail-fast 和 fail-safe](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/fail-fast-vs-fail-safe.md)

[CopyOnWriteArrayList](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/CopyOnWriteArrayList.md)

[ConcurrentSkipListMap](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/ConcurrentSkipListMap.md)
#### 枚举
[枚举的用法](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-usage.md)

[枚举的实现](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-impl.md)

[枚举与单例](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-singleton.md)

Enum类

[Java枚举如何比较](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-compare.md)

[switch对枚举的支持](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-switch.md)

[枚举的序列化如何实现](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-serializable.md)

[枚举的线程安全性问题](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/enum-thread-safe.md)
#### IO
[字符流、字节流](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/byte-stream-vs-character-stream.md)

[输入流、输出流](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/input-stream-vs-output-stream.md)

[同步、异步、阻塞、非阻塞、Linux 5种IO模型](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/synchronized-vs-asynchronization.md)

[BIO、NIO和AIO的区别、三种IO的用法与原理](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/bio-vs-nio-vs-aio.md)

netty
#### 反射
[反射与工厂模式](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/reflection.md)

[反射有什么作用](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/usage-of-reflection.md)

[Class类](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/Class.md)

java.lang.reflect.*
#### 动态代理
[静态代理](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/static-proxy.md)

[动态代理](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/dynamic-proxy.md)

[动态代理和反射的关系](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/dynamic-proxy-vs-reflection.md)

[动态代理的几种实现方式](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/dynamic-proxy-implementation.md)

[AOP](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/aop-vs-proxy.md)
#### 序列化
[什么是序列化与反序列化](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/serialize.md)

为什么序列化

[序列化底层原理](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/serialize-principle.md)

[序列化与单例模式](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/serialize-singleton.md)

protobuf

为什么说序列化并不安全
#### 注解
[Java注解的基本原理](https://juejin.im/post/5b45bd715188251b3a1db54f)

[元注解](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/meta-annotation.md)

[自定义注解](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/custom-annotation.md)

Java中常用注解使用

注解与反射的结合

[如何自定义一个注解？](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/create-annotation.md)

[Spring常用注解](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/annotation-in-spring.md)
#### JMS (Java消息服务（Java Message Service）)
[什么是Java消息服务](https://zh.wikipedia.org/wiki/Java消息服务)

JMS消息传送模型
#### JMX
[什么是JMX](https://zh.wikipedia.org/wiki/JMX)

java.lang.management.*

javax.management.*
#### 泛型
[泛型与继承](https://segmentfault.com/q/1010000007925818)

[类型擦除](https://segmentfault.com/a/1190000003831229)

[泛型中K T V E ？ object等的含义](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/k-t-v-e.md)

[泛型各种用法](https://blog.csdn.net/s10461/article/details/53941091)

[限定通配符和非限定通配符](https://blog.csdn.net/qq172435345/article/details/79060082)

上下界限定符extends 和 super

[List和原始类型List之间的区别?](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/genericity-list.md)

[List\<?>和List之间的区别是什么?](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/genericity-list-wildcard.md)

#### 单元测试
junit

mock

mockito

内存数据库（h2）

#### 正则表达式
java.lang.util.regex.*

#### 常用的Java工具库
commons.lang

commons.\*... 

guava-libraries

netty

#### API&SPI
API

[API和SPI的关系和区别](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/api-vs-spi.md)

[如何定义SPI](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/create-spi.md)

[SPI的实现原理](https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/spi-principle.md)
#### 异常
[深入理解java异常处理机制](https://blog.csdn.net/hguisu/article/details/6155636)

异常类型、正确处理异常、自定义异常

Error和Exception

[异常链](https://waylau.gitbooks.io/essential-java/docs/exceptions-chained-exceptions.html)

try-with-resources

finally和return的执行顺序

#### 时间处理
时区、冬令时和夏令时、时间戳、Java中时间API

格林威治时间、CET,UTC,GMT,CST几种常见时间的含义和关系

[SimpleDateFormat的线程安全性问题](https://www.jianshu.com/p/d9977a048dab)

Java 8中的时间处理

如何在东八区的计算机上获取美国时间

#### 编码方式
[Unicode、有了Unicode为啥还需要UTF-8](https://www.zhihu.com/question/23374078)

GBK、GB2312、GB18030之间的区别

UTF8、UTF16、UTF32区别

URL编解码、Big Endian和Little Endian

如何解决乱码问题

#### 语法糖
[Java语法糖详解](https://blog.csdn.net/gitchat/article/details/79294904)

Java中语法糖原理、解语法糖

语法糖：switch 支持 String 与枚举、泛型、自动装箱与拆箱、方法变长参数、枚举、内部类、条件编译、 断言、数值字面量、
for-each、try-with-resource、Lambda表达式、
## 阅读源代码  
[String](https://www.hollischuang.com/archives/99)

[Integer](https://www.hollischuang.com/archives/1058)

[BigInteger](https://www.hollischuang.com/archives/176)

[Long](https://juejin.im/post/59c07d795188253da713c710)

[Enum](https://www.hollischuang.com/archives/92)

[BigDecimal](https://blog.csdn.net/jackiehff/article/details/8582449)

[ClassLoader && URLClassLoader](https://www.hollischuang.com/archives/199)

[ArrayList](https://blog.csdn.net/zxt0601/article/details/77281231)

[LinkedList](https://blog.csdn.net/zxt0601/article/details/77341098)

[HashMap](https://blog.csdn.net/zxt0601/article/details/77413921)

[LinkedHashMap](https://blog.csdn.net/zxt0601/article/details/77429150)

[TreeMap](https://www.jianshu.com/p/fc5e16b5c674)

[ConcurrentHashMap](https://juejin.im/entry/59fc786d518825297f3fa968)

[HashSet](https://www.jianshu.com/p/1f7a8dda341b)

[LinkedHashSet](http://wiki.jikexueyuan.com/project/java-collection/linkedhashset.html)

[TreeSet](https://wiki.jikexueyuan.com/project/java-enhancement/java-twentyeight.html)
## Java并发编程  
#### 并发与并行
什么是并发

什么是并行

[并发与并行的区别](https://www.zhihu.com/question/33515481/answer/199929767)
#### 线程
[线程的实现](https://www.zhihu.com/question/263955521/answer/296521081)

[线程的状态](https://blog.csdn.net/pange1991/article/details/53860651)

优先级,线程调度

[创建线程的多种方式](https://zhuanlan.zhihu.com/p/48415513)

[守护线程](https://zhuanlan.zhihu.com/p/28049750)

[线程与进程的区别](https://www.liaoxuefeng.com/wiki/1016959663602400/1017627212385376)
#### 线程池
自己设计线程池

[submit()和execute()](https://www.cnblogs.com/handsomeye/p/6225033.html)

[线程池原理](https://juejin.im/post/5aeec0106fb9a07ab379574f)

为什么不允许使用Executors创建线程
#### 线程安全
[死锁](https://juejin.im/post/59e9ac446fb9a045167c520a)

死锁如何排查

线程安全和内存模型的关系
#### 锁
[CAS](https://www.hollischuang.com/archives/1537)

[乐观锁与悲观锁](https://www.hollischuang.com/archives/934)

[数据库相关锁机制](https://blog.csdn.net/C_J33/article/details/79487941)

[分布式锁](https://www.hollischuang.com/archives/1716)

[偏向锁](https://www.cnblogs.com/wade-luffy/p/5969418.html)

轻量级锁

重量级锁

[monitor](https://www.hollischuang.com/archives/2030)

[锁优化](https://juejin.im/entry/5ae1e7a5f265da0ba351c2c0)

[锁消除](https://blog.csdn.net/qq_26222859/article/details/80546917)

锁粗化

[自旋锁](https://segmentfault.com/a/1190000015795906)

可重入锁

阻塞锁

#### 死锁
死锁的原因

死锁的解决办法

#### synchronized

[synchronized是如何实现的？](https://www.hollischuang.com/archives/1883)

synchronized和lock之间关系、不使用synchronized如何实现一个线程安全的单例

synchronized和原子性、可见性和有序性之间的关系
#### volatile
happens-before、内存屏障、编译器指令重排和CPU指令重排

volatile的实现原理

volatile和原子性、可见性和有序性之间的关系

有了symchronized为什么还需要volatile

#### 线程相关方法
[sleep 和 wait](https://blog.csdn.net/eff666/article/details/53559201)

wait 和 notify

notify 和 notifyAll

写一个死锁程序

写代码来解决生产者消费者问题
#### 阅读源码
[Thread](https://juejin.im/post/5ad743f7f265da50463e3715)

[Runnable](https://my.oschina.net/mengyuankan/blog/2249979)

[Callable](http://blueskykong.com/2017/05/26/callable-future-analyze/)

[ReentrantLock](https://www.cnblogs.com/leesf456/p/5383609.html)

[ReentrantReadWriteLock](https://juejin.im/post/5b7d659c6fb9a019fc76dfba)

Atomic*

[Semaphore](https://www.cnblogs.com/leesf456/p/5414778.html)

[CountDownLatch](https://www.cnblogs.com/leesf456/p/5406191.html)

[ConcurrentHashMap](https://juejin.im/entry/59fc786d518825297f3fa968)

[Executors](https://www.hollischuang.com/archives/2888)
