<center>

### <font color=red>Java Logging System</font> <!-- {docsify-ignore} -->
</center>

<p align="right">
<a href="https://www.loggly.com/ultimate-guide/java-logging-basics/" target="_blank"> 
<font face="Arial" color="green" size="1">| origin-1 |</font>  
</a>
<a href="https://segmentfault.com/a/1190000038491835" target="_blank"> 
<font face="Arial" color="green" size="1">| origin-2 | </font>   
</a>
<a href="https://blog.csdn.net/bjchenxu/article/details/108031101" target="_blank"> 
<font face="Arial" color="green" size="1">| origin-3 | </font>   
</a>
</p>

> ##### <font color=green>Overview</font>
 
对于程序的运行以及调试, 日志记录是不可或缺的一个环节; 日志记录了程序的状态数据, 有助于我们在程序的不同阶段做出相应的处理, 如: 

- 开发调试阶段: 日志信息有助于我们高效定位问题点
- 应用运维阶段: 日志系统帮助我们实时监控系统, 记录异常信息及时告警
- 数据分析阶段: 通过日志系统收集的海量数据, 可以对用户的行为做出针对性分析, 一定程度帮助系统业务改善以及架构的演化

> ##### <font color=green>Java Logging History</font>

Java 日志系统看起来还是很混乱, 因为早期没有一个固定的标准, 导致多个不同日志框架的诞生, 简单来看框架演变过程如下: 
![history](../../media/log/history.png ':size=65%')

- [Log4j][1]

Apache Log4j 是由 Ceki Gulcu 创建的一种基于 Java 的日志记录工具, 改变了通过 system.out 记录日志的传统方法.

- [JUL][2]

Sun 公司拒绝把 log4j 纳入 jdk, 并且在 jdk 1.4 中引入了类似的日志工具 JUL (java.util.logging).

- [Commons Logging][3]

为了日志接口与实现的解耦, Apache 推出了 JCL (Jakarta Commons Logging), 它定义了一套日志接口, Log4j / JUL 
可以提供实现. JCL 基于动态绑定来寻找具体实现的日志框架, 在使用时只需要用它定义的接口编码, 程序运行时通过 ClassLoader
动态加载具体的实现; 因此,可以灵活选择 log4j 或 JUL.

- [Slf4j][4] / [Logback][5]

Slf4j 是一个日志门面, 只提供接口, 具体实现由 Logback 完成, 同样支持 JUL, Log4j, Log4j 2 等日志实现. 
Ceki Gulcu 与 Apache JCL 制定的标准存在分歧; 因此, 离开 Apache 并创建了 Slf4j 和 Logback 两个项目. 这也是
为什么二者更得人心, 毕竟出自于同一个作者, 天然适配.

- [Log4j 2][6]

Apache 推出的 Log4j 2 是 Log4j 的升级版本, 因为 Slf4j 和 Logback 的流行, 为了保住自家 JCL 和 Log4j 地位, 
Apache 对 log4j 进行了大量优化 因此使用 Apache 的 Commons Logging 和 Log4j2 也是一个不错的选择.

- [Tinylog][7]

最后提一下 tinylog, 一个更加轻量级的日志框架, 不限于 java 同时支持其他 JVM 语言, 如: Kotlin, Scala. 官方页面
的 benchmark 数据显示性能卓越.

> ##### <font color=green>Java Logging Framework</font>

上述日志框架, 总体可以分为两类, 日志门面和日志实现系统.

![framework](../../media/log/framework.png ':size=65%')

- 日志门面: 只提供日志相关的接口定义, 动态 (JCL) 或者静态 (Slf4j) 地寻找具体的日志实现, 从而使接口和实现解耦, 
 可以灵活地选择具体的框架.

- 日志系统: 日志具体地实现, 包括日志级别控制, 日志打印格式, 日志输出形式 (输出到数据库, 输出到文件, 输出到控制台等).

将日志门面和日志实现分离其实是一种典型的门面模式, 可以在不改动代码的情况下, 实现日志框架的自由切换,
而开发者只需要掌握日志门面的 API.

我们可以不使用 Sl4j 这种日志门面, 仅仅使用一个具体的日志实现框架; 但是, 对于一个系统而言, 
假如不同模块使用了不同的日志框架, 如: 我们引用了 A.jar, B.jar, C.jar 分别使用了 Logback, Log4j, JUL,
那么整个项目就需要同时维护这三种日志框架, 而此时门面模式的作用就体现出来了, 日志门面会提供一个适配层, 
来统一处理这三种日志框架的调用, 这个适配层向上就会抽象出一个门面层, 整个系统只需要调用门面的接口即可.

> ##### <font color=green>Java Logging Level</font>

不同日志框架的日志级别有些许差异, 我们通常使用三种, INFO - 用于生产环境中输出程序运行的一些重要信息, 
WARN - 警告提示, ERROR - 打印错误和异常信息, 可以使用这个级别来减少日志输出量.

- JUL: 定义了 7 个日志级别, 从严重到普通依次是:
  - **SEVERE**
  - **WARNING**
  - **INFO**
  - **CONFIG**
  - **FINE**
  - **FINER**
  - **FINEST**

- Log4j: 定义了 8 个日志级别, 从严重到普通依次是:
  - **OFF**
  - **FATAL**
  - **ERROR**
  - **WARN**
  - **INFO**
  - **DEBUG**
  - **TRACE**
  - **ALL** 

- Logback: 定义了 5 个日志级别, 从严重到普通依次是:
  - **ERROR**
  - **WARN**
  - **INFO**
  - **DEBUG**
  - **TRACE**

> ##### <font color=green>Spring Logging Framework</font>

- Spring 框架默认使用 Apache JCL + Log4j 作为日志输出:

![spring](../../media/log/spring.png)

  
- SpringBoot 默认使用 Slf4j + Logback 作为日志输出:

![springboot](../../media/log/springboot.png)

> ##### <font color=green>Reference</font>

<font color=green>参考原文链接: [origin-1][8] / [origin-2][9] / [origin-3][10]</font>


[1]: https://logging.apache.org/log4j/1.2/manual.html "Log4j"
[2]: https://docs.oracle.com/en/java/javase/12/docs/api/java.logging/java/util/logging/package-summary.html "JUL"
[3]: https://commons.apache.org/proper/commons-logging/ "JCL"
[4]: http://www.slf4j.org/ "SL4J"
[5]: http://logback.qos.ch/index.html "Logback"
[6]: http://logging.apache.org/log4j/2.x/manual/index.html "Log4j2"
[7]: https://tinylog.org/v2/ "Tinylog"
[8]: https://www.loggly.com/ultimate-guide/java-logging-basics/ "8"
[9]: https://segmentfault.com/a/1190000038491835 "9"
[10]: https://blog.csdn.net/bjchenxu/article/details/108031101 "10"


















