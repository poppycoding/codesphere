## <font color=red>Adapter</font>

> ##### <font color=green>适配器模式:抽象出统一的接口, 用于匹配不同类的接口</font>

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

## <font color=red>Strategy</font>

> ##### <font color=green>策略模式: 可以实现运行时切换不同算法, 适用于执行某些行为时存在许多相似类</font>

Spring 中一个典型的策略模式应用: AOP

Spring AOP 有基于实现接口类的 JDK 动态代理, 以及通过修改对象类字节码的 CGLIB 代理. 针对两种被代理类的类型, Spring
定义一个策略接口, 让两种动态代理策略实现这个接口, 然后运行时动态地选择具体的实现方式.

策略模式三个核心: 策略的定义, 策略的创建, 策略的使用;

策略的定义: AopProxy

```java
public interface AopProxy {

    /**
     * Uses the AopProxy's default class loader: Thread.getContextClassLoader().
     */
    Object getProxy();

    /**
     * Uses the given class loader.
     */
    Object getProxy(@Nullable ClassLoader classLoader);

}
```

策略的创建: AopProxyFactory

策略的创建通常是利用工厂方法实现, AopProxyFactory 工厂类接口, 默认使用 DefaultAopProxyFactory, 创建 Proxy 策略.

```java
public interface AopProxyFactory {

    /**
     * Create an AopProxy for the given AOP configuration.
     */
    AopProxy createAopProxy(AdvisedSupport config) throws AopConfigException;

}

public class DefaultAopProxyFactory implements AopProxyFactory, Serializable {

    @Override
    public AopProxy createAopProxy(AdvisedSupport config) throws AopConfigException {
        // 策略的判断条件: 通常是通过环境变量, 状态值, 逻辑计算
        if (config.isOptimize() || config.isProxyTargetClass() || hasNoUserSuppliedProxyInterfaces(config)) {
            Class<?> targetClass = config.getTargetClass();
            if (targetClass == null) {
                throw new AopConfigException("TargetSource cannot determine target class: " +
                        "Either an interface or a target is required for proxy creation.");
            }
            if (targetClass.isInterface() || Proxy.isProxyClass(targetClass)) {
                return new JdkDynamicAopProxy(config);
            }
            return new ObjenesisCglibAopProxy(config);
        }
        else {
            return new JdkDynamicAopProxy(config);
        }
    }
    
    private boolean hasNoUserSuppliedProxyInterfaces(AdvisedSupport config) {
        Class<?>[] ifcs = config.getProxiedInterfaces();
        return (ifcs.length == 0 || (ifcs.length == 1 && SpringProxy.class.isAssignableFrom(ifcs[0])));
    }

}
```

## <font color=red>Composite</font>

> ##### <font color=green>组合模式: 主要应用于树状的模型结构, 简化叶子和容器节点的处理</font>

Spring 中一个典型的组合模式应用: CompositeCacheManager

Cache 模块也是 Spring 再封装抽象设计思想的体现, Spring 提供了统一的抽象接口 Cache; 对于不同的缓存实现, 如: 
ConcurrentMapCache, EhCacheCache, RedisCache, GuavaCache, CaffeineCache 等常用的缓存, 统一使用露相同的 api, 
开发者可以任意切换, 而不需要改动访问逻辑.

```java
public interface Cache {

    /**
     * Return the cache name.
     */
    String getName();

    /**
     * Return the value to which this cache maps the specified key.
     */
    @Nullable
    ValueWrapper get(Object key);

    /**
     * Return the value to which this cache maps the specified key,
     */
    @Nullable
    <T> T get(Object key, @Nullable Class<T> type);

    // ... ...
    
    /**
     * Evict the mapping for this key from this cache if it is present.
     */
    void evict(Object key);

    // ... ...

    /**
     * A (wrapper) object representing a cache value.
     */
    @FunctionalInterface
    interface ValueWrapper {

        /**
         * Return the actual value in the cache.
         */
        @Nullable
        Object get();
    }
    
    // ... ...
}
```

对于开发中不同缓存的处理, 以及同一个缓存的分区 (业务逻辑划分) 等管理, Spring 提供了统一的 CacheManager, 
可以帮助我们获取 Cache 对象, 以及获取所有缓存的名字. 

