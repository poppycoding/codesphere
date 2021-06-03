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

> ##### <font color=green>Spring 框架蕴含的设计模式</font>

- ###### <font color=red>适配器模式</font>

Spring MVC 模块中一个典型的适配器应用: HandlerAdapter

```java
public interface HandlerAdapter {
    // ...
    
    @Nullable
    ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception;
    
    // ...
}
```

MVC 中 Controller 三种常见的定义方式:
- 通过 @Controller 注解标记类为 Controller, @RequestMapping 注解映射对应的 URL

```java
@Controller
public class HelloController {
    
    @RequestMapping("/hello")
    public ModelAndView hello() {
        
        // ...
    }
}
```

- 通过继承 HttpServlet 类标记类为 Controller, web.xml 配置映射对应的 URL

```java
public class HelloServlet extends HttpServlet {
    
    protected void service(HttpServletRequest req, HttpServletResponse resp) {

        doGet(req, resp);
        
        // ...
        
        doPost(req, resp);
    }
}
```

```xml
<web-app>
    <servlet>
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>servlet.HelloServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>HelloServlet</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```

- 通过实现 Controller 接口标记类为 Controller, web.xml 配置映射对应的 URL

```java
public class HelloController implements Controller {
    
    @Override
    public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse resp) {
        // ...
    }
}
```

MVC 简化流程: 应用启动, Spring 容器加载 Controller 类, 解析 URL 对应的方法, 封装成 Handler 对象到 HandlerMapping 中; 
当 Request 请求到 DispatcherServlet 之后, 会根据 URL 从  HandlerMapping 中获取对应的 Handler, 然后执行对应的函数,
并将结果返回给客户端. 

不同 Controller 定义的方式, 对应的是不同的 handler 方法: hello(), service(), handleRequest(), DispatcherServlet 
需要根据 Controller 的类型, 来执行对应的函数. 

```java
public class DispatcherServlet {
    protected void doDispatch(HttpServletRequest request, HttpServletResponse response) {
        // ...
        Handler handler = handlerMapping.get(URL);
        if (handler instanceof Controller) {
            ((Controller)handler).handleRequest();
        } else if (handler instanceof HttpServlet) {
            ((Servlet)handler).service();
        } else if (handler instanceof HandlerMethod) {
            // ... Invoke hello()
        } else {
            // ...
        }
    }
}
```

if else 分支判断处理不同类型逻辑, 显然不符合软件设计中的开闭原则; 因为只要增加新的 Controller 的定义, 就要在 
DispatcherServlet 中, 增加对应的 if 逻辑.

我们可以利用设计模式来消除 if else, 如, 策略模式, 适配器模式, 责任链模式等, Spring 采用的是 HandlerAdapter 适配器模式, 
适配器可以通过抽象统一的接口, 来适配多个类不同的接口; 我们将不同定义的 Controller 类中的函数, 适配成一个统一的函数,
调用统一的函数; 即使后续有新增定义, 只要实现适配器定义的统一接口即可, 这样就移除了 DispatcherServlet 中的 if-else 逻辑, 
更好地支持扩展, 也满足了满足开闭原则.

Spring 中统一的接口 HandlerAdapter, 及不同定义 Controller 的适配器类: RequestMappingHandlerAdapter, 
SimpleControllerHandlerAdapter, SimpleServletHandlerAdapter 等.

```java
public class SimpleControllerHandlerAdapter implements HandlerAdapter {
    // ...

    @Override
    @Nullable
    public ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler) {

        return ((Controller) handler).handleRequest(request, response);
    }
    
    // ...
}

public class SimpleServletHandlerAdapter implements HandlerAdapter {
    // ...

    @Override
    @Nullable
    public ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        
        ((Servlet) handler).service(request, response);
        return null;
    }

    // ...
}

// 父类 AbstractHandlerMethodAdapter#handle(), 最终逻辑是 RequestMappingHandlerAdapter#handleInternal()
public class RequestMappingHandlerAdapter extends AbstractHandlerMethodAdapter implements BeanFactoryAware, InitializingBean {
    // ...
    
    @Override
    @Nullable
    public final ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler) {

        return handleInternal(request, response, (HandlerMethod) handler);
    }
    // ...
}
```

