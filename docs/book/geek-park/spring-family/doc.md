## <font color=red>Overview</font>

Java 语言中, Spring 是项目开发的必备框架. 作为世界上最流行的开发框架之一, 我们除了满足日常开发需求之外, 
其框架设计中的 **<font color=red>设计思想, 设计原则, 设计模式</font>** 等也值得深入学习.

![0](../../../media/geek-park/spring-family/img.png ':size=60%')

> ##### <font color=green>Spring 全家桶: Spring Framework, Spring Boot, Spring Cloud</font>

Spring 框架指的是 Spring Framework, 是整个 **<font color=red>Spring 生态的基石</font>**. Spring Boot, Spring Cloud 都是基于 Spring
Framework 封装整合常用的企业级功能而来.

![2](../../../media/geek-park/spring-family/img_2.png ':size=35%')

> ##### <font color=green>Spring Framework 构建企业级应用的轻量级一站式解决方案</font>

Spring Framework 初始版本由 **<font color=red>Rod Johnson</font>** 在 2002 年 10 月发布在著作中: Expert One-on-One J2EE Development without EJB
    
Spring 于 2003 年 6 月, 发布在 Apache 2.0 许可证下, 在 Rod Johnson 及 Juergen Hoeller 等人的持续努力下,
不断更新迭代, 目前 (2021.01) 已经发展到了 5.3.3 版本; 值得注意的是, Spring 4.0 版本中增加了对 Java SE8,
Java EE7, 以及 WebSocket 的支持; 而 Spring 5.0 版本中引入了 WebFlux, 致力于打造一个响应式异步 web 框架,
这也是 5.0 的重要理念 Reactive Programming 以及函数式编程.

Spring Framework 是 Spring 全家桶中的底层架构, 提供了核心基础功能 **<font color=red>IOC 和 AOP</font>**, 
还有其他如事务管理 Transactions, Spring MVC Web 框架等常用功能.

![1](../../../media/geek-park/spring-family/img_1.png ':size=60%')

Spring 作为 Java 开发框架, 拥有框架最显著的特性 **<font color=red>简化开发, 通过解耦业务和非业务功能, 
使开发人员更加关注业务开发; 框架本身尽可能的整合非业务的通用功能, 实现代码复用, 并且隐藏复杂的实现细节 (数据源, 事务管理), 从而降低开发难度, 
减少 bug (框架经过一次次迭代考验)</font>**.

Spring 框架的另一个作用就是 **<font color=red>标准化项目开发, 降低学习和维护成本, 节省开发时间</font>**; 如一个 Web 应用, 
使用 Spring MVC 框架通常按照 Controller, Service, Repository 三层结构开发, 只需要完成业务功能逻辑, 即可打包部署; 
而对于其他非业务功能, 如对象生命周期的管理, http 请求的解析映射转发, 响应的处理, 都由 MVC 来完成.

![10](../../../media/geek-park/spring-family/img_10.png ':size=50%')

> ##### <font color=green>Spring Boot 快速构建 Spring 应用程序</font>

Spring Boot 是基于 Spring Framework 开发, "Boot" 代表了其设计初衷, 快速地启动一个项目, 快速地开发一个项目, 
快速地部署一个项目. 核心特性如: auto-configuration, spring-boot-xx-starter, embedded web container等.

![8](../../../media/geek-park/spring-family/img_8.png ':size=60%')

> ##### <font color=green>Spring Cloud 服务于分布式系统, 简化微服务开发</font>

而对于构建微服务集群服务而言, 那 Spring Cloud 就是一套全面的解决方案. Spring Cloud 主要负责微服务集群的服务治理工作, 
包含很多独立的功能组件, 比如 Spring Cloud Sleuth 调用链追踪, Spring Cloud Config 配置中心等.

![9](../../../media/geek-park/spring-family/img_9.png ':size=50%')

## <font color=red>Spring Framework</font>

> ##### <font color=green>Spring 框架蕴含的设计思想</font>

Spring 框架背后有很多经典的设计思想, 在日常开发设计中有很多借鉴之处, 这也是学习 Spring 源码的价值所在, 
在使用框架及阅读代码过程中, 我们可以有意识的模仿, 品味这些源代码的实现轨迹. 

- ###### <font color=red>低侵入, 低耦合</font>

一个优秀的框架通常避免把框架本身的代码, 耦合到业务代码中, 避免对业务程序的侵入, 便于后续迭代开发更换框架等操作; 
一些长期维护的老项目, 大多数限于技术老旧, 框架侵入性高, 导致系统无法轻易迭代.

Spring 中的低侵入设计思想, 如: 仅仅通过 xml 配置, 就可以把 bean 注入到 IOC 依赖管理中, 不需要对 bean 本身做出修饰, 
保证业务 bean 没有与框架耦合; 此外, AOP 面向切面编程, 它将非业务功能以增强的方式放到切面处理, 没有直接编码到业务代码中, 
也保证了业务代码的独立性.

- ###### <font color=red>模块化, 轻量级</font>

在 Spring 之前, 主流的 Java 企业级框架是 EJB. 但是, 它架构复杂, 侵入性高, 依赖于应用服务器, 开发, 维护, 调试困难;
Rod Johnson 就是基于此现状, 开发了 Spring 初始框架 Interface21, 早期版本仅仅提供了最基本的 IOC 功能.
随着时间发展 Spring 早已发展成一个庞大的 Java 生态, 但不同于 EJB 臃肿的架构, Spring 在设计时分层分模块架构,
在不断迭代, 集成各种企业级功能时, 封装成一个个可组合模块, 这样就可以按需加载, 轻量部署运行. 

Spring Framework 每个模块负责一个独立的功能. 模块之间除了上下层的依赖关系, 几乎没有依赖和耦合. 因此, 
开发者可以按需引用功能模块, 而不会因为一个小功能, 就要把整个 Spring 框架引入到项目中.

![7](../../../media/geek-park/spring-family/img_7.png ':size=50%')

- ###### <font color=red>封装, 抽象</font>

Spring 除了提供企业级应用的常用功能模块, 还集成了很多主流的第三方中间件, 类库, 在上面封装和抽象出统一的高层接口. 
如, spring-data-redis 模块, 除了对最基础的 Redis 类库 Jedis, Lettuce 封装, 还有易用的 RedisTemplate, 
RedisRepository 支持; 而 Spring Cache 模块, 更是对 Redis, Memcached, Guava Cache, Caffeine 等不同的缓存实现做了高级抽象,
暴露出统一的 Cache 访问接口, 而不依赖具体的 Cache 实现, 开发者可以仅仅通过配置, 就实现各种 Cache 之间的切换.

再如, JdbcTemplate, Transaction, DataAccessException 等数据库模块, 也是对 JDBC 的封装和抽象进一步简化数据库编程, 
屏蔽了不同数据库的事务, 异常处理细节.

- ###### <font color=red>约定大于配置</font>

约定优于配置是一种软件设计范式, 按照约定编程可以减少软件开发人员需做决定的数量, 有效地简化常用配置. 简单来说, 
就是优先使用提供地默认值, 同时开发者可以根据需求个性化覆盖默认值. 如, 在 Spring Data JPA 模块中, 约定类名默认跟表名相同,
属性名默认跟表字段名相同等, 开发者默认使用即可, 不需要额外的配置映射关系; 还有 Spring Boot 中的自动装配实现, 
默认路径 resources/META-INF/spring.factories, 以及配置格式等, 开发者只需要按照约定配置即可实现功能. 

简化不必要的配置, 提高开发效率; 同时让开发者形成共识, 统一开发规范, 减少维护沟通成本.




