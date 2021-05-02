
## <font color=red><u>Overview</u></font>

> ##### <font color=green>Spring Framework 初始版本由 Rod Johnson 在 2002 年 10 月发布在著作中</font>

Expert One-on-One J2EE Design and Development

Expert One-on-One J2EE Development without ELB
    
Spring 于 2003 年 6 月, 发布在 Apache 2.0 许可证下, 在 Rod Johnson 及 Juergen Hoeller 等人的持续努力下,
不断更新迭代, 目前 (2021.01) 已经发展到了 5.3.3 版本; 值得注意的是, Spring 4.0 版本中增加了对 Java SE8,
Java EE7, 以及 WebSocket 的支持; 而 Spring 5.0 版本中引入了 WebFlux, 致力于打造一个响应式异步 web 框架,
这也是 5.0 的重要理念 Reactive Programming 以及函数式编程.

<style>
table th:first-of-type {
    width: 4cm;
}
table th:nth-of-type(2) {
    width: 4cm;
}
</style>
| Version | Date	| 
| :----:  | :----:  |
| 0.9	  | 2002    |
| 1.0	  | 2003    |
| 2.0	  | 2006    |
| 3.0	  | 2009    |
| 4.0	  | 2013    |
| 5.0	  | 2017    |

> ##### <font color=green>Spring 是当前主流的 Java 开发框架: Spring Framework, Spring Boot, Spring Cloud</font>

![1](../../../media/geek-time/spring-family/1.png ':size=60%')

> ##### <font color=green>Spring Framework 构建企业级应用的轻量级一站式解决方案</font>

> ##### <font color=green>Spring Boot 快速构建 Spring 应用程序</font>

> ##### <font color=green>Spring Cloud 服务于分布式系统, 简化微服务开发</font>


## <font color=red><u>JDBC</u></font>

> ##### <font color=green>DataAutoConfiguration 自动装配</font>
  - DataSourceAutoConfiguration
  - DataSourceTransactionManagerAutoConfiguration
  - JdbcTemplateAutoConfiguration

> ##### <font color=green>Bean Annotation 四个常用注解</font>
  - @Controller
  - @Service
  - @Repository
  - @Component

> ##### <font color=green>Spring Transaction 事务抽象</font>

| Isolation        | Value  | Dirty-Reads | Non-Repeatable-Reads | Phantom-Read   |
| :----            | :----: | :----:      | :----:               |:----:          |
|READ_UNCOMMITTED  | 1      | √           | √                    | √              |
|READ_COMMITTED    | 2      | ×           | √                    | √              |
|REPEATABLE_READ   | 3      | ×           | ×                    | √              |
|SERIALIZABLE      | 4      | ×           | ×                    | ×              |

| Propagation              | Value  |  Desc                         |
| :----                    | :----: | :----                        |
|PROPAGATION_REQUIRED      | 0      | 当前有事务就⽤当前的, 没有就⽤新的  |
|PROPAGATION_SUPPORTS      | 1      | 事务可有可⽆, 不是必须的          |
|PROPAGATION_MANDATORY     | 2      | 当前⼀定要有事务, 不然就抛异常     |
|PROPAGATION_REQUIRES_NEW  | 3      | ⽆论是否有事务, 都起个新的事务     |
|PROPAGATION_NOT_SUPPORTED | 4      | 不⽀持事务, 按⾮事务⽅式运⾏       |
|PROPAGATION_NEVER         | 5      | 不⽀持事务, 如果有事务则抛异常     |
|PROPAGATION_NESTED        | 6      | 当前有事务就在当前事务⾥再起⼀个事务|

核心接口: 
- PlatformTransactionManager
    - DataSourceTransactionManager
    - HibernateTransactionManager
    - JtaTransactionManager

- TransactionDefinition
    - Propagation
    - Isolation
    - Timeout
    - Read-only status

- 编程式事务: TransactionTemplate  
  
- 声明式事务: @Transactional

![2](../../../media/geek-time/spring-family/2.png ':size=50%')


> ##### <font color=green>Spring DataAccessException 异常抽象</font>

![3](../../../media/geek-time/spring-family/3.png ':size=50%')

- SQLErrorCodeSQLExceptionTranslator
- sql-error-codes.xml



## <font color=red><u>O/R Mapping</u></font>

ORM 框架屏蔽底层数据库细节.

