<center>

![logo](../../media/jdk/logo.svg ':size=7%')

### <font color=red>JDK Version</font> <!-- {docsify-ignore} -->
</center>

!> **JDK Beta (1995)**
- 1995 年发布 alpha 和 beta Java 公开版本

!> **JDK1.0 (1996)**
- Sun 公司发布 Java 1.0, 发布初期叫 Oak, 后改名为 Java

!> **JDK1.1 (1997)**
- 引入内部类
- 引入反射
- JAR 文件格式
- 引入 JDBC
- 引入 RMI

!> **J2SE 1.2 (1998) [Milestone]**
- Java 技术体系拆分为: J2SE, J2EE, J2ME
- 引入集合框架
- 引入 JIT 即时编译器
- 引入 EJB 技术
- 引入 Swing

!> **J2SE 1.3 (2000)**
- 引入 Timer API
- 默认虚拟机改为 HotSpot VM, 之前为 Classic VM
- 提升 JNDI 为平台级服务, 之前仅仅是一项扩展

!> **J2SE 1.4 (2002)**
- 引入 NIO
- 正则表达式
- 异常链
- 新增 java.util.logging 日志 API

!> **J2SE 5.0 (2004) [Milestone]**
- 泛型
- 枚举
- 注解
- 自动装拆箱
- 静态导入
- 可变参
- JUC
- For-Each 循环
- 改进了 Java 内存模型(并发编程)

!> **Java SE 6 (2006) [JVM]**
- 引入垃圾回收器 G1
- 优化锁与同步, 垃圾收集, 类加载等
- 提供动态语言支持

!> **Java SE 7 (2011) [Sun 2009 -> Oracle]**
- 钻石语法
- try-with-resources 语法糖
- 多个 catch 块用|连接
- switch 中可以使用字符串
- 64 位 JDK 的指针压缩
- 数值可加下划线
- 添加对 ARM 架构的支持
- JUC 中引入 fork join 编程框架

!> **Java SE 8 (2014) [Milestone]**
- Lambda 表达式 (闭包, 将函数当成参数传递)
- 接口中的默认方法和静态方法
- 方法引用
- 重复注解 (@Repeatable)
- 升级工具库实现: HashMap, ConCurrentHashMap, Stream, Optional
- 工具包: 类依赖分析工具 jdeps
- JVM 方面: 使用 Metaspace (JEP 122) 代替方法区持久代 (PermGen space)