按照适配器模式, 不需区分对待不同的 Controller, 在 DispatcherServlet 类中统一调用 HandlerAdapter#handle().

```java
public class DispatcherServlet {
    protected void doDispatch(HttpServletRequest request, HttpServletResponse response) {
        // ...

        // Determine handler for the current request.
        HandlerExecutionChain mappedHandler = getHandler(processedRequest);

        // Determine handler adapter for the current request.
        HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

        // Actually invoke the handler.
        ModelAndView mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
        
        // ...
    }
}
```

- ###### <font color=red>策略模式</font>

我们前面讲到, Spring AOP 是通过动态代理来实现的. 熟悉 Java 的同学应该知道, 具体到代码实现, Spring 支持两种动态代理实现方式, 一种是 JDK 提供的动态代理实现方式, 另一种是 Cglib 提供的动态代理实现方式. 前者需要被代理的类有抽象的接口定义, 后者不需要（这两种动态代理实现方式的更多区别请自行百度研究吧）. 针对不同的被代理类, Spring 会在运行时动态地选择不同的动态代理实现方式. 这个应用场景实际上就是策略模式的典型应用场景. 我们前面讲过, 策略模式包含三部分, 策略的定义, 创建和使用. 接下来, 我们具体看下, 这三个部分是如何体现在 Spring 源码中的. 在策略模式中, 策略的定义这一部分很简单. 我们只需要定义一个策略接口, 让不同的策略类都实现这一个策略接口. 对应到 Spring 源码, AopProxy 是策略接口, JdkDynamicAopProxy, CglibAopProxy 是两个实现了 AopProxy 接口的策略类. 其中, AopProxy 接口的定义如下所示: 
```java
public interface AopProxy {
    Object getProxy();
    Object getProxy(ClassLoader var1);
}
```

在策略模式中, 策略的创建一般通过工厂方法来实现. 对应到 Spring 源码, AopProxyFactory 是一个工厂类接口, DefaultAopProxyFactory 是一个默认的工厂类, 用来创建 AopProxy 对象. 两者的源码如下所示: 
```java

public interface AopProxyFactory {
    AopProxy createAopProxy(AdvisedSupport var1) throws AopConfigException;
}

public class DefaultAopProxyFactory implements AopProxyFactory, Serializable {
    public DefaultAopProxyFactory() {
    }

    public AopProxy createAopProxy(AdvisedSupport config) throws AopConfigException {
        if (!config.isOptimize() && !config.isProxyTargetClass() && !this.hasNoUserSuppliedProxyInterfaces(config)) {
            return new JdkDynamicAopProxy(config);
        } else {
            Class<?> targetClass = config.getTargetClass();
            if (targetClass == null) {
                throw new AopConfigException("TargetSource cannot determine target class: Either an interface or a target is required for proxy creation.");
            } else {
                return (AopProxy)(!targetClass.isInterface() && !Proxy.isProxyClass(targetClass) ? new ObjenesisCglibAopProxy(config) : new JdkDynamicAopProxy(config));
            }
        }
    }

    //用来判断用哪个动态代理实现方式
    private boolean hasNoUserSuppliedProxyInterfaces(AdvisedSupport config) {
        Class<?>[] ifcs = config.getProxiedInterfaces();
        return ifcs.length == 0 || ifcs.length == 1 && SpringProxy.class.isAssignableFrom(ifcs[0]);
    }
}
```

策略模式的典型应用场景, 一般是通过环境变量, 状态值, 计算结果等动态地决定使用哪个策略. 对应到 Spring 源码中, 我们可以参看刚刚给出的 DefaultAopProxyFactory 类中的 createAopProxy() 函数的代码实现. 其中, 第 10 行代码是动态选择哪种策略的判断条件. 

- 组合模式在 Spring 中的应用