```java
public interface CacheManager {

    /**
     * Get the cache associated with the given name.
     */
    @Nullable
    Cache getCache(String name);

    /**
     * Get a collection of the cache names known by this manager.
     */
    Collection<String> getCacheNames();

}
```

基于 CacheManager 构建树形结构, 以 EhCacheManager, SimpleCacheManager 等做叶子节点, 以 CompositeCacheManager 
做容器节点或者中间节点. 叶子节点内是具体的 Cache 对象, 而中间节点则可以嵌套 CompositeCacheManager 自身, 或者 
CacheManager 其他叶子节点, 如: EhCacheManager, CaffeineCacheManager 等. 树形结构的嵌套结构天然配合递归实现数据访问 
getCache(), getCacheNames(). 

```java
public class CompositeCacheManager implements CacheManager, InitializingBean {

    private final List<CacheManager> cacheManagers = new ArrayList<>();    

    // ... ...

    @Override
    @Nullable
    public Cache getCache(String name) {
        for (CacheManager cacheManager : this.cacheManagers) {
            Cache cache = cacheManager.getCache(name);
            if (cache != null) {
                return cache;
            }
        }
        return null;
    }

    @Override
    public Collection<String> getCacheNames() {
        Set<String> names = new LinkedHashSet<>();
        for (CacheManager manager : this.cacheManagers) {
            names.addAll(manager.getCacheNames());
        }
        return Collections.unmodifiableSet(names);
    }
}
```

## <font color=red>Decorator</font>

> ##### <font color=green>装饰器模式: 将对象放入包含行为的特殊封装对象中, 从而为对象新增额外的行为</font>

Spring 中一个典型的装饰器模式应用: TransactionAwareCacheDecorator

通常情况下, 缓存层和数据库层也需要保持事务的的一致性, 防止缓存写入后, 数据库事务回滚, 导致缓存出现脏数据. Spring 
是通过 TransactionAwareCacheDecorator 将缓存和数据库的写操作放到同一个事务中, 保证了两者的一致性. 

TransactionAwareCacheDecorator 通过实现 Cache 接口, 在内部封装一个 targetCache 对象, 然后将相关行为委托给 targetCache, 
并对 targetCache 的数据操作前后做了装饰, 添加了新的行为; 也就是利用 TransactionSynchronizationManager 实现的事务控制.

```java
/**
 * Cache decorator which synchronizes its put, evict and clear operations with Spring-managed transactions, 
 * through Spring's TransactionSynchronizationManager, performing the actual cache put/evict/clear 
 * operation only in the after-commit phase of a successful transaction. 
 * 
 * If no transaction is active, put, evict and clear operations will be performed immediately, as usual.
 */
public class TransactionAwareCacheDecorator implements Cache {

    private final Cache targetCache;

    @Override
    public String getName() {
        return this.targetCache.getName();
    }

    // ... ...

    @Override
    @Nullable
    public <T> T get(Object key, Callable<T> valueLoader) {
        return this.targetCache.get(key, valueLoader);
    }

    @Override
    public void put(final Object key, @Nullable final Object value) {
        if (TransactionSynchronizationManager.isSynchronizationActive()) {
            TransactionSynchronizationManager.registerSynchronization(new TransactionSynchronizationAdapter() {
                @Override
                public void afterCommit() {
                    TransactionAwareCacheDecorator.this.targetCache.put(key, value);
                }
            });
        }
        else {
            this.targetCache.put(key, value);
        }
    }

    @Override
    @Nullable
    public ValueWrapper putIfAbsent(Object key, @Nullable Object value) {
        return this.targetCache.putIfAbsent(key, value);
    }

    @Override
    public void evict(final Object key) {
        if (TransactionSynchronizationManager.isSynchronizationActive()) {
            TransactionSynchronizationManager.registerSynchronization(new TransactionSynchronizationAdapter() {
                @Override
                public void afterCommit() {
                    TransactionAwareCacheDecorator.this.targetCache.evict(key);
                }
            });
        }
        else {
            this.targetCache.evict(key);
        }
    }
    
    // ... ...
}
```

## <font color=red>Factory</font>

> ##### <font color=green>工厂模式: 用于封装和管理对象的创建, 隐藏内部细节, 使得创建对象和使用对象分离</font>

Spring 中一个典型的工厂模式应用: BeanFactory

