## 计算机基础

### 网络

#### http和https有什么区别？

HTTPS和HTTP的区别主要如下：

1、https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。

2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。

3、http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。

4、http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。

注：SSL(Secure Sockets Layer [安全套接层](https://baike.baidu.com/item/%E5%AE%89%E5%85%A8%E5%A5%97%E6%8E%A5%E5%B1%82))，SSL协议位于[TCP/IP协议](https://baike.baidu.com/item/TCP%2FIP%E5%8D%8F%E8%AE%AE)与各种[应用层](https://baike.baidu.com/item/%E5%BA%94%E7%94%A8%E5%B1%82)协议之间，为[数据通讯](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E9%80%9A%E8%AE%AF)提供安全支持。SSL协议可分为两层： SSL记录协议（SSL Record Protocol）：它建立在可靠的[传输协议](https://baike.baidu.com/item/%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)（如TCP）之上，为高层协议提供[数据封装](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%B0%81%E8%A3%85)、压缩、加密等基本功能的支持。 SSL[握手协议](https://baike.baidu.com/item/%E6%8F%A1%E6%89%8B%E5%8D%8F%E8%AE%AE)（SSL Handshake Protocol）：它建立在SSL记录协议之上，用于在实际的数据传输开始前，通讯双方进行[身份认证](https://baike.baidu.com/item/%E8%BA%AB%E4%BB%BD%E8%AE%A4%E8%AF%81)、协商[加密算法](https://baike.baidu.com/item/%E5%8A%A0%E5%AF%86%E7%AE%97%E6%B3%95)、交换加密[密钥](https://baike.baidu.com/item/%E5%AF%86%E9%92%A5)等。



## java基础

### 基础知识

#### 1.Obect类有哪些方法？

#### 2.sleep和wait方法的区别？

1.sleep属于Thread类的。wait属于Object类的

2.sleep方法导致是线程暂停执行指定时间，让出cpu给其他线程，但是它的状态依然保持着，当指定的时间到了之后有自动恢复到运行状态。

 3.在调用sleep方法的过程中，线程不会释放对象锁。 而当调用wait方法的时候，线程会放弃对象锁，进入等待此对象的等待锁定池。只有针对此对象调用notify方法后，本线程才进入对象锁定池准备获取对象锁进入运行状态。



### JVM 

#### 1. Jvm的内存模型是什么？java的内存模型？

java并发内存模型

java试图定义一个Java内存模型(Java memory model JMM)来屏蔽掉各种硬件/操作系统的内存访问差异，以实现让java程序在各个平台下都能达到一致的内存访问效果。java内存模型主要目标是定义程序中各个变量的访问规则，即在虚拟机中将变量存储到内存和从内存中取出变量这样的底层细节。模型图如下

![img](https://images2015.cnblogs.com/blog/286989/201611/286989-20161124093612487-2048767455.jpg)

java内存模型中规定了所有变量都存贮到主内存（如虚拟机物理内存中的一部分）中。每一个线程都有一个自己的工作内存(如cpu中的高速缓存)。线程中的工作内存保存了该线程使用到的变量的主内存的副本拷贝。线程对变量的所有操作（读取、赋值等）必须在该线程的工作内存中进行。不同线程之间无法直接访问对方工作内存中变量。线程间变量的值传递均需要通过主内存来完成。

关于主内存与工作内存之间的交互协议，即一个变量如何从主内存拷贝到工作内存。如何从工作内存同步到主内存中的实现细节。java内存模型定义了8种操作来完成。这8种操作每一种都是原子操作。8种操作如下：

- lock(锁定)：作用于主内存，它把一个变量标记为一条线程独占状态；
- unlock(解锁)：作用于主内存，它将一个处于锁定状态的变量释放出来，释放后的变量才能够被其他线程锁定；
- read(读取)：作用于主内存，它把变量值从主内存传送到线程的工作内存中，以便随后的load动作使用；
- load(载入)：作用于工作内存，它把read操作的值放入工作内存中的变量副本中；
- use(使用)：作用于工作内存，它把工作内存中的值传递给执行引擎，每当虚拟机遇到一个需要使用这个变量的指令时候，将会执行这个动作；
- assign(赋值)：作用于工作内存，它把从执行引擎获取的值赋值给工作内存中的变量，每当虚拟机遇到一个给变量赋值的指令时候，执行该操作；
- store(存储)：作用于工作内存，它把工作内存中的一个变量传送给主内存中，以备随后的write操作使用；
- write(写入)：作用于主内存，它把store传送值放到主内存中的变量中。

Java内存模型还规定了执行上述8种基本操作时必须满足如下规则:

- 不允许read和load、store和write操作之一单独出现，以上两个操作必须按顺序执行，但没有保证必须连续执行，也就是说，read与load之间、store与write之间是可插入其他指令的。
- 不允许一个线程丢弃它的最近的assign操作，即变量在工作内存中改变了之后必须把该变化同步回主内存。
- 不允许一个线程无原因地（没有发生过任何assign操作）把数据从线程的工作内存同步回主内存中。
- 一个新的变量只能从主内存中“诞生”，不允许在工作内存中直接使用一个未被初始化（load或assign）的变量，换句话说就是对一个变量实施use和store操作之前，必须先执行过了assign和load操作。
- 一个变量在同一个时刻只允许一条线程对其执行lock操作，但lock操作可以被同一个条线程重复执行多次，多次执行lock后，只有执行相同次数的unlock操作，变量才会被解锁。
- 如果对一个变量执行lock操作，将会清空工作内存中此变量的值，在执行引擎使用这个变量前，需要重新执行load或assign操作初始化变量的值。
- 如果一个变量实现没有被lock操作锁定，则不允许对它执行unlock操作，也不允许去unlock一个被其他线程锁定的变量。
- 对一个变量执行unlock操作之前，必须先把此变量同步回主内存（执行store和write操作）。

#### 2. jvm采用什么垃圾收集器？G1和CMS有什么好处？

jdk1.7 默认垃圾收集器Parallel Scavenge（新生代）+Parallel Old（老年代）

jdk1.8 默认垃圾收集器Parallel Scavenge（新生代）+Parallel Old（老年代）

jdk1.9 默认垃圾收集器G1

**评价垃圾回收质量的两个指标：**

- 停顿时间

- 吞吐量

  G1有着更可控的pause time 和 更大的throughput，所以g1在java9 便是默认的垃圾收集器，是cms 的替代

垃圾收集常用参数

| 参数                           | 描述                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| UseSerialGC                    | 虚拟机运行在Client模式下的默认值，打开此开关后，使用Serial + Serial Old的收集器组合进行内存回收 |
| UseParNewGC                    | 打开此开关后，使用ParNew + Serial Old的收集器组合进行内存回收 |
| UseConcMarkSweepGC             | 打开此开关后，使用ParNew + CMS + Serial Old的收集器组合进行内存回收。Serial Old收集器将作为CMS收集器出现Concurrent Mode Failure失败后的后备收集器使用 |
| **UseParallelGC**              | **虚拟机运行在Server模式下的默认值，打开此开关后，使用Parallel Scavenge + Serial Old（PS MarkSweep）的收集器组合进行内存回收** |
| UseParallelOldGC               | 打开此开关后，使用Parallel Scavenge + Parallel Old 的收集器组合进行内存回收 |
| SurvivorRatio                  | 新生代中Eden区域与Survivor区域的容量比值，默认为8，代表Eden : Survivor=8:1 |
| PretenureSizeThreshold         | 直接晋升到老年代的对象大小，设置这个参数后，大于这个参数的对象将直接在老年代分配 |
| MaxTenuringThreshold           | 晋升到老年代的对象年龄。每个对象在坚持过一次Minor GC之后，年龄就增加1，当超过这个参数值时就进入老年代 |
| UseAdaptiveSizePolicy          | 动态调整Java堆中各个区域的大小以及进入老年代的年龄           |
| HandlePromotionFailure         | 是否允许分配担保失败，即老年代的剩余空间不足以应付新生代的整个Eden和Survivor区的所有对象都存活的极端情况 |
| ParallelGCThreads              | 设置并行GC时进行内存回收的线程数                             |
| GCTimeRatio                    | GC时间占总时间的比率，默认值为99，即允许1%的GC时间。仅使用Parallel Scavenge收集器时生效 |
| MaxGCPauseMillis               | 设置GC的最大停顿时间。仅在使用Parallel Scavenge收集器生效    |
| CMSInitiatingOccupancyFraction | 设置CMS收集器在老年代空间被使用多少后触发垃圾收集，默认值为68%，仅使用CMS收集器时生效 |
| UseCMSCompactAtFullCollection  | 设置CMS收集器在完成垃圾收集后是否要进行一次内存碎片整理。仅在使用CMS收集器时生效 |
| CMSFullGCsBeforeCompaction     | 设置CMS收集器在进行若干次垃圾收集后再启动一次内存碎片整理。仅在使用CMS收集器时生效 |

#### 3.什么情况下发生fullGC ?怎么样避免fullGC 

#### 4.查询gc情况采用什么命令？如果cpu过高，但是jvm内存使用正常？哪些情况会引起？怎么排查？

Linux使用jstat命令查看jvm的GC情况  

```shell
jstat [ generalOption | outputOptions vmid [interval[s|ms] [count]] ]
```

我们一般使用 -gcutil 查看gc情况 。vmid，VM的进程号，即当前运行的java进程号. interval，间隔时间，单位为秒或者毫秒

 vmid是虚拟机ID，在Linux/Unix系统上一般就是进程ID。interval是采样时间间隔。count是采样数目。比如下面输出的是GC信息，采样时间间隔为250ms，采样数为4：

```shell
jstat -gc 21711 250 4
```



jvm在疯狂的Full GC 会导致cpu过高？

1. 执行top -c命令，找到cpu最高的进程的id
2.  执行top -H -p pid，这个命令就能显示刚刚找到的进程的所有线程的资源消耗情况。找到CPU负载高的线程tid 8627, 把这个数字转换成16进制，21B3（10进制转16进制，用linux命令: printf %x 8627）。
3. 执行jstack -l pid，拿到进程的线程dump文件。这个命令会打出这个进程的所有线程的运行堆栈。
4. 用记事本打开这个文件，搜索“21B3”，就是搜一下16进制显示的线程id。搜到后，下面的堆栈就是这个线程打出来的。排查问题从这里深入。





## 主流框架

### Spring

#### 1.@Autowired和@Resource有啥区别？

- @Autowired与@Resource都可以用来装配bean. 都可以写在字段上,或写在setter方法上。

- @Autowired默认按类型装配（这个注解是属业spring的），默认情况下必须要求依赖对象必须存在，如果要允许null值，可以设置它的required属性为false，如：@Autowired(required=false) ，如果我们想使用名称装配可以结合@Qualifier注解进行使用，如下：

  ```java
  @Autowired() 
  @Qualifier(“baseDao”)
  private BaseDao baseDao;
  ```

- @Resource（这个注解属于J2EE的），默认按照名称进行装配，名称可以通过name属性进行指定，如果没有指定name属性，当注解写在字段上时，默认取字段名进行安装名称查找，如果注解写在setter方法上默认取属性名进行装配。当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的是，如果name属性一旦指定，就只会按照名称进行装配。

#### spring的消息机制是什么样的？启动完成之后执行的是什么？

**Spring消息机制**

**消息机制是使用消息通知的方式，解耦生产者与消费者。编程上体现的是职责分割，使得消息处理的扩展性得到增强，符合设计原则中的单一职责以及开闭原则。**

ApplicationContext提供事件处理通过ApplicationEvent类和ApplicationListener接口。如果一个bean实现ApplicationListener接口在容器中,每次一个ApplicationEvent被发布到ApplicationContext中,这类bean就会收到这些通知。从本质上讲,这是标准的观察者设计模式。



Spring的ApplicationListener接口

**ApplicationListener**是Spring应用监听器基础接口，是消费者需要实现，用来消费特定事件的，接口如下：

```java
public interface ApplicationListener<E extends ApplicationEvent> 
extends EventListener {
    /**
    * Handle an application event.
    * @param event the event to respond to
    */
    void onApplicationEvent(E event);
  }
```



实现Spring事件机制主要有4个类：

ApplicationEvent：事件，每个实现类表示一类事件，可携带数据。
ApplicationListener：事件监听器，用于接收事件、处理事件。
ApplicationEventMulticaster：事件管理者，用于事件监听器的注册和事件的广播。
ApplicationEventPublisher：事件发布者，委托ApplicationEventMulticaster完成事件发布

ApplicationEvent  ：事件就是一个包含了任意对象并含有事件对象创建时间戳的类。



```java
public abstract class ApplicationEvent extends EventObject {
   private static final long serialVersionUID = 7099057708183571937L;
   private final long timestamp;
   public ApplicationEvent(Object source) {
      super(source);
      this.timestamp = System.currentTimeMillis();
   }
   public final long getTimestamp() {
      return this.timestamp;
   }
}
```





利用**Spring**消息机制进行业务处理，逻辑非常简单，只需要以下几个步骤：

1. *定义事件消息，即需要发布的消息对象*，例如：

```java
public class ApplicationEventSimple extends ApplicationEvent
```

1. *定义处理器，并声明能够处理的消息类型*，即实现**ApplicationListener**接口，实现类被Spring托管后，会自动被识别，并加入到消息监听器集合中，使用实例：

```java
@Component
public class DefaultListener 
implements ApplicationListener<ApplicationEventSimple> 
```

1. *发布消息*，通过**ApplicationContext.publishEvent(ApplicationEvent event)**发布消息，Spring会自动找到对应的处理器进行处理。

Spring内置事件

Spring在容器启动阶段或者销毁阶段，提供了相关的事件通知，通过传递整个*ApplicationContext*，以便使用者在容器启停阶段能够植入自己的逻辑。

消息类的基类为：**ApplicationContextEvent**，具体实现有*ContextStartedEvent（容器开始启动事件）*，*ContextClosedEvent（容器关闭时事件）*，*ContextRefreshedEvent（IOC容器启动完成事件）*，*ContextStoppedEvent（容器停止事件）*。

如果想要监听这些事件，只需要实现监听程序即可，我司RPC自动注册就是通过ContextRefreshedEvent消息的监听，然后处理的。

**最后，Spring消息消费可以控制异步或者同步执行，而正确的使用消息机制对于代码的逻辑控制将会非常的灵活。**

#### spring默认的事物传播方式？具体什么意思？

### mybais

#### 1.mybatis的#和$的区别

mybatis中有两个常见的#和$符号，在进行变量的赋值的时候是很常见的，我的理解就是#是需要进行编译的，是字符串进行编译之后在放入到sql中，如果不是单纯的字符串替换，而$是直接进行字符串的替换工作，两者各有有缺点。

```
一、#
我们一般在刚刚接触到mybatis的时候都是使用的是#{变量名字}，来进行sql语句的赋值工作，比如 select * from table_name where name = #{name}，来进行查询，在接口里面传入Map或者是String都是可以的，这样的是mybatis进行了语句的编译工作，#{name}，必须要放到name=后面，这样才会编译通过，进行执行sql语句。

二、$
然而有的时候我们需要进行变化传输sql语句，这个时候一般都会用到的是<if>等mybatis里面的动态标签来动态拼接sql语句，但是有的时候根据一些业务场景是需要传输表名或者是特定的变量名的，这个时候就需要根据业务来动态的改变查询的表名等问题，这个时候的好处就体现了，我的感觉就是单纯的字符串替换，比如，select * from table_name where name = {name}这个时候mybatis不需要编译而是直接进行字符串的替换工作，比如我要传入一个表名，select * from {tableName} where name = #{name}，这个时候在前台就需要传输map来进行传参或者是entity（自己建立的数据库表里面的实体的别名），前面的{tableName}就是单纯的字符串替换，而#{name}则需要进行编译才可以赋值

三、$不是特别安全，容易引起的注入问题，导致毁灭性灾难，但是也不是说一点不能用，只能说不可以从前台传输有关于的语句来使用不是特别安全，容易引起sql的注入问题，导致毁灭性灾难，但是也不是说一点不能用，只能说不可以从前台传输有关于sql的语句来使用$否则就会出现删除数据的问题，所以请大家慎重使用
```

### Spring Boot

#### spring boot的启动机制说一下。



## 分布式

### zookeeper

#### 1.zookeeper leader选举机制？zookeeper  三个节点一个节点挂了之后，怎么选举的？

ZooKeeper选举Leader依赖下列原则并遵循优先顺序：

**1、选举投票必须在同一轮次中进行**

如果Follower服务选举轮次不同，不会采纳投票。

**2、数据最新的节点优先成为Leader**

数据的新旧使用事务ID判定，事务ID越大认为节点数据约接近Leader的数据，自然应该成为Leader。

**3、比较server.id，id值大的优先成为Leader**

如果每个参与竞选节点事务ID一样，再使用server.id做比较。server.id是节点在集群中唯一的id，myid文件中配置。



### RabbitMQ

#### RabbitMQ的消息确认机制有哪些？

**1.消息发送确认。**这种是用来确认生产者将消息发送给交换器，交换器传递给队列的过程中，消息是否成功投递。发送确认分为两步，一是确认是否到达交换器，二是确认是否到达队列。

**2.消费接收确认。**这种是确认消费者是否成功消费了队列中的消息。

1）ConfirmCallback

通过实现ConfirmCallBack接口，消息发送到交换器Exchange后触发回调。

![RabbitMQ的消息确认机制](http://p3.pstatp.com/large/pgc-image/1532946146596ed9413e5d2)

使用该功能需要开启确认，spring-boot中配置如下：

spring.rabbitmq.publisher-confirms = true



（2）ReturnCallback

通过实现ReturnCallback接口，如果消息从交换器发送到对应队列失败时触发（比如根据发送消息时指定的routingKey找不到队列时会触发）

![RabbitMQ的消息确认机制](http://p9.pstatp.com/large/pgc-image/1532946688790b3c9437fef)

 

使用该功能需要开启确认，spring-boot中配置如下：

spring.rabbitmq.publisher-returns = true



三：消息接收确认

1）确认模式

- AcknowledgeMode.NONE：不确认
- AcknowledgeMode.AUTO：自动确认
- AcknowledgeMode.MANUAL：手动确认

spring-boot中配置方法：

spring.rabbitmq.listener.simple.acknowledge-mode = manual

（2）手动确认

未确认的消息数

上图为channel中未被消费者确认的消息数。

通过RabbitMQ的host地址加上默认端口号15672访问管理界面。

（2.1）成功确认

void basicAck(long deliveryTag, boolean multiple) throws IOException;

deliveryTag:该消息的index

multiple：是否批量. true：将一次性ack所有小于deliveryTag的消息。

消费者成功处理后，调用channel.basicAck(message.getMessageProperties().getDeliveryTag(), false)方法对消息进行确认。

（2.2）失败确认

void basicNack(long deliveryTag, boolean multiple, boolean requeue)

throws IOException;

deliveryTag:该消息的index。

multiple：是否批量. true：将一次性拒绝所有小于deliveryTag的消息。

requeue：被拒绝的是否重新入队列。

 

void basicReject(long deliveryTag, boolean requeue) throws IOException;

deliveryTag:该消息的index。

requeue：被拒绝的是否重新入队列。

channel.basicNack 与 channel.basicReject 的区别在于basicNack可以批量拒绝多条消息，而basicReject一次只能拒绝一条消息。

 

（2.3）消息拒绝后，再次发布消息

```
channel.basicPublish(message.getMessageProperties().getReceivedExchange(),
                    message.getMessageProperties().getReceivedRoutingKey(), 
                    MessageProperties.PERSISTENT_TEXT_PLAIN,
                    message.getBody());
```



、

Bean的生命周期：

       1，实例化bean
    
       2，设置javaBean的属性值
    
       3，若该bean实现了BeanNameAware接口，则调用该接口的setBeanName()方法
    
       4，若该bean实现了BeanFactoryAware接口，则调用该接口的setBeanFactory()方法 
    
       5, 若sping为所有javaBean配置了后处理器，即实现了BeanPostPorcessor接口的java类，并在配置文件中注册为bean
    
          调用BeanPostProcessor接口的postProcessBeforeInitialization()方法 
    
       6，若bean实现了InitializingBean接口，则调用该接口的afterPropertiesSet()方法 
    
       7, 调用bean中自己定制的初始化方法：配置文件中配置init-method， 7和8的初始化方法是平级的，可共存，效果一样，一般选其一即可 
    
       8，调用BeanPostPorcessor接口的postProcessAfterInitialization()方法 
    
       容器销毁后，调用bean中定制的销毁方法
    
       9，若该bean实现了DisposableBean接口，调用其destroy()方法，      
    
       10，配置文件中指定自定义的销毁方法：destroy-method  , 9和10是两种不同的销毁方式，是平级的，可共存，效果一样，一般选其一即可 

图解：

![img](https://images0.cnblogs.com/blog/383337/201303/16230802-6b5a6341839a48e58488896e672693fa.jpg)





### Dubbo

#### dubbo的负载均衡和集群容错策略

#### dubbo怎么更新缓存。采用什么方式？

#### dubbo重试

#### dubbo服务降级

#### dubbo可以服务分组吗？有什么作用？





### Solr

#### solr分词机制

### Redis

#### reidis怎么删除锁?redis避免死锁?