上节课讲到 Spring“再封装, 再抽象”设计思想的时候, 我们提到了 Spring Cache. Spring Cache 提供了一套抽象的 Cache 接口. 使用它我们能够统一不同缓存实现（Redis, Google Guava…）的不同的访问方式. Spring 中针对不同缓存实现的不同缓存访问类, 都依赖这个接口, 比如: EhCacheCache, GuavaCache, NoOpCache, RedisCache, JCacheCache, ConcurrentMapCache, CaffeineCache. Cache 接口的源码如下所示: 
```java

public interface Cache {
    String getName();
    Object getNativeCache();
    Cache.ValueWrapper get(Object var1);
    <T> T get(Object var1, Class<T> var2);
    <T> T get(Object var1, Callable<T> var2);
    void put(Object var1, Object var2);
    Cache.ValueWrapper putIfAbsent(Object var1, Object var2);
    void evict(Object var1);
    void clear();

    public static class ValueRetrievalException extends RuntimeException {
        private final Object key;

        public ValueRetrievalException(Object key, Callable<?> loader, Throwable ex) {
            super(String.format("Value for key '%s' could not be loaded using '%s'", key, loader), ex);
            this.key = key;
        }

        public Object getKey() {
            return this.key;
        }
    }

    public interface ValueWrapper {
        Object get();
    }
}
```
在实际的开发中, 一个项目有可能会用到多种不同的缓存, 比如既用到 Google Guava 缓存, 也用到 Redis 缓存. 除此之外, 同一个缓存实例, 也可以根据业务的不同, 分割成多个小的逻辑缓存单元（或者叫作命名空间）. 为了管理多个缓存, Spring 还提供了缓存管理功能. 不过, 它包含的功能很简单, 主要有这样两部分: 一个是根据缓存名字（创建 Cache 对象的时候要设置 name 属性）获取 Cache 对象；另一个是获取管理器管理的所有缓存的名字列表. 对应的 Spring 源码如下所示: 
```java

public interface CacheManager {
  Cache getCache(String var1);
  Collection<String> getCacheNames();
}
```

刚刚给出的是 CacheManager 接口的定义, 那如何来实现这两个接口呢？实际上, 这就要用到了我们之前讲过的组合模式. 我们前面讲过, 组合模式主要应用在能表示成树形结构的一组数据上. 树中的结点分为叶子节点和中间节点两类. 对应到 Spring 源码, EhCacheManager, SimpleCacheManager, NoOpCacheManager, RedisCacheManager 等表示叶子节点, CompositeCacheManager 表示中间节点. 叶子节点包含的是它所管理的 Cache 对象, 中间节点包含的是其他 CacheManager 管理器, 既可以是 CompositeCacheManager, 也可以是具体的管理器, 比如 EhCacheManager, RedisManager 等. 我把 CompositeCacheManger 的代码贴到了下面, 你可以结合着讲解一块看下. 其中, getCache(), getCacheNames() 两个函数的实现都用到了递归. 这正是树形结构最能发挥优势的地方. 
```java

public class CompositeCacheManager implements CacheManager, InitializingBean {
  private final List<CacheManager> cacheManagers = new ArrayList();
  private boolean fallbackToNoOpCache = false;

  public CompositeCacheManager() {
  }

  public CompositeCacheManager(CacheManager... cacheManagers) {
    this.setCacheManagers(Arrays.asList(cacheManagers));
  }

  public void setCacheManagers(Collection<CacheManager> cacheManagers) {
    this.cacheManagers.addAll(cacheManagers);
  }

  public void setFallbackToNoOpCache(boolean fallbackToNoOpCache) {
    this.fallbackToNoOpCache = fallbackToNoOpCache;
  }

  public void afterPropertiesSet() {
    if (this.fallbackToNoOpCache) {
      this.cacheManagers.add(new NoOpCacheManager());
    }

  }

  public Cache getCache(String name) {
    Iterator var2 = this.cacheManagers.iterator();

    Cache cache;
    do {
      if (!var2.hasNext()) {
        return null;
      }

      CacheManager cacheManager = (CacheManager)var2.next();
      cache = cacheManager.getCache(name);
    } while(cache == null);

    return cache;
  }

  public Collection<String> getCacheNames() {
    Set<String> names = new LinkedHashSet();
    Iterator var2 = this.cacheManagers.iterator();

    while(var2.hasNext()) {
      CacheManager manager = (CacheManager)var2.next();
      names.addAll(manager.getCacheNames());
    }

    return Collections.unmodifiableSet(names);
  }
}
```

- 装饰器模式在 Spring 中的应用