BeanFactory 或 ApplicationContext (AnnotationConfigApplicationContext, ClassPathXmlApplicationContext ...)
就是 IOC 核心, 或者简单认为就是 IOC 容器, 而整个 IOC 本身就是一个工厂实现, 可以通过不同的方法获取 bean, 而不要手动 new, 
降低耦合. 

另一方面, 通过工厂方法暴露出共同的抽象接口, 在不修改客户端代码的情况下, 可以新增不同的子类, 这也是开闭原则的实践.

```java
public class User {
  private Long id;
  private String name;
  
  public User() {
  }
  
  public User(Long id, String name) {
    this.id = id;
    this.name = name;
  }
  
  public void setId(Long id) {
    this.id = id;
  }
  
  public void setName(String name) {
    this.name = name;
  }
}

// 实例化 Bean: 工厂方法
public class UserFactory {
    private static Map<Long, User> users = new HashMap<>();

    static{
        map.put(1, new User(1,"Apple"));
        map.put(2, new User(2,"Watermelon"));
        map.put(3, new User(3,"Cherry"));
    }

    public static User get(Long id){
        return users.get(id);
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans">

    <!-- 构造函数实例化 Bean -->
    <bean id="apple" class="org.spring.domain.User">
        <constructor-arg name="id" value="1"/>
        <constructor-arg name="name" value="Apple"/>
    </bean>

    <!-- setter 方法实例化 Bean -->
    <bean id="watermelon" class="org.spring.domain.User">
        <property name="id" value="2"/>
        <property name="name" value="Watermelon"/>
    </bean>

   <!-- 工厂方法实例化 Bean -->
    <bean id="cherry" class="org.spring.domain.UserFactory" factory-method="get">
        <constructor-arg value="3"/>
    </bean>
</beans>
```

除了通过 constructor/setter 方法, 还可以通过静态方法实例化 bean, 这些都是工厂方法的体现.

```java
public class BeanInstantiationDemo {
    public static void main(String[] args) {
        BeanFactory beanFactory = new ClassPathXmlApplicationContext("classpath:/META-INF/bean-context.xml");
        
        // constructor
        User apple = beanFactory.getBean("apple", User.class);
        
        // setter
        User watermelon = beanFactory.getBean("watermelon", User.class);
        
        // factory-method
        User cherry = beanFactory.getBean("cherry", User.class);
    }
}
```

## <font color=red>Observer</font>

> ##### <font color=green>观察者模式: 在观察目标和观察者之间建立一个抽象的耦合, 同时支持异步非阻塞编程</font>

Spring 中一个典型的观察者模式应用: ApplicationEvent

观察者模式是一种常见的编程模式, 它通过订阅机制, 实现发布者 publisher 和订阅者 subscribers 的状态互动, 适用于一对一或者一对多的对象交互场景; 
Java 中的 Observable, Observer, 以及 Guava 提供的 EventBus 等都是对观察者模式的支持. 

Spring 中实现的观察者模式: Event 事件代表了目标对象, Listener 监听者对应者观察者或者订阅者, Publisher 发布者也就是被观察者.

核心逻辑在 AbstractApplicationContext 中, 把观察者注册到 context 中， 以及发布事件到订阅者。

