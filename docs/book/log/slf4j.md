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

> ##### <font color=green>Bridge & Adapt</font>
 
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

> ##### <font color=green>Infinite Loop</font>

在使用 Slf4j 的时候, 要注意 jar 包的循环依赖问题, 当引入了同一种日志框架的桥接和适配 jar 会造成无线循环, 
如下图: 在使用 Slf4j 的时候, 同时引入了 log4j-over-slf4j 和 slf4j-log4j12.

![history](../../media/log/cycle.png ':size=75%')

这时就会导致 StackOverflowError:

```textgetILoggerFactory
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

> ##### <font color=green>Adapt Logic</font>

Slf4j 使用的时门面模式, 只提供接口而没有具体实现, 而对于不同的日志实现, Slf4j 是通过不同的适配包, 来完成实际调用,
具体适配逻辑如下:

- **Logger**

我们使用 Slf4j 的 LoggerFactory 获取 Logger 对象:
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Demo {
    private Logger logger = LoggerFactory.getLogger(Demo.class);
    
    // ... ...
}
```

- **LoggerFactory**

LoggerFactory 是 ILoggerFactory 的一个 wrapper 类, 主要功能是为不同日志框架 (log4j, logback and JDK 1.4 logging),
提供具体的绑定逻辑, 在编译时期会 Slf4j 会找到具体实现日志的 xxxLoggerFactory.
```java
public final class LoggerFactory {

    // ... ...
    
    public static Logger getLogger(Class<?> clazz) {
        Logger logger = getLogger(clazz.getName());
        if (DETECT_LOGGER_NAME_MISMATCH) {
            Class<?> autoComputedCallingClass = Util.getCallingClass();
            if (autoComputedCallingClass != null && nonMatchingClasses(clazz, autoComputedCallingClass)) {
                Util.report(String.format("Detected logger name mismatch. Given name: \"%s\"; computed name: \"%s\".", logger.getName(),
                        autoComputedCallingClass.getName()));
                Util.report("See " + LOGGER_NAME_MISMATCH_URL + " for an explanation");
            }
        }
        return logger;
    }

    // getILoggerFactory
    public static Logger getLogger(String name) {
        ILoggerFactory iLoggerFactory = getILoggerFactory();
        return iLoggerFactory.getLogger(name);
    }
}
```

- **GetILoggerFactory**

LoggerFactory 是关键, 这里通过双重检查锁机制保证只初始化一次, 当初始化完成之后的状态为 SUCCESSFUL, 
则通过 StaticLoggerBinder 获取到具体的 LoggerFactory.
```java
public final class LoggerFactory {

    // ... ...

    public static ILoggerFactory getILoggerFactory() {
        if (INITIALIZATION_STATE == UNINITIALIZED) {
            synchronized (LoggerFactory.class) {
                if (INITIALIZATION_STATE == UNINITIALIZED) {
                    INITIALIZATION_STATE = ONGOING_INITIALIZATION;
                    performInitialization();
                }
            }
        }
        switch (INITIALIZATION_STATE) {
            case SUCCESSFUL_INITIALIZATION:
                // getLoggerFactory
                return StaticLoggerBinder.getSingleton().getLoggerFactory();
            case NOP_FALLBACK_INITIALIZATION:
                return NOP_FALLBACK_FACTORY;
            case FAILED_INITIALIZATION:
                throw new IllegalStateException(UNSUCCESSFUL_INIT_MSG);
            case ONGOING_INITIALIZATION:
                return SUBST_FACTORY;
        }
        throw new IllegalStateException("Unreachable code");
    }

    // initial
    private final static void performInitialization() {
        bind();
        if (INITIALIZATION_STATE == SUCCESSFUL_INITIALIZATION) {
            versionSanityCheck();
        }
    }
}    
```

- **bind**

初始化最终会走到 bind 方法, 这里绑定了具体 StaticLoggerBinder, 编译时期自动寻找 classpath 下面所有符合条件的 
StaticLoggerBinder 单例类, 然后修改初始化状态为 SUCCESSFUL_INITIALIZATION.