我们知道, 缓存一般都是配合数据库来使用的. 如果写缓存成功, 但数据库事务回滚了, 那缓存中就会有脏数据. 为了解决这个问题, 我们需要将缓存的写操作和数据库的写操作, 放到同一个事务中, 要么都成功, 要么都失败. 实现这样一个功能, Spring 使用到了装饰器模式. TransactionAwareCacheDecorator 增加了对事务的支持, 在事务提交, 回滚的时候分别对 Cache 的数据进行处理. TransactionAwareCacheDecorator 实现 Cache 接口, 并且将所有的操作都委托给 targetCache 来实现, 对其中的写操作添加了事务功能. 这是典型的装饰器模式的应用场景和代码实现, 我就不多作解释了
```java

public class TransactionAwareCacheDecorator implements Cache {
  private final Cache targetCache;

  public TransactionAwareCacheDecorator(Cache targetCache) {
    Assert.notNull(targetCache, "Target Cache must not be null");
    this.targetCache = targetCache;
  }

  public Cache getTargetCache() {
    return this.targetCache;
  }

  public String getName() {
    return this.targetCache.getName();
  }

  public Object getNativeCache() {
    return this.targetCache.getNativeCache();
  }

  public ValueWrapper get(Object key) {
    return this.targetCache.get(key);
  }

  public <T> T get(Object key, Class<T> type) {
    return this.targetCache.get(key, type);
  }

  public <T> T get(Object key, Callable<T> valueLoader) {
    return this.targetCache.get(key, valueLoader);
  }

  public void put(final Object key, final Object value) {
    if (TransactionSynchronizationManager.isSynchronizationActive()) {
      TransactionSynchronizationManager.registerSynchronization(new TransactionSynchronizationAdapter() {
        public void afterCommit() {
          TransactionAwareCacheDecorator.this.targetCache.put(key, value);
        }
      });
    } else {
      this.targetCache.put(key, value);
    }
  }
  
  public ValueWrapper putIfAbsent(Object key, Object value) {
    return this.targetCache.putIfAbsent(key, value);
  }

  public void evict(final Object key) {
    if (TransactionSynchronizationManager.isSynchronizationActive()) {
      TransactionSynchronizationManager.registerSynchronization(new TransactionSynchronizationAdapter() {
        public void afterCommit() {
          TransactionAwareCacheDecorator.this.targetCache.evict(key);
        }
      });
    } else {
      this.targetCache.evict(key);
    }

  }

  public void clear() {
    if (TransactionSynchronizationManager.isSynchronizationActive()) {
      TransactionSynchronizationManager.registerSynchronization(new TransactionSynchronizationAdapter() {
        public void afterCommit() {
          TransactionAwareCacheDecorator.this.targetCache.clear();
        }
      });
    } else {
      this.targetCache.clear();
    }
  }
}
```

- 工厂模式在 Spring 中的应用

在 Spring 中, 工厂模式最经典的应用莫过于实现 IOC 容器, 对应的 Spring 源码主要是 BeanFactory 类和 ApplicationContext 相关类（AbstractApplicationContext, ClassPathXmlApplicationContext, FileSystemXmlApplicationContext…）. 除此之外, 在理论部分, 我还带你手把手实现了一个简单的 IOC 容器. 你可以回过头去再看下. 在 Spring 中, 创建 Bean 的方式有很多种, 比如前面提到的纯构造函数, 无参构造函数加 setter 方法. 我写了一个例子来说明这两种创建方式, 代码如下所示: 
```text

public class Student {
  private long id;
  private String name;
  
  public Student(long id, String name) {
    this.id = id;
    this.name = name;
  }
  
  public void setId(long id) {
    this.id = id;
  }
  
  public void setName(String name) {
    this.name = name;
  }
}

// 使用构造函数来创建Bean
<bean id="student" class="com.xzg.cd.Student">
    <constructor-arg name="id" value="1"/>
    <constructor-arg name="name" value="wangzheng"/>
</bean>

// 使用无参构造函数+setter方法来创建Bean
<bean id="student" class="com.xzg.cd.Student">
    <property name="id" value="1"></property>
    <property name="name" value="wangzheng"></property>
</bean>
```