```java
public abstract class AbstractApplicationContext extends DefaultResourceLoader implements ConfigurableApplicationContext {
    
    // ... ...

    /** Helper class used in event publishing. */
    @Nullable
    private ApplicationEventMulticaster applicationEventMulticaster;

    /** Statically specified listeners. */
    private final Set<ApplicationListener<?>> applicationListeners = new LinkedHashSet<>();

    /** Local listeners registered before refresh. */
    @Nullable
    private Set<ApplicationListener<?>> earlyApplicationListeners;

    /** ApplicationEvents published before the multicaster setup. */
    @Nullable
    private Set<ApplicationEvent> earlyApplicationEvents;
    
    // ... ...
    
    /**
     * Publish the given event to all listeners. May be an ApplicationEvent or a payload object to 
     * be turned into a  PayloadApplicationEvent.
     */
    protected void publishEvent(Object event, @Nullable ResolvableType eventType) {
        Assert.notNull(event, "Event must not be null");

        // Decorate event as an ApplicationEvent if necessary
        ApplicationEvent applicationEvent;
        if (event instanceof ApplicationEvent) {
            applicationEvent = (ApplicationEvent) event;
        }else {
            applicationEvent = new PayloadApplicationEvent<>(this, event);
            if (eventType == null) {
                eventType = ((PayloadApplicationEvent<?>) applicationEvent).getResolvableType();
            }
        }

        // Multicast right now if possible - or lazily once the multicaster is initialized
        if (this.earlyApplicationEvents != null) {
            this.earlyApplicationEvents.add(applicationEvent);
        }else {
            getApplicationEventMulticaster().multicastEvent(applicationEvent, eventType);
        }
    }
    // ... ...

    @Override
    public void addApplicationListener(ApplicationListener<?> listener) {
        Assert.notNull(listener, "ApplicationListener must not be null");
        if (this.applicationEventMulticaster != null) {
            this.applicationEventMulticaster.addApplicationListener(listener);
        }
        this.applicationListeners.add(listener);
    }

    /**
     * Return the list of statically specified ApplicationListeners.
     */
    public Collection<ApplicationListener<?>> getApplicationListeners() {
        return this.applicationListeners;
    }

    // ... ...
    
    /**
     * Add beans that implement ApplicationListener as listeners.
     */
    protected void registerListeners() {
        // Register statically specified listeners first.
        for (ApplicationListener<?> listener : getApplicationListeners()) {
            getApplicationEventMulticaster().addApplicationListener(listener);
        }

        // Do not initialize FactoryBeans here: We need to leave all regular beans
        // uninitialized to let post-processors apply to them!
        String[] listenerBeanNames = getBeanNamesForType(ApplicationListener.class, true, false);
        for (String listenerBeanName : listenerBeanNames) {
            getApplicationEventMulticaster().addApplicationListenerBean(listenerBeanName);
        }

        // Publish early application events now that we finally have a multicaster...
        Set<ApplicationEvent> earlyEventsToProcess = this.earlyApplicationEvents;
        this.earlyApplicationEvents = null;
        if (!CollectionUtils.isEmpty(earlyEventsToProcess)) {
            for (ApplicationEvent earlyEvent : earlyEventsToProcess) {
                getApplicationEventMulticaster().multicastEvent(earlyEvent);
            }
        }
    }
    
    /**
     * Return the internal ApplicationEventMulticaster used by the context.
     */
    ApplicationEventMulticaster getApplicationEventMulticaster() throws IllegalStateException {
        if (this.applicationEventMulticaster == null) {
            throw new IllegalStateException("ApplicationEventMulticaster not initialized - " +
                    "call 'refresh' before multicasting events via the context: " + this);
        }
        return this.applicationEventMulticaster;
    }
    
    // ... ...
}

// ApplicationEventMulticaster 是真正的消息发送者， 
public class SimpleApplicationEventMulticaster extends AbstractApplicationEventMulticaster {
    
    // ... ...

    @Override
    public void multicastEvent(final ApplicationEvent event, @Nullable ResolvableType eventType) {
        ResolvableType type = (eventType != null ? eventType : resolveDefaultEventType(event));
        
        // 通过线程池, 实现异步非阻塞, 同步阻塞两种类型的消息广播
        Executor executor = getTaskExecutor();
        for (ApplicationListener<?> listener : getApplicationListeners(event, type)) {
            if (executor != null) {
                executor.execute(() -> invokeListener(listener, event));
            }
            else {
                invokeListener(listener, event);
            }
        }
    }
}
```

基于 Spring 的事件机制, 开发者可以轻易实现事件的订阅机制; 这也体现了 Spring 的扩展机制, 在不修改代码的情况下, 
可以增加新的事件和监听, 另一方面也体现了开闭原则.

```java
// 继承 ApplicationEvent 的事件
public class EmailEvent extends ApplicationEvent {
  private String message;

  public EmailEvent(Object source, String message) {
    super(source);
  }

  public String getMessage() {
    return this.message;
  }
}

// 实现 ApplicationListener 的监听器
@Component
public class EmailListener implements ApplicationListener<EmailEvent> {
  @Override
  public void onApplicationEvent(EmailEvent event) {
    String message = event.getMessage();
    System.out.println(message);
  }
}

// Publisher 利用 ApplicationContext 发布
@Component
public class EmailPublisher {
  @Autowired
  private ApplicationContext appContext;

  public void publishEvent(EmailEvent event) {
    this.appContext.publishEvent(event);
  }
}
```

