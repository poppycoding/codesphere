<center>

### <font color=red>Slf4j</font> <!-- {docsify-ignore} -->
</center>

<p align="right">
<a href="http://www.slf4j.org/legacy.html" target="_blank"> 
<font face="Arial" color="green" size="1">| origin-1 |</font>  
</a>
<a href="https://blog.csdn.net/bjchenxu/article/details/108031101" target="_blank"> 
<font face="Arial" color="green" size="1">| origin-3 | </font>   
</a>
</p>

> ##### <font color=green>Overview</font>

Simple Logging Facade for Java 是由 Ceki Gulcu 设计的一个日志抽象层. Ceki Gulcu 是 Log4j 的前作者, 
当时与 Apache 在定义日志抽象定义时意见不合, 因此离开 Apache 并且推出 Slf4j, 同时提供了具体日志实现 Logback;
目前 Spring Boot 内置日志框架就是 Slf4j + Logback.

> ##### <font color=green>Slf4j Framework</font>

对于新项目而言, 我们可以直接选择 Slf4j + Logback 来控制日志, 但是, 对于遗留项目, 我们就需要维护不同的日志框架;
因此, 为了统一这些日志框架的调用, Ceki Gulcu 又提供了一个桥接层来把这些不同的日志框架重定向到 Slf4j, 经过 Slf4j 之后,
就进入了适配层, 会自动选择 JUL, Log4j, JCL 等具体实现的日志框架. 而 Logback 本就是 Slf4j 默认实现, 
因此也不需要 Slf4j-xx 来适配.

![history](../../media/log/slf4j.png ':size=75%')

> ##### <font color=green>Slf4j Framework Bridge & Adapt</font>
 
如图所示, 可以通过不同的 jar 包来实现其他日志框架桥接到 Slf4j, 同时指定具体的日志框架作为 Sl4j 的具体实现.

因此, 最复杂的一种情况, 我们的系统中分别使用了 JCL, Log4j, JUL 三种不同的日志实现, 这时只需要引入三种对应的 jar
就可以把所有日志请求统一委托到 Slf4j 中处理.

- Bridge
  - jcl-over-slf4j: 把 Jakarta Commons Logging 委托到 Slf4j
  - log4j-over-slf4j: 把 Log4j 委托到 Slf4j
  - jul-to-slf4j: 把 JUL 委托到 Slf4j

当我们系统已经在使用 Slf4j, 但是某些特殊情况, 有需要采用指定的日志实现, 这时候我们可以通过引入下列对应的 jar
来指定 Slf4j 使用对应的日志实现.

- Adapt
  - slf4j-jcl: 把请求到 Slf4j 的日志委托到 JCL 来处理
  - slf4j-log4j12: 把请求到 Slf4j 的日志委托到 log4j 来处理
  - slf4j-jdk14: 把请求到 Slf4j 的日志委托到 JUL 来处理
    
![history](../../media/log/bridge.png)

> ##### <font color=green>Slf4j Infinite Loop</font>

在使用 Slf4j 的时候, 要注意 jar 包的循环依赖问题, 当引入了同一种日志框架的桥接和适配 jar 会造成无线循环, 
如下图: 在使用 Slf4j 的时候, 同时引入了 log4j-over-slf4j 和 slf4j-log4j12.

![history](../../media/log/cycle.png ':size=75%')

这时就会导致类似的错误:

```text
Exception in thread "main" java.lang.StackOverflowError
  at java.lang.String.hashCode(String.java:1482)
  at java.util.HashMap.get(HashMap.java:300)
  at org.slf4j.impl.JCLLoggerFactory.getLogger(JCLLoggerFactory.java:67)
  at org.slf4j.LoggerFactory.getLogger(LoggerFactory.java:249)
  at org.apache.commons.logging.impl.SLF4JLogFactory.getInstance(SLF4JLogFactory.java:155)
  at org.apache.commons.logging.LogFactory.getLog(LogFactory.java:289)
  at org.slf4j.impl.JCLLoggerFactory.getLogger(JCLLoggerFactory.java:69)
  at org.slf4j.LoggerFactory.getLogger(LoggerFactory.java:249)
  at org.apache.commons.logging.impl.SLF4JLogFactory.getInstance(SLF4JLogFactory.java:155)
  subsequent lines omitted...
```

!> **<font color=red>因此, 需要避免同一时间出现下列组合:</font>**

- jcl-over-slf4j 和 slf4j-jcl
  
- log4j-over-slf4j 和 slf4j-log4j12
  
- jul-to-slf4j 和 slf4j-jdk14

> ##### <font color=green>Slf4j Detail</font>

TODO 


[1]: https://logging.apache.org/log4j/1.2/manual.html "Log4j"