实际上, 除了这两种创建 Bean 的方式之外, 我们还可以通过工厂方法来创建 Bean. 还是刚刚这个例子, 用这种方式来创建 Bean 的话就是下面这个样子: 
```text

public class StudentFactory {
  private static Map<Long, Student> students = new HashMap<>();
  
  static{
    map.put(1, new Student(1,"wang"));
    map.put(2, new Student(2,"zheng"));
    map.put(3, new Student(3,"xzg"));
  }
 
  public static Student getStudent(long id){
    return students.get(id);
  }
}

// 通过工厂方法getStudent(2)来创建BeanId="zheng""的Bean
<bean id="zheng" class="com.xzg.cd.StudentFactory" factory-method="getStudent">
    <constructor-arg value="2"></constructor-arg>           
</bean>
```
- 其他模式在 Spring 中的应用
  前面的几个模式在 Spring 中的应用讲解的都比较详细, 接下来的几个模式, 大部分都是我们之前讲过的, 这里只是简单总结一下, 点到为止, 如果你对哪块有遗忘, 可以回过头去看下理论部分的讲解. SpEL, 全称叫 Spring Expression Language, 是 Spring 中常用来编写配置的表达式语言. 它定义了一系列的语法规则. 我们只要按照这些语法规则来编写表达式, Spring 就能解析出表达式的含义. 实际上, 这就是我们前面讲到的解释器模式的典型应用场景. 因为解释器模式没有一个非常固定的代码实现结构, 而且 Spring 中 SpEL 相关的代码也比较多, 所以这里就不带你一块阅读源码了. 如果感兴趣或者项目中正好要实现类似的功能的时候, 你可以再去阅读, 借鉴它的代码实现. 代码主要集中在 spring-expresssion 这个模块下面. 前面讲到单例模式的时候, 我提到过, 单例模式有很多弊端, 比如单元测试不友好等. 应对策略就是通过 IOC 容器来管理对象, 通过 IOC 容器来实现对象的唯一性的控制. 实际上, 这样实现的单例并非真正的单例, 它的唯一性的作用范围仅仅在同一个 IOC 容器内. 除此之外, Spring 还用到了观察者模式, 模板模式, 职责链模式, 代理模式. 其中, 观察者模式, 模板模式在上一节课已经详细讲过了. 实际上, 在 Spring 中, 只要后缀带有 Template 的类, 基本上都是模板类, 而且大部分都是用 Callback 回调来实现的, 比如 JdbcTemplate, RedisTemplate 等. 剩下的两个模式在 Spring 中的应用应该人尽皆知了. 职责链模式在 Spring 中的应用是拦截器（Interceptor）, 代理模式经典应用是 AOP
  

> ##### <font color=green>Spring 框架的扩展实现</font>

可扩展也是大部分框架应该具备的一个重要特性. 所谓的框架可扩展, 我们之前也提到过, 意思就是, 框架使用者在不修改框架源码的情况下, 基于扩展点定制扩展新的功能. 前面在理论部分, 我们也讲到, 常用来实现扩展特性的设计模式有: 观察者模式, 模板模式, 职责链模式, 策略模式等. 今天, 我们再剖析 Spring 框架为了支持可扩展特性用的 2 种设计模式: 观察者模式和模板模式. 

- 观察者模式在 Spring 中的应用

在前面我们讲到, Java, Google Guava 都提供了观察者模式的实现框架. Java 提供的框架比较简单, 只包含 java.util.Observable 和 java.util.Observer 两个类. Google Guava 提供的框架功能比较完善和强大: 通过 EventBus 事件总线来实现观察者模式. 实际上, Spring 也提供了观察者模式的实现框架. 今天, 我们就再来讲一讲它. Spring 中实现的观察者模式包含三部分: Event 事件（相当于消息）, Listener 监听者（相当于观察者）, Publisher 发送者（相当于被观察者）. 我们通过一个例子来看下, Spring 提供的观察者模式是怎么使用的. 代码如下所示: 
```java

// Event事件
public class DemoEvent extends ApplicationEvent {
  private String message;

  public DemoEvent(Object source, String message) {
    super(source);
  }

  public String getMessage() {
    return this.message;
  }
}

// Listener监听者
@Component
public class DemoListener implements ApplicationListener<DemoEvent> {
  @Override
  public void onApplicationEvent(DemoEvent demoEvent) {
    String message = demoEvent.getMessage();
    System.out.println(message);
  }
}

// Publisher发送者
@Component
public class DemoPublisher {
  @Autowired
  private ApplicationContext applicationContext;

  public void publishEvent(DemoEvent demoEvent) {
    this.applicationContext.publishEvent(demoEvent);
  }
}
```

