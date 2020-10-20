# SpringCloud

## Eureka

### 为什么使用Eureka？

微服务架构的项目中，子系统之间不在同一个环境下，需要通过**远程调用**来实现各个子系统之间的**通讯**问题。

远程调用，就需要通过IP地址：

- 服务A调用服务B，通过IP地址调用：

  `http://111.111.111.111:8888/yuer/B`

- 服务C调用服务B，通过IP地址调用：

  `http://111.111.222.333:8888/yuer/C`

- 出现的问题：

  - 万一B的IP地址变了，就需要更改A服务和C服务中的调用地址，在服务多的情况下，维护这些静态配置比较不方便。

> 为了解决微服务架构中**服务实例维护问题（IP地址等）**，产生了大量的服务治理框架和产品，这些框架和产品的实现都围绕着服务注册和服务发现机制对微服务应用实例**自动化的管理**。

SpringCloud中，使用Eureka来管理微服务。

- 创建一个Eureka Server，将A、B、C、D四个服务的信息都注册到Eureka Server上，Eureka Server来维护这些已经注册的信息。

![image-20201019170547630](https://gitee.com/yuer-pic/picgo/raw/master/pictures/20201019210043.png)

A、B、C、D四个服务都可以拿到Eureka Server的注册清单。这四个服务相互调用可以不使用具体的IP地址，而是用过服务名来调用。

- 拿到注册清单，就能拿到具体服务的具体地址。
- 通过服务名找到IP地址（IP地址会变，但是服务名不会变）

![image-20201019174003926](https://gitee.com/yuer-pic/picgo/raw/master/pictures/20201019210054.png)

### Eureka细节

- Eureka用于给其他服务注册，称为Eureka Server（服务注册中心），其余注册到Eureka Server的服务称为Eureka Client。
- Eureka Client**分别为服务提供者和服务消费者**。
  - 但是有可能某服务**既是服务提供者有事服务消费者**。

- Eureka的治理机制：
  - **服务注册**：启动时会通过发送REST请求的方式将自己注册到Eureka Server上，同时带上了自身服务的一些元数据。
  - **服务续约**：在注册完服务后，**服务提供者会维护一个心跳**，用来持续告诉Eureka Server当前服务还活着。
  - **服务下线**：**当服务实例在进行正常的关闭操作时，它会触发一个服务下线的REST请求**给Eureka Server，告诉服务注册中心本服务将要下线。
  - **获取服务**：**当我们启动服务消费者**的时候，会发送一个REST请求给服务注册中心，来获取上面注册过的服务清单。
  - **服务调用**：**服务消费者在获取服务清单后，通过服务名**可以获得具体提供服务的实例名和实例的元数据信息。在进行服务调用后，**优先访问同处一个Zone的服务提供方**。
- Eureka Server（服务注册中心）：
  - **失效剔除**：默认每隔一段时间（默认60秒）将当前清单中超时（默认90秒）没有的服务剔除出去。
  - **自我保护**：Eureka Server在运行期间，会统计心跳失败的比例在15分钟内是否低于85%（通常由于网络不稳定导致）。Eureka Server会将当前的**实例注册信息保护起来**，让这些实例不会过期，尽可能**保护这些注册信息**。

![image-20201019170547630](https://gitee.com/yuer-pic/picgo/raw/master/pictures/20201019210058.png)

## RestTemplate和Ribbon

通过Eureka服务治理框架，我们可以通过服务名来调用相应的服务了，一般在使用SpringCloud不需要自己手动创建HTTPClient来进行远程调用。

可以使用Spring封装好的RestTemplate工具类：

```java
// 传统的方式，直接显示写死IP是不好的！
    //private static final String REST_URL_PREFIX = "http://localhost:8001";
	
	// 服务实例名
    private static final String REST_URL_PREFIX = "http://MICROSERVICECLOUD-DEPT";

    /**
     * 使用 使用restTemplate访问restful接口非常的简单粗暴无脑。 (url, requestMap,
     * ResponseBean.class)这三个参数分别代表 REST请求地址、请求参数、HTTP响应转换被转换成的对象类型。
     */
    @Autowired
    private RestTemplate restTemplate;

    @RequestMapping(value = "/consumer/dept/add")
    public boolean add(Dept dept) {
        return restTemplate.postForObject(REST_URL_PREFIX + "/dept/add", dept, Boolean.class);
    }
/**
 *   作者：Java3y
 *   链接：https://www.zhihu.com/question/283286745/answer/763040709
 *   来源：知乎
 *   著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
 */
```

为了实现**高可用性**，我们可以将服务提供者集群。比如，一个秒杀系统准备上线。在11月11日为了可以支持高并发，开启多台机器来支持。

![image-20201019175452869](https://gitee.com/yuer-pic/picgo/raw/master/pictures/20201019210103.png)

现在想对这三个秒杀系统进行合理分配用户的请求（负载均衡），也许你会想到Nginx。

SpringCloud也支持负载均衡的功能，只是它是**客户端的负载均衡**，这个功能的实现就是Ribbon。

负载均衡分为两种类型：

- 客户端负载均衡（Ribbon）
  - 服务实例的清单在客户端，客户端进行负载均衡算法进行分配。
  - 客户端从Eureka Server中获得一份服务清单，在发送请求时通过负载均衡算法，在多个服务器之间选择一个进行访问
- 服务端负载均衡（Nginx）
  - 服务实例的清单在服务端，服务器进行负载均衡算法分配

![image-20201019194730357](https://gitee.com/yuer-pic/picgo/raw/master/pictures/20201019210109.png)

### Ribbon细节

Ribbon是支持负载均衡，默认负载均衡策略是轮询，我们也可以根据自己实际需求自定义负载均衡策略。

```java
@Configuration
public class MySelfRule
{
	@Bean
	public IRule myRule()
	{
		//return new RandomRule();// Ribbon默认是轮询，我自定义为随机
		//return new RoundRobinRule();// Ribbon默认是轮询，我自定义为随机
		
		return new RandomRule_ZY();// 我自定义为每台机器5次
	}
}
/**
 *   作者：Java3y
 *   链接：https://www.zhihu.com/question/283286745/answer/763040709
 *   来源：知乎
 *   著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
 */
```

继承AbstractLoadBalancerRule类，重写`public Server choose(ILoadBalancer lb, Object key)`即可实现自定义负载均衡策略

## Hystrix

### 当一个服务调用多个远程服务时，某个服务出现延迟，会发生什么？

![image-20201019210635856](https://gitee.com/yuer-pic/picgo/raw/master/pictures/20201019210635.png)

在高并发的情况下，由于单个服务的延迟，可能导致所有请求都处于延迟状态，甚至几秒钟就使服务处于负载饱和状态，资源耗尽，直到不可用，最终导致整个分布式系统都不可用，这就是“服务雪崩”。

为了解决上述问题，Spring Could Hystrix实现了**断路器、线程隔离**等服务保护功能。

- Fallback（失败快速返回）：当某个服务单元发生故障（类似用电器发生短路）之后，通过断路器的故障监控（类似熔断保险丝），**向调用方法返回一个错误响应，而不是长时间等待**,这样就算某个依赖服务出现延迟过高的情况，也只是对该该依赖服务的调用产生影响，而不会拖慢其他依赖服务。

Hystrix提供了几个熔断关键参数“`滑动窗口大小(20)、熔断器开关间隔(5s)、错误率(50%)`

- 每当20个请求中有50%失败时，熔断器就会打开，此时调用服务的时候，会**直接返回错误响应**不会再调用远程服务。
- 直到5秒钟之后，重新检测该出发条件，判断是否需要把熔断器关闭，或者继续打开。

Hystrix还有请求合并、请求缓存等强大功能。

#### Hystrix仪表盘

Hystrix仪表盘：主要用来实时监控Hystrix的各项指标信息。通过Hystrix Dashboard反馈的实时信息，可以帮助我们快速发现系统中存在的问题，从而采取应对措施。