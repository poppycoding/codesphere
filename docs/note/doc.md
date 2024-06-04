<center>

![logo](../../../media/wikipedia/note/logo.svg ':size=7%')

### <font color=red>JDK Version</font>
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


<center>

![release](../../../media/wikipedia/note/release.png ':size=30%')

### <font color=red>Release Train Naming Convention</font>
</center>

<p align="right">
<a href="https://developer.aliyun.com/article/778402" target="_blank"> 
<font face="Arial" color="green" size="1">| origin-1 |</font>  
</a>
</p>

> ##### <font color=green>Agile Release Train</font>

Release Train 字面意义就是"火车发布", 就像生活中的车站发车一样, 在软件行业, 项目通常也会定时定点发布. 尤其是对
于复杂的项目, 系统规模越大, 加入的包越多, 就越有可能进入软件管理的领域里的"依赖地狱", 版本混乱导致新版本难以发
布; 而 Release Train 就是软件行业一种发布模式, 可以有效保证软件产品的持续迭代, 保证产品发布的稳定及正确性.

例如: 规定每个月的一号为发布日期, 这个时间点就作为各个团队的 deadline 做好统一的发车准备, 如果中间某一个团队
出现意外不能按点 ready, 那就及时下车, 等待下一趟车, 并通知整个项目尽早缩小影响范围.

这种发布模式都会涉及到版本的命名, 虽然命名方式不同, 但是都有一种类似递增的规律:
- IOS 14, IOS 14.1, IOS14.1.1
- macOS Mojave, macOS Sierra
- Spring Cloud Greenwich, Spring Cloud Hoxton

> ##### <font color=green>Project Module</font>

一个大的项目按照 Release Train 发布, 内部会涉及不同的模块, 而每一个模块的就是对应一个 Project Module, 
以最新版本 Spring Data 2020.0.2 版本为例, 内含有多个 Project Module 模块: 
- Spring Data JDBC 2.1.2
- Spring Data JPA 2.4.2
- Spring Data MongoDB 3.1.2
- Spring Data Redis 2.4.2

> ##### <font color=green>Semantic Versioning</font>

[Semantic Version][1] 定义了一套版本号规则的标准, 语义化版本号: X.Y.Z (主版本号.次版本号.修订号)

当主版本号更新时, 次版本号和补丁号都要归零; 次版本号更新时补丁号归零, 递增规则如下: 
- 主版本号: 当你做了不兼容的 API 修改 (改变很大)
- 次版本号: 当你做了向下兼容的功能性新增 (改变不大)
- 修订号: 当你做了向下兼容的问题修正 (bug 修复, 代码优化等)

也既是, 修复问题但不影响 API 时, 递增修订号; API 保持向下兼容的新增及修改时, 递增次版本号; 
进行不向下兼容的修改时, 递增主版本号. 版本号均从零开始, 但是当主版本号为零时 0.x.y (0.1.0), 
代表了软件处于开发初始阶段, 而 1.0.0 的版本号则代表了一个稳定的公共 API.

> ##### <font color=green>Calendar Versioning</font>

[Calendar Versioning][2] 定义了一套版本号规则的标准, 日历化版本号: MAJOR.MINOR.MICRO-[MODIFIER] (主要.次要.微小-可选修饰符)

日历化版本不是基于任意数字, 而是基于项目发布日期的版本控制约定. Versioning gets better with time!

整体意义与语义化版本号类似:
- 主要: 主要版本号, 版本中的第一个数字
- 次要: 次要版本号, 版本中的第二个数字
- 微小: 版本中的第三个且通常是最终数字
- 修饰符: 可选的文本标记,例如 "dev", "alpha", "beta", "rc1", 依此类推

与语义化版本不同的是, 因为日期格式的多种多样, 衍生出多种多样的日历化版本, 通常规则如下:
- YYYY: 年份全称 - 2006, 2016, 2106
- YY: 年份缩写 - 6, 16, 106
- 0Y: 以零填充的年份 - 06, 16, 106
- MM: 月份缩写 - 1, 2 ... 11, 12
- 0M: 以零填充的月份 - 01, 02 ... 11, 12
- DD: 日 - 1, 2 ... 30, 31
- 0D: 以零填充的日 - 01, 02 ... 30, 31

> ##### <font color=green>Spring Release Train</font>

Spring Release Train 版本号都是按照字母表顺序来发布, 如 2020 之前的版本:

[Spring Cloud][3] 伦敦地铁站: 

- Spring Cloud **<font color=red>Angel</font>**
- Spring Cloud **<font color=red>Brixton</font>**
- Spring Cloud **<font color=red>Camden</font>**
- Spring Cloud **<font color=red>Dalston</font>**
- Spring Cloud **<font color=red>Edgware</font>**
- Spring Cloud **<font color=red>Finchley</font>**
- **...**

[Spring Data][4] 人名姓氏:

- Spring Data **<font color=red>Arora</font>**  
- Spring Data **<font color=red>Babbage</font>**  
- Spring Data **<font color=red>Codd</font>**  
- Spring Data **<font color=red>Dijkstra</font>**  
- Spring Data **<font color=red>Evans</font>**  
- Spring Data **<font color=red>Fowler</font>**  
- **...**

> ##### <font color=green>Spring Release Train Change On 2020</font>

从[2020][5]开始, Spring Team 采用日历化风格的版本来控制 Release Train, 规则: YYYY.MINOR.MICRO-[MODIFIER]
- YYYY: 年份全称. 2020
- MINOR: 辅助版本号 (一般升级些非主线功能), 在当前年内从0递增
- MICRO: 补丁版本号 (一般修复些bug), 在当前年内从0递增
- MODIFIER:[非必填]. 后缀,它用于修饰一些关键节点
  - M 数字: 里程碑版本, 如2020.0.0-M1, 2020.0.0-M2
  - RC 数字: 发布候选版本, 如2020.0.0-RC1, 2020.0.0-RC2
  - SNAPSHOT: 快照版本, 如2020.0.0-SNAPSHOT
  - 未填: 正式版本 (可放心使用, 相当于之前的xxx-RELEASE), 2020.0.0
    
对于 Project Module, 仍然采用 Semantic Versioning 语义化版本, 但是做了微小变更, 规则: MAJOR.MINOR.PATCH[-MODIFIER]
<style>
table th:first-of-type {
    width: 7cm;
}
table th:nth-of-type(2) {
    width: 7cm;
}
</style>
| Before      | After     |
| :----:      | :----:    |
| 3.0.0.RC1   | 3.0.0-RC1 |

> ##### <font color=green>Reference</font>

**<font color=green face="Microsoft Sans Serif">原文链接: [origin-1][6]</font>**





[1]: https://semver.org/ "1"
[2]: https://calver.org/ "2"
[3]: https://spring.io/projects/spring-cloud#learn "3"
[4]: https://spring.io/projects/spring-data#learn "4"
[5]: https://github.com/spring-cloud/spring-cloud-release/wiki/Release-Train-Naming-Convention "5"
[6]: https://developer.aliyun.com/article/778402 "6"