从代码中, 我们可以看出, 框架使用起来并不复杂, 主要包含三部分工作: 定义一个继承 ApplicationEvent 的事件（DemoEvent）；定义一个实现了 ApplicationListener 的监听器（DemoListener）；定义一个发送者（DemoPublisher）, 发送者调用 ApplicationContext 来发送事件消息. 其中, ApplicationEvent 和 ApplicationListener 的代码实现都非常简单, 内部并不包含太多属性和方法. 实际上, 它们最大的作用是做类型标识之用（继承自 ApplicationEvent 的类是事件, 实现 ApplicationListener 的类是监听器）. 
```java

public abstract class ApplicationEvent extends EventObject {
  private static final long serialVersionUID = 7099057708183571937L;
  private final long timestamp = System.currentTimeMillis();

  public ApplicationEvent(Object source) {
    super(source);
  }

  public final long getTimestamp() {
    return this.timestamp;
  }
}

public class EventObject implements java.io.Serializable {
    private static final long serialVersionUID = 5516075349620653480L;
    protected transient Object  source;

    public EventObject(Object source) {
        if (source == null)
            throw new IllegalArgumentException("null source");
        this.source = source;
    }

    public Object getSource() {
        return source;
    }

    public String toString() {
        return getClass().getName() + "[source=" + source + "]";
    }
}

public interface ApplicationListener<E extends ApplicationEvent> extends EventListener {
  void onApplicationEvent(E var1);
}
```

在前面讲到观察者模式的时候, 我们提到, 观察者需要事先注册到被观察者（JDK 的实现方式）或者事件总线（EventBus 的实现方式）中. 那在 Spring 的实现中, 观察者注册到了哪里呢？又是如何注册的呢？我想你应该猜到了, 我们把观察者注册到了 ApplicationContext 对象中. 这里的 ApplicationContext 就相当于 Google EventBus 框架中的“事件总线”. 不过, 稍微提醒一下, ApplicationContext 这个类并不只是为观察者模式服务的. 它底层依赖 BeanFactory（IOC 的主要实现类）, 提供应用启动, 运行时的上下文信息, 是访问这些信息的最顶层接口. 实际上, 具体到源码来说, ApplicationContext 只是一个接口, 具体的代码实现包含在它的实现类 AbstractApplicationContext 中. 我把跟观察者模式相关的代码, 摘抄到了下面. 你只需要关注它是如何发送事件和注册监听者就好, 其他细节不需要细究
```java

public abstract class AbstractApplicationContext  {
  private final Set<ApplicationListener<?>> applicationListeners;
  
  public AbstractApplicationContext() {
    this.applicationListeners = new LinkedHashSet();
    //...
  }
  
  public void publishEvent(ApplicationEvent event) {
    this.publishEvent(event, (ResolvableType)null);
  }

  public void publishEvent(Object event) {
    this.publishEvent(event, (ResolvableType)null);
  }

  protected void publishEvent(Object event, ResolvableType eventType) {
    //...
    Object applicationEvent;
    if (event instanceof ApplicationEvent) {
      applicationEvent = (ApplicationEvent)event;
    } else {
      applicationEvent = new PayloadApplicationEvent(this, event);
      if (eventType == null) {
        eventType = ((PayloadApplicationEvent)applicationEvent).getResolvableType();
      }
    }

    if (this.earlyApplicationEvents != null) {
      this.earlyApplicationEvents.add(applicationEvent);
    } else {
      this.getApplicationEventMulticaster().multicastEvent(
            (ApplicationEvent)applicationEvent, eventType);
    }

    if (this.parent != null) {
      if (this.parent instanceof AbstractApplicationContext) {
        ((AbstractApplicationContext)this.parent).publishEvent(event, eventType);
      } else {
        this.parent.publishEvent(event);
      }
    }
  }
  
  public void addApplicationListener(ApplicationListener<?> listener) {
    Assert.notNull(listener, "ApplicationListener must not be null");
    if (this.applicationEventMulticaster != null) {
    this.applicationEventMulticaster.addApplicationListener(listener);
    } else {
      this.applicationListeners.add(listener);
    }  
  }
  
  public Collection<ApplicationListener<?>> getApplicationListeners() {
    return this.applicationListeners;
  }
  
  protected void registerListeners() {
    Iterator var1 = this.getApplicationListeners().iterator();

    while(var1.hasNext()) {
      ApplicationListener<?> listener = (ApplicationListener)var1.next();     this.getApplicationEventMulticaster().addApplicationListener(listener);
    }

    String[] listenerBeanNames = this.getBeanNamesForType(ApplicationListener.class, true, false);
    String[] var7 = listenerBeanNames;
    int var3 = listenerBeanNames.length;

    for(int var4 = 0; var4 < var3; ++var4) {
      String listenerBeanName = var7[var4];
      this.getApplicationEventMulticaster().addApplicationListenerBean(listenerBeanName);
    }

    Set<ApplicationEvent> earlyEventsToProcess = this.earlyApplicationEvents;
    this.earlyApplicationEvents = null;
    if (earlyEventsToProcess != null) {
      Iterator var9 = earlyEventsToProcess.iterator();

      while(var9.hasNext()) {
        ApplicationEvent earlyEvent = (ApplicationEvent)var9.next();
        this.getApplicationEventMulticaster().multicastEvent(earlyEvent);
      }
    }
  }
}
```