```java
public final class LoggerFactory {

    // ... ...

    private static String STATIC_LOGGER_BINDER_PATH = "org/slf4j/impl/StaticLoggerBinder.class";

    private final static void bind() {
        try {
            Set<URL> staticLoggerBinderPathSet = null;
            // skip check under android, see also
            // http://jira.qos.ch/browse/SLF4J-328
            if (!isAndroid()) {
                staticLoggerBinderPathSet = findPossibleStaticLoggerBinderPathSet();
                reportMultipleBindingAmbiguity(staticLoggerBinderPathSet);
            }
            // the next line does the binding
            StaticLoggerBinder.getSingleton();
            INITIALIZATION_STATE = SUCCESSFUL_INITIALIZATION;
            reportActualBinding(staticLoggerBinderPathSet);
            fixSubstituteLoggers();
            replayEvents();
            // release all resources in SUBST_FACTORY
            SUBST_FACTORY.clear();
        } catch (Exception e) {
            
            // ... ...
            
            failedBinding(e);
            throw new IllegalStateException("Unexpected initialization failure", e);
        }
   }
}
```

这里额外关注几个方法:

- findPossibleStaticLoggerBinderPathSet: 通过classloader 寻找到所有 StaticLoggerBinder.class 文件
```java
public final class LoggerFactory {

    // ... ...
    
    static Set<URL> findPossibleStaticLoggerBinderPathSet() {
        // use Set instead of list in order to deal with bug #138
        // LinkedHashSet appropriate here because it preserves insertion order
        // during iteration
        Set<URL> staticLoggerBinderPathSet = new LinkedHashSet<URL>();
        try {
            ClassLoader loggerFactoryClassLoader = LoggerFactory.class.getClassLoader();
            Enumeration<URL> paths;
            if (loggerFactoryClassLoader == null) {
                paths = ClassLoader.getSystemResources(STATIC_LOGGER_BINDER_PATH);
            } else {
                paths = loggerFactoryClassLoader.getResources(STATIC_LOGGER_BINDER_PATH);
            }
            while (paths.hasMoreElements()) {
                URL path = paths.nextElement();
                staticLoggerBinderPathSet.add(path);
            }
        } catch (IOException ioe) {
            Util.report("Error getting resources from path", ioe);
        }
        return staticLoggerBinderPathSet;
    }
}
```

- reportMultipleBindingAmbiguity: 当发现多个 StaticLoggerBinder 时候打印错误日志.
```java
public final class LoggerFactory {

    // ... ...

    private static void reportMultipleBindingAmbiguity(Set<URL> binderPathSet) {
        if (isAmbiguousStaticLoggerBinderPathSet(binderPathSet)) {
            Util.report("Class path contains multiple SLF4J bindings.");
            for (URL path : binderPathSet) {
                Util.report("Found binding in [" + path + "]");
            }
            Util.report("See " + MULTIPLE_BINDINGS_URL + " for an explanation.");
        }
    }
}
```

- reportActualBinding: 打印最终绑定的 StaticLoggerBinder 信息.
```java
public final class LoggerFactory {

    // ... ...

    private static void reportActualBinding(Set<URL> binderPathSet) {
        // binderPathSet can be null under Android
        if (binderPathSet != null && isAmbiguousStaticLoggerBinderPathSet(binderPathSet)) {
            Util.report("Actual binding is of type [" + StaticLoggerBinder.getSingleton().getLoggerFactoryClassStr() + "]");
        }
    }
}
```

!> 当 pom 依赖中引入多个日志实现:
```xml
<!-- ... -->
<dependencies>
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
    </dependency>

    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
    </dependency>

    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-jdk14</artifactId>
    </dependency>
</dependencies>
<!-- ... -->
```

!> 启动应用时 SLF4J 会打印错误日志 multiple SLF4J bindings 以及 Actual binding:
```text
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/C:/Domain/Maven/repository/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/C:/Domain/Maven/repository/org/slf4j/slf4j-log4j12/1.7.30/slf4j-log4j12-1.7.30.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/C:/Domain/Maven/repository/org/slf4j/slf4j-jdk14/1.7.30/slf4j-jdk14-1.7.30.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [ch.qos.logback.classic.util.ContextSelectorStaticBinder]
```





[1]: https://logging.apache.org/log4j/1.2/manual.html "Log4j"