## <font color=red>Template Method</font>

> ##### <font color=green>模板方法模式: 定义了一个算法的步骤, 并允许子类为一个或多个步骤提供实现</font>

Spring 中一个典型的模板方法模式应用: AbstractApplicationContext

一个好的程序设计, 会区分变与不变, 然后把不变的部分抽象出公共实现, 把变化的部分隔离开, 用接口封装隔离, 或者抽象类约束子类;
而模板方法的一个标准做法, 就是利用一个抽象类, 将部分逻辑以具体的方法实现, 然后声明一些抽象方法来迫使子类进行不同地实现, 
这就很好的体现了变与不变的控制, 从而达到灵活复用.

钩子: 在模板方法中, 定义在父类中的一个空方法, 或者仅仅有默认实现, 子类可以自行决定是否覆盖的这个方法被称为钩子;
钩子可以让子类更加灵活的对变化点进行挂钩, 如, 作为一个条件控制, 控制抽象类的算法流程.

```java
// Uses the Template Method design pattern, requiring concrete subclasses to implement abstract methods
public abstract class AbstractApplicationContext extends DefaultResourceLoader implements ConfigurableApplicationContext {
    // ... ...

    // 公共实现
    @Override
    public void publishEvent(ApplicationEvent event) {
        publishEvent(event, null);
    }

    protected void publishEvent(Object event, @Nullable ResolvableType eventType) {
        Assert.notNull(event, "Event must not be null");

        // Decorate event as an ApplicationEvent if necessary
        ApplicationEvent applicationEvent;
        if (event instanceof ApplicationEvent) {
            applicationEvent = (ApplicationEvent) event;
        } else {
            applicationEvent = new PayloadApplicationEvent<>(this, event);
            if (eventType == null) {
                eventType = ((PayloadApplicationEvent<?>) applicationEvent).getResolvableType();
            }
        }

        // Multicast right now if possible - or lazily once the multicaster is initialized
        if (this.earlyApplicationEvents != null) {
            this.earlyApplicationEvents.add(applicationEvent);
        } else {
            getApplicationEventMulticaster().multicastEvent(applicationEvent, eventType);
        }
        // ... ...
    }

    // 钩子方法
    protected void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) {
    }

    //---------------------------------------------------------------------
    // Abstract methods that must be implemented by subclasses: 抽象方法
    //---------------------------------------------------------------------
    
    protected abstract void refreshBeanFactory() throws BeansException, IllegalStateException;
    
    protected abstract void closeBeanFactory();
    
    @Override
    public abstract ConfigurableListableBeanFactory getBeanFactory() throws IllegalStateException;
    
    // ... ...
}
```

AbstractApplicationContext 也可以认为是 Spring IOC 的核心实现, 包含了 Bean 的创建销毁过程, 这其中也涉及模板模式.
简单来说开发者可以根据 Spring 预留的扩展点自定义实现 Bean 的创建销毁过程.

```java
public class User {
    private Long id;

    private String name;
    
    public void initUser(){
        // ... ...
    }
    
    public void destroyUser(){
        // ... ...
    }
}
```

Spring Bean 的创建过程, 主要包含对象的创建和及初始化过程, 创建是利用反射来动态生成对象, 而初始化有两种模板实现方式;
一种是在类中自定义一个初始化函数, 并且通过 xml 或者注解, 显式地声明.

```java
public class UserDemo {
    
    // 初始化函数自定义, 因此 Spring 通过反射, 动态地调用这个初始化函数
    @Bean(initMethod = "initUser", destroyMethod = "destroyUser")
    public User user() {
        return new User();
    }
}
```

反射有一定的性能开销, 因此 Spring 提供了另外一种初始化途径 InitializingBean, 通过实现这个接口中的函数 afterPropertiesSet(),
完成初始化过程; Spring 在初始化 Bean 的同时默认调用, 而不需要使用反射来调用了.

```java
// 虽然避免了反射调用, 但是与框架代码耦合到一起, 侵入性高不利于后续迭代
public class UserDemo implements InitializingBean, DisposableBean {
    
    @Override
    public void afterPropertiesSet() {
        // ... ...
    }

    @Override
    public void destroy() {
        // ... ...
    }
}
```