- Java Persistence API
  - 简化数据库操作
  - 统一 Java 持久化 API
  
- Spring Data JPA: 封装底层特性, 对外提供一致的编程模型
  - 实体: @Entity, @MapperSuperclass, @Table
  - 主键: @Id, @GeneratedValue (strategy, generator), @SequenceGenerator (name, sequenceName)
  - 映射: @Column (name, nullable, length, insertable, updatable), @JoinTable (name), @JoinColumn (name)
  - 关系: @OneToOne, @OneToMany, @ManyToOne, @ManyToMany, @OrderBy

- Repository<T, ID>
  - CrudRepository<T, ID>
  - PagingAndSortingRepository<T, ID>
  - JpaRepository<T, ID>

- Repository 注册成 Bean 
  - JpaRepositoriesRegistrar
    - 激活了 @EnableJpaRepositories
    - 返回了 JpaRepositoryConfigExtension
  - RepositoryBeanDefinitionRegistrarSupport.registerBeanDefinitions
    - 注册 Repository Bean (类型是 JpaRepositoryFactoryBean)
  - RepositoryConfigurationExtensionSupport.getRepositoryConfigurations
    - 取得 Repository 配置
  - JpaRepositoryFactory.getTargetRepository
    - 创建了 Repository

- Repository 中的方法执行
  - RepositoryFactorySupport.getRepository 添加了 Advice
    - DefaultMethodInvokingMethodInterceptor
    - QueryExecutorMethodInterceptor
  - AbstractJpaQuery.execute 执⾏具体的查询
    - 语法解析在 Part 中


## <font color=red><u>Cache</u></font>

- Spring Data Redis
  - 底层客户端 Jedis / Lettuce
  - RedisTemplate<K, V>
    - opsForXxx()
  - RedisRepository
    - @RedisHash
    - @Id
    - @Indexed
    
- Spring Cache 为不同的缓存提供⼀层抽象
  - org.springframework.cache.Cache
  - org.springframework.cache.CacheManager 
  - @EnableCaching, @Cacheable, @CacheEvict, @CachePut, @Caching, @CacheConfig  


## <font color=red><u>Spring MVC</u></font>

- ApplicationContext

![5](../../../media/geek-time/spring-family/5.png ':size=47%')
![4](../../../media/geek-time/spring-family/4.png ':size=30%')


- DispatcherServlet
  - Controller
  - xxxResolver
    - ViewResolver
    - HandlerExceptionResolver
    - MultipartResolver
  - HandlerMapping

- @Controller / @RestController
- @RequestMapping: @GetMapping / @PostMapping @PutMapping / @DeleteMapping
  - path / method 指定映射路径与⽅法
  - params / headers 限定映射范围
  - consumes / produces 限定请求与响应格式
- @PathVariable / @RequestParam / @RequestHeader
- @RequestBody / @ResponseBody / @ResponseStatus

- Spring Validator

- JacksonAutoConfiguration
  - Spring Boot 通过 @JsonComponent 注册 JSON 序列化组件
  - Jackson2ObjectMapperBuilderCustomizer
- JacksonHttpMessageConvertersConfiguration: 增加 jackson-dataformat-xml 以⽀持 XML 序列化

- Spring MVC 异常解析
  - 核⼼接⼝
    - HandlerExceptionResolver
  - 实现类
    - SimpleMappingExceptionResolver
    - DefaultHandlerExceptionResolver
    - ResponseStatusExceptionResolver
    - ExceptionHandlerExceptionResolver
  - 处理⽅法
    - @ExceptionHandler
    - @Controller / @RestController / @ControllerAdvice / @RestControllerAdvice

- Spring MVC 拦截器
  - 核⼼接⼝
    - HandlerInterceptor
    - boolean preHandle()
    - void postHandle()
    - void afterCompletion()

  - 针对 @ResponseBody 和 ResponseEntity 的情况
    - ResponseBodyAdvice
  - 针对异步请求的接⼝
    - AsyncHandlerInterceptor
    - void afterConcurrentHandlingStarted()    

  -常规⽅法
    - WebMvcConfigurer.addInterceptors()
  - Spring Boot 中的配置
    - 创建⼀个带 @Configuration 的 WebMvcConfigurer 配置类
    - 不能带 @EnableWebMvc（想彻底⾃⼰控制 MVC 配置除外）