从上面的代码中, 我们发现, 真正的消息发送, 实际上是通过 ApplicationEventMulticaster 这个类来完成的. 这个类的源码我只摘抄了最关键的一部分, 也就是 multicastEvent() 这个消息发送函数. 不过, 它的代码也并不复杂, 我就不多解释了. 这里我稍微提示一下, 它通过线程池, 支持异步非阻塞, 同步阻塞这两种类型的观察者模式. 
```text
public void multicastEvent(ApplicationEvent event) {
  this.multicastEvent(event, this.resolveDefaultEventType(event));
}

public void multicastEvent(final ApplicationEvent event, ResolvableType eventType) {
  ResolvableType type = eventType != null ? eventType : this.resolveDefaultEventType(event);
  Iterator var4 = this.getApplicationListeners(event, type).iterator();

  while(var4.hasNext()) {
    final ApplicationListener<?> listener = (ApplicationListener)var4.next();
    Executor executor = this.getTaskExecutor();
    if (executor != null) {
      executor.execute(new Runnable() {
        public void run() {
          SimpleApplicationEventMulticaster.this.invokeListener(listener, event);
        }
      });
    } else {
      this.invokeListener(listener, event);
    }
  }

}
```

借助 Spring 提供的观察者模式的骨架代码, 如果我们要在 Spring 下实现某个事件的发送和监听, 只需要做很少的工作, 定义事件, 定义监听器, 往 ApplicationContext 中发送事件就可以了, 剩下的工作都由 Spring 框架来完成. 实际上, 这也体现了 Spring 框架的扩展性, 也就是在不需要修改任何代码的情况下, 扩展新的事件和监听. 

- 模板模式在 Spring 中的应用

刚刚讲的是观察者模式在 Spring 中的应用, 现在我们再讲下模板模式. 我们来看下一下经常在面试中被问到的一个问题: 请你说下 Spring Bean 的创建过程包含哪些主要的步骤. 这其中就涉及模板模式. 它也体现了 Spring 的扩展性. 利用模板模式, Spring 能让用户定制 Bean 的创建过程. Spring Bean 的创建过程, 可以大致分为两大步: 对象的创建和对象的初始化. 对象的创建是通过反射来动态生成对象, 而不是 new 方法. 不管是哪种方式, 说白了, 总归还是调用构造函数来生成对象, 没有什么特殊的. 对象的初始化有两种实现方式. 一种是在类中自定义一个初始化函数, 并且通过配置文件, 显式地告知 Spring, 哪个函数是初始化函数. 我举了一个例子解释一下. 如下所示, 在配置文件中, 我们通过 init-method 属性来指定初始化函数. 
```text

public class DemoClass {
  //...
  
  public void initDemo() {
    //...初始化..
  }
}

// 配置: 需要通过init-method显式地指定初始化方法
<bean id="demoBean" class="com.xzg.cd.DemoClass" init-method="initDemo"></bean>
```