模板方法的核心时抽象出一个固定的算法框架, 并让约束子类实现不同的行为, 但是每个不同的实现都需要一个子类来实现, 
这就会导致类的个数不断增加，使得系统臃肿; 这时可以利用回调 callback 来实现模板方法, 相比继承而言更加简洁灵活, 如, 
Jdk 中的 Array.sort() 方法, 通过Comparable#compareTo() 接口回调完成模板实现.

回调: 方法参数中传递一个接口, 父类在调用此方法时, 必须调用方法中传递的接口的实现类.

Spring 这种通过回调实现模板方法的应用: JdbcTemplate, RedisTemplate

```java
public class JdbcTemplate extends JdbcAccessor implements JdbcOperations {
    // ... ...

    // 封装
    @Override
    @Nullable
    public <T> T execute(String callString, CallableStatementCallback<T> action) throws DataAccessException {
        return execute(new SimpleCallableStatementCreator(callString), action);
    }

    // 模板方法
    @Override
    @Nullable
    public <T> T execute(CallableStatementCreator csc, CallableStatementCallback<T> action)
            throws DataAccessException {

        Assert.notNull(csc, "CallableStatementCreator must not be null");
        Assert.notNull(action, "Callback object must not be null");
        if (logger.isDebugEnabled()) {
            String sql = getSql(csc);
            logger.debug("Calling stored procedure" + (sql != null ? " [" + sql  + "]" : ""));
        }

        Connection con = DataSourceUtils.getConnection(obtainDataSource());
        CallableStatement cs = null;
        try {
            cs = csc.createCallableStatement(con);
            applyStatementSettings(cs);
            T result = action.doInCallableStatement(cs);
            handleWarnings(cs);
            return result;
        } catch (SQLException ex) {
            // ... ...
        }
    }
}

// 回调接口
@FunctionalInterface
public interface CallableStatementCallback<T> {

    @Nullable
    T doInCallableStatement(CallableStatement cs) throws SQLException, DataAccessException;

}
```

```java
public class RedisTemplate<K, V> extends RedisAccessor implements RedisOperations<K, V>, BeanClassLoaderAware {
    // ... ... 

    @Override
    public Boolean delete(K key) {

        byte[] rawKey = rawKey(key);

        Long result = execute(connection -> connection.del(rawKey), true);

        return result != null && result.intValue() == 1;
    }

    // 封装
    @Nullable
    public <T> T execute(RedisCallback<T> action, boolean exposeConnection) {
        return execute(action, exposeConnection, false);
    }

    // 模板方法
    @Nullable
    public <T> T execute(RedisCallback<T> action, boolean exposeConnection, boolean pipeline) {

        Assert.isTrue(initialized, "template not initialized; call afterPropertiesSet() before using it");
        Assert.notNull(action, "Callback object must not be null");

        RedisConnectionFactory factory = getRequiredConnectionFactory();
        RedisConnection conn = null;
        try {

            if (enableTransactionSupport) {
                // only bind resources in case of potential transaction synchronization
                conn = RedisConnectionUtils.bindConnection(factory, enableTransactionSupport);
            } else {
                conn = RedisConnectionUtils.getConnection(factory);
            }

            boolean existingConnection = TransactionSynchronizationManager.hasResource(factory);

            RedisConnection connToUse = preProcessConnection(conn, existingConnection);

            boolean pipelineStatus = connToUse.isPipelined();
            if (pipeline && !pipelineStatus) {
                connToUse.openPipeline();
            }

            RedisConnection connToExpose = (exposeConnection ? connToUse : createRedisConnectionProxy(connToUse));
            T result = action.doInRedis(connToExpose);

            // close pipeline
            if (pipeline && !pipelineStatus) {
                connToUse.closePipeline();
            }

            // TODO: any other connection processing?
            return postProcessResult(result, connToUse, existingConnection);
        } finally {
            RedisConnectionUtils.releaseConnection(conn, factory, enableTransactionSupport);
        }
    }
}

// 回调接口
public interface RedisCallback<T> {
    
    @Nullable
    T doInRedis(RedisConnection connection) throws DataAccessException;
}
```


## <font color=red>TODO</font>