这种初始化方式有一个缺点, 初始化函数并不固定, 由用户随意定义, 这就需要 Spring 通过反射, 在运行时动态地调用这个初始化函数. 而反射又会影响代码执行的性能, 那有没有替代方案呢？Spring 提供了另外一个定义初始化函数的方法, 那就是让类实现 Initializingbean 接口. 这个接口包含一个固定的初始化函数定义（afterPropertiesSet() 函数）. Spring 在初始化 Bean 的时候, 可以直接通过 bean.afterPropertiesSet() 的方式, 调用 Bean 对象上的这个函数, 而不需要使用反射来调用了. 我举个例子解释一下, 代码如下所示. 
```text

public class DemoClass implements InitializingBean{
  @Override
  public void afterPropertiesSet() throws Exception {
    //...初始化...      
  }
}

// 配置: 不需要显式地指定初始化方法
<bean id="demoBean" class="com.xzg.cd.DemoClass"></bean>
```

尽管这种实现方式不会用到反射, 执行效率提高了, 但业务代码（DemoClass）跟框架代码（InitializingBean）耦合在了一起. 框架代码侵入到了业务代码中, 替换框架的成本就变高了. 所以, 我并不是太推荐这种写法. 实际上, 在 Spring 对 Bean 整个生命周期的管理中, 还有一个跟初始化相对应的过程, 那就是 Bean 的销毁过程. 我们知道, 在 Java 中, 对象的回收是通过 JVM 来自动完成的. 但是, 我们可以在将 Bean 正式交给 JVM 垃圾回收前, 执行一些销毁操作（比如关闭文件句柄等等）. 销毁过程跟初始化过程非常相似, 也有两种实现方式. 一种是通过配置 destroy-method 指定类中的销毁函数, 另一种是让类实现 DisposableBean 接口. 因为 destroy-method, DisposableBean 跟 init-method, InitializingBean 非常相似, 所以, 这部分我们就不详细讲解了, 你可以自行研究下. 实际上, Spring 针对对象的初始化过程, 还做了进一步的细化, 将它拆分成了三个小步骤: 初始化前置操作, 初始化, 初始化后置操作. 其中, 中间的初始化操作就是我们刚刚讲的那部分, 初始化的前置和后置操作, 定义在接口 BeanPostProcessor 中. BeanPostProcessor 的接口定义如下所示: 
```java

public interface BeanPostProcessor {
    Object postProcessBeforeInitialization(Object var1, String var2) throws BeansException;

    Object postProcessAfterInitialization(Object var1, String var2) throws BeansException;
}
```

我们再来看下, 如何通过 BeanPostProcessor 来定义初始化前置和后置操作？我们只需要定义一个实现了 BeanPostProcessor 接口的处理器类, 并在配置文件中像配置普通 Bean 一样去配置就可以了. Spring 中的 ApplicationContext 会自动检测在配置文件中实现了 BeanPostProcessor 接口的所有 Bean, 并把它们注册到 BeanPostProcessor 处理器列表中. 在 Spring 容器创建 Bean 的过程中, Spring 会逐一去调用这些处理器. 通过上面的分析, 我们基本上弄清楚了 Spring Bean 的整个生命周期（创建加销毁）. 针对这个过程, 我画了一张图, 你可以结合着刚刚讲解一块看下. 

![11](../../../media/geek-park/spring-family/img_11.png ':size=60%')

不过, 你可能会说, 这里哪里用到了模板模式啊？模板模式不是需要定义一个包含模板方法的抽象模板类, 以及定义子类实现模板方法吗？实际上, 这里的模板模式的实现, 并不是标准的抽象类的实现方式, 而是有点类似我们前面讲到的 Callback 回调的实现方式, 也就是将要执行的函数封装成对象（比如, 初始化方法封装成 InitializingBean 对象）, 传递给模板（BeanFactory）来执行.



