# 技能树
### 集合
集合.1-[collection接口(list set queue)](https://www.cnblogs.com/nayitian/p/3266090.html)
LinkedList: 双向链表 Node节点：value ,previous,next。维护了一个first,last指针。add():直接add last, add（index i ,E e):看i位于那个半区，然后从fisrt or last开始遍历。
ArrayList:数组，满了才扩容，扩容1.5倍，Arrays.copyOf
“==” 基本数据类型的话，比较的是值，基本数据类型是在虚拟机栈里存储，自定义数据类型的话，比较的是是否指向同一个地址的引用，String存在字符串常量池中。
equals，基本数据类型不能equals ，Object 类是== ，String比较的是字符串内容，其他类型最好重写。

集合.2-[concurrent包集合](https://www.cnblogs.com/nayitian/p/3319040.html)


字典.1-[map接口](https://www.cnblogs.com/nayitian/p/3267110.html) </br>
[hashMap](https://www.cnblogs.com/LiaHon/p/11149644.html)</br>
[linkedHashMap](https://www.cnblogs.com/LiaHon/p/11180869.html)</br>
[treeMap](https://www.cnblogs.com/LiaHon/p/11221634.html)

字典.2-[concurrent包map](https://www.cnblogs.com/nayitian/p/3319379.html)

[二叉树、平衡二叉树、红黑树](https://xiaozhuanlan.com/topic/5036471892)</br>
[红黑树](https://blog.csdn.net/qq_36610462/article/details/83277524)
红黑树的意义：达到近似于平衡二叉树的遍历性能，而不用严格为达到平衡二叉树的要求（平衡因子不超过1）而浪费过多性能去调整

[数据库索引和B+tree](https://www.bilibili.com/video/BV1UC4y1p7zm) </br>
.最重要的原因是从harddisk读取数据的成本（I/O）远大于 在momory中遍历的成本，我们当然知道二分法是效率最高的（遍历次数最少），但是二分法一次只能读取一个数，所以需要的读取成本更高。</br>
[数据库为什么要用B+tree索引](https://zhuanlan.zhihu.com/p/57359459) </br>

### 多线程
[thread类状态及常用方法](https://www.jianshu.com/p/dd36ac5bce05) </br>
#### 1.多线程编程的挑战 </br>
1、上下文切换 </br>
1.1 尽量采用无锁兵法编程，比如hash取模 </br>
1.2 CAS </br>
2、死锁 </br>
2.1避免一个线程获得多个锁 </br>
2.2避免一个锁处理多个资源 </br>
2.3使用定时锁 </br>
2.4数据库锁，加锁和解锁在一个数据库链接里 </br>
3、资源限制 </br>
带宽、硬盘读写、cpu处理速度、socket链接、数据库链接 </br>
#### 2.JMM内存 
共享 or 通信 </br>
##### 2.1 
volatile 多线程中的变量可见性，主要是cpu的缓存L1 L2 L3 </br>
volatile声明的变量在汇编代码里会加上LOCK前缀 </br>
lock前缀会做两件事情， </br>
1，写操作后变量的值会立即被写会到主内存中  </br>
2，写回操作会使得其他cpu缓存里的内存地址失效，强制cpu缓存重新从主内存中获取 </br>
##### 2.2 
volatile与锁的区别，和重排序： </br>
1， volatile写，锁的解锁 都是 刷新 线程内存到主内存 语义 相当于锁 的解锁，所以 第二个操作是volatile写 之前的操作都不允许重排序 </br>
    volatile读，锁的加锁， 从主内存再次获取变量，语义 相当于 锁 的加锁，，所以 第一个操作是volatile读 之后的操作都不允许重排序 </br>
    第一个操作是volatile写，第二个是volatile读，不允许重排序 </br>
2，volatile只能保证单个变量的读写有原子性 ，锁可以保证整个代码块，锁更强大，volitale更灵活 </br>

##### 2.3 synchionized
[synchionies](https://juejin.im/post/6844903600334831629) </br>
[syn锁升级](https://blog.csdn.net/tongdanping/article/details/79647337) </br>
##### 2.4 final
[final](https://juejin.im/post/6844903601068998664#heading-4) </br>
JMM禁止编译器把final域的写重排序到构造函数之外:final变量，先赋值，再被构造对象的引用赋值给读取者 </br>

##### 2.5 类初始化
双重检测锁 1创建内存空间、2对象初始化、3设置对象指向内存空间（本意是先初始化后指向），但是2，3有可能重排序，先指向，此时还为初始化，为null，可用volatile声明，禁止重排序。
this溢出：要先构造完成，再赋值给其他引用。否则对象还没构造完成，就赋值了，就造成逃逸。

##### 2.6 as if serial && happens-before
as if serial语义，对单线程会有重排序控制，对多线程不会，跟happens-before一样

##### 2.7 锁

threadLocalMap 实际上就是一个以 threadLocal 实例为 key，任意对象为 value 的 Entry 数组

#### 3.设计模式
3.1 工厂模式 调用方尽量持有接口或抽象类，避免持有具体类型的子类，以便工厂方法能随时切换不同的子类返回，却不影响调用方代码。 </br>
里氏替换原则：返回实现接口的任意子类都可以满足该方法的要求，且不影响调用方。</br>
3.2 抽象工厂 模式是为了让创建工厂和一组产品与使用相分离，并可以随时切换到另一个工厂以及另一组产品；</br>
抽象工厂模式实现的关键点是定义工厂接口和产品接口，但如何实现工厂与产品本身需要留给具体的子类实现，客户端只和抽象工厂与抽象产品打交道。</br>
3.3 Builder模式是为了创建一个复杂的对象，需要多个步骤完成创建，或者需要多个零件组装的场景，且创建过程中可以灵活调用不同的步骤或组件。比如mybaties的SqlSessionFactoryBuilder</br>
3.4 原型模式Prototype 是根据一个现有对象实例复制出一个新的实例，复制出的类型和属性与原实例相同。clone </br>
3.5 适配器adapter 将一个类的接口转换成客户希望的另外一个接口 1、实现目标接口 2、内部持有一个待转换接口的引用 3、在目标接口的实现方法内部，调用待转换接口的方法 callable ->runnable
3.6 桥接模式bridge 使用桥接模式扩展一种新的品牌和新的核动力引擎，实际应用也非常少，但它提供的设计思想值得借鉴，即不要过度使用继承，而是优先拆分某些部件，使用组合的方式来扩展功能。
3.7 组合 Composite模式使得叶子对象和容器对象具有一致性
3.8 Decorator 装饰器模式 Java标准库中，InputStream是抽象类，FileInputStream、ServletInputStream、Socket.getInputStream()这些InputStream都是最终数据源
3.9 Facade 门面，外观，类似于builder
3.10 享元（Flyweight）如果一个对象实例一经创建就不可变，那么反复创建相同的实例就没有必要，直接向调用方返回一个共享的实例就行，这样即节省内存，又可以减少创建对象的过程，提高运行速度。比如Integer.valueOf()
3.11 责任链模式（Chain of Responsibility）是一种处理请求的模式，构成处理链路，依次处理，或者随机处理，比如filter
3.12 命令模式：将操作封装到对象内，以便存储，传递和返回，如：java.lang.Runnable
3.13 解释器模式 定义了一个语言的语法，然后解析相应语法的语句，如，java.text.Format
3.14 迭代器 Iterator 提供一种方法顺序访问一个聚合对象中的各个元素，而又不需要暴露该对象的内部表示
3.15 观察者模式（Observer）又称发布-订阅模式（Publish-Subscribe：Pub/Sub）
3.16 状态模式（State）经常用在带有状态的对象中，通过改变状态，来切换同一个借口的不同类。
3.17 策略模式：Strategy 核心思想是在一个计算方法中把容易变化的算法抽出来作为“策略”参数传进去，从而使得新增策略不必修改原有逻辑。
3.18 模板方法（Template Method）是一个比较简单的模式。它的主要思想是，定义一个操作的一系列步骤，对于某些暂时确定不下来的步骤，就留给子类去实现。 有抽象方法的抽象类就是模版方法
3.19 访问者模式（Visitor） 访问者模式是为了抽象出作用于一组复杂对象的操作，并且后续可以新增操作而不必对现有的对象结构做任何改动。

#### Redis
1、缓存穿透
缓存穿透是指查询一个一定不存在的数据。由于缓存不命中，并且出于容错考虑，如果从数据库查不到数据则不写入缓存，这将导致查询这个不存在的数据每次请求都要到数据库去查询。
1.1 缓存空数据
1.2 bloom filter （位数组 + hash）
2、缓存击穿
缓存击穿则是由于同时存在大量的查询请求去查询一个数据库中存在的数据，这往往发生在第一次请求缓存数据或者缓存数据到期的时候。在这个瞬间，并发请求特别多
2.1 后台job刷新
2.2 检查更新，把将缓存key的过期时间（绝对时间）一起保存到缓存中，如果缓存过期时间 - 当前系统时间 <= 1分钟（自定义的一个值），则主动更新缓存。
2.3 互斥锁 
3、缓存雪崩
3.1 缓存雪崩就是当数据未加载到缓存中，或者缓存同一时间大面积的失效，从而导致所有请求都去查数据库，导致数据库CPU和内存负载过高，甚至宕机。
4、热点数据集失效
为了避免这些热点数据集中的数据同时失效，导致大量访问请求数据库，我们可以在为每个热点数据设置缓存过期时间的时候，让他们的失效时间错开。
一个挤出时间之上加上或者减去一个范围内的随机值

redis: 
数据类型：string，list，set，sorted set，hash
数据结构：RAW EMBSTR INT HT字典 LINKEDLIST双端链表  ZIPLIST压缩列表  INTSET 整数集合 SKIPLIST跳跃表
redis 为啥这么快 1、完全基于内存 2、采用单线程 3、Redis中的数据结构是专门进行设计的，性能最高 4、使用多路I/O复用模型，非阻塞IO
1、redis 
主从，主节点断，人工升级 
哨兵 自动升级 写的压力都在主节点 心跳包、选举

Redis 的 Sentinel 系统用于管理多个 Redis 服务器（instance）， 该系统执行以下三个任务：

.监控（Monitoring）： Sentinel 会不断地检查你的主服务器和从服务器是否运作正常。
.提醒（Notification）： 当被监控的某个 Redis 服务器出现问题时， Sentinel 可以通过 API 向管理员或者其他应用程序发送通知。
.自动故障迁移（Automatic failover）： 当一个主服务器不能正常工作时， Sentinel 会开始一次自动故障迁移操作， 它会将失效主服务器的其中一个从服务器升级为新的主服务器， 并让失效主服务器的其他从服务器改为复制新的主服务器； 当客户端试图连接失效的主服务器时， 集群也会向客户端返回新主服务器的地址， 使得集群可以使用新主服务器代替失效服务器。

RDB的缺点:RDB 需要经常fork子进程来保存数据集到硬盘上,当数据集比较大的时候,fork的过程是非常耗时的,可能会导致Redis在一些毫秒级内不能响应客户端的请求.
RDB的优点:RDB是一个非常紧凑的文件,与AOF相比,在恢复大的数据集的时候，RDB方式会更快一些.

AOF优点：
1、文件有序地保存了对数据库执行的所有写入操作
2、AOF重写：Redis 可以在 AOF 文件体积变得过大时，自动地在后台对 AOF 进行重写

缺点：对于相同的数据集来说，AOF 文件的体积通常要大于 RDB 文件的体积。
    根据所使用的 fsync 策略，AOF 的速度可能会慢于 RDB 
Note: 因为以上提到的种种原因， 未来我们可能会将 AOF 和 RDB 整合成单个持久化模型。 

Redis 集群的数据分片
Redis 集群没有使用一致性hash, 而是引入了 哈希槽的概念.
 集群的主从复制模型
Redis 并不能保证数据的强一致性
Redis集群并不支持处理多个keys的命令,因为这需要在不同的节点间移动数据
至少得6台redis

分布式锁：
1. set NX EX 5s 不存在即设置 5s过期
2.存在的问题有，session1 加锁，5s没处理完，删除了，session2拿到了锁，session1处理完 把session2的锁删除掉了，所以只能删自己的锁



前面说过， Redis 的 Sentinel 中关于下线（down）有两个不同的概念：

主观下线（Subjectively Down， 简称 SDOWN）指的是单个 Sentinel 实例对服务器做出的下线判断。
客观下线（Objectively Down， 简称 ODOWN）指的是多个 Sentinel 实例在对同一个服务器做出 SDOWN 判断， 并且通过 SENTINEL is-master-down-by-addr 命令互相交流之后， 得出的服务器下线判断。 （一个 Sentinel 可以通过向另一个 Sentinel 发送 SENTINEL is-master-down-by-addr 命令来询问对方是否认为给定的服务器已下线。）

mongodb:
https://www.cnblogs.com/angle6-liu/p/10791875.html
面向文档的kv数据库，binary json，面向文件的 高性能 高可用性 易扩展性 丰富的查询语言 集合-文档-field 相当于表-列-字段，文档格式可以不一致。大数据、文档管理。分片。GridFS文件系统。没有使用传统的锁或者复杂的带回滚的事务。开发便捷起见,我们建议以非集群分片(unsharded)方式开始一个 mongodb 环境。
非结构化/半结构化的大数据，增加字段频繁，数据精准度要求不高，优先考虑nosql。
namespace 库名.集合名。
https://www.iteye.com/blog/mxdxm-2093603

ESK：
ELK=elasticsearch+Logstash+kibana
elasticsearch：后台分布式存储以及全文检索
logstash: 日志加工、“搬运工”
kibana：数据可视化展示。
ELK架构为数据分布式存储、可视化查询和日志解析创建了一个功能强大的管理链。 三者相互配合，取长补短，共同完成分布式大数据处理工作。

F5/Nginx 
F5:硬件负载均衡，一般都不管实际系统与应用的状态，而只是从网络层来判断，所以有时候系统处理能力已经不行了，但网络可能还来 得及反应
Nginx:基于系统与应用的负载均衡，能够更好地根据系统与应用的状况来分配负载。轻量。
限流算法：漏桶算法、令牌桶、guava、nginx都可以。

JVM: 
一、
Java虚拟机所管理的内存包括
1、程序计数器：线程私有，记录执行的字节码文件的行数。线程切换、循环、跳转、异常处理都会跳走，要记录行号以便下次执行。不会OOM
2、虚拟机栈：生命周期与方法相同，是执行方法的内存模型。每执行一个方法就会创建一个栈帧，把栈帧压入栈，成功返回或者抛出异常之后，栈帧就会出栈。会SOE(stackOverflowError)和OOM(outOfMemory)
    栈帧：栈帧存储方法的相关信息，包含局部变量数表(八种基本数据类型、对象引用)、返回值等，操作数栈、动态链接 
3、本地方法栈：native方法，会SOE和OOM
4、方法区：用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译后的代码等数据，会OOM
5、堆：对象实例，数组。
二、线程模型，见volatile
三、堆的划分
新生代：young generation minorGC  (Eden - survivor from  -survivor to 8:1:1)  ,JVM无法为一个新的对象分配内存的 Minor GC （Copinng算法）
老年代：old gen major GC （mark-compact），当eden区内存不足时触发  。
Full GC：清理整个堆空间，包括年轻代和老年代
Perm：jdk1.8中，Perm被替换成MetaSpace，存储类的元数据，也就是方法区。
https://mmbiz.qpic.cn/mmbiz_png/QCu849YTaIMvwdH7Zz5BtyMBHwLicG82ljTbIQ0EiaDUDDaTtZjicYOnuz2nIt9nVx8t15H0bRZZgZ54Sd4fvFHVg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1

GC垃圾回收：
可达性分析法root research：通过一系列“GC Roots”对象作为起点进行搜索
GC Roots包括：
（1） 虚拟机栈（栈帧中本地变量表）中引用的对象  
（2） 方法区中静态属性、常量引用的对象  
（3） 本地方法栈中Native方法引用的对象

回收步骤：1、可达性分析 2、finalize()方法已经被调用过

4、 方法区中的垃圾回收： 
（1） 常量池中一些常量、符号引用没有被引用，则会被清理出常量池  
（2） 无用的类：被判定为无用的类，会被清理出方法区。判定方法如下： 
A、 该类的所有实例被回收  
B、 加载该类的ClassLoader被回收  
C、 该类的Class对象没有被引用

常见的垃圾回收算法
1、Mark-Sweep（标记-清除算法）： 
2、Copying（复制清除算法）： 
3、Mark-Compact（标记-整理算法）： 

java对象的创建过程：
1、虚拟机遇到一条new指令时，首先检查这个指令的参数能否在常量池中定位到一个类的符号引用，并检查这个符号引用代表的   类是否已经加载、连接和初始化。如果没有，就执行该类的加载过程。
2、为对象分配内存，
A：如果内存是规整的，就用指针作为分界线，挪动指针、这种方式叫做 ”指针碰撞“。
B:如果不是连续的，虚拟机会维护一个列表，记录那些是可用的，然后找一个足够大的分配给实例。这个叫“空闲列表”。
C：使用那种要看Java堆是否规整，由所采用的垃圾收集器是否带有压缩整理（mark-compact）功能决定。 
3、设置对象，进行初始化，包括对象头（元数据、分代年龄、哈希吗、指向锁记录的指针、重量级锁指针、偏向锁线程ID等）

对象的定位办法：
1、句柄：句柄池：对象的实例数据+对象的类型数据
2、直接指针：引用对象地址。对象地址包括实例数据，自己再去引用类型数据。

两种方式各有优点： 
A、使用句柄访问的好处是引用中存放的是稳定的句柄地址，当对象被移动（比如说垃圾回收时移动对象），只会改变句柄中实例数据指针，而引用本身不会被修改。 
B、使用直接指针，节省了一次指针定位的时间开销。

引用：强软弱虚 软：第二次GC被回收，弱：第一次GC就会被回收

finalize()方法，被调用或者没覆盖，就会被GC

垃圾收集器：
serial
parNew
parallel Scavenge
serial old
parNew old
CMS(Concurrent Mark-Sweep)  
A、初始标记：标记GC Root能直接引用的对象  
B、并发标记：利用多线程对每个GC Root对象进行tracing搜索，在堆中查找其下所有能关联到的对象。  
C、重新标记：为了修正并发标记期间，用户程序继续运作而导致标志产生变动的那一部分对象的标记记录。  
D、并发清除：利用多个线程对标记的对象进行清除

cpu敏感，浮动垃圾，有碎片导致full GC

JVM优化：
1、设置合理的eden区，survivor区及使用率，一般8:1:1， -Xmn
2、占用内存比较多的大对象，一般会选择在老年代分配内存。XX:PetenureSizeThreshold=1000000，单位为B，标明对象大小超过1M时，在老年代(tenured)分配内存空间。
3、年轻对象放在eden区，当第一次GC后，如果对象还存活，放到survivor区，此后，每GC一次，年龄增加1。当对象的年龄达到阈值，就被放到tenured老年区。这个阈值可以同构-XX:MaxTenuringThreshold设置。如果想让对象留在年轻代，可以设置比较大的阈值。
4、设置最小堆和最大堆：-Xmx和-Xms，系统在运行时堆大小理论上是恒定的，稳定的堆空间可以减少GC次数，因此，很多服务端都会将这两个参数设置为一样的数值。
5、一个不稳定的堆并非毫无用处。在系统不需要使用大内存的时候，压缩堆空间，使得GC每次应对一个较小的堆空间，加快单次GC次数。基于这种考虑，JVM提供两个参数，用于压缩和扩展堆空间。 
（1）-XX:MinHeapFreeRatio 参数用于设置堆空间的最小空闲比率。默认值是40，当堆空间的空闲内存比率小于40，JVM便会扩展堆空间  
（2）-XX:MaxHeapFreeRatio 参数用于设置堆空间的最大空闲比率。默认值是70， 当堆空间的空闲内存比率大于70，JVM便会压缩堆空间。 
（3）当-Xmx和-Xmx相等时，上面两个参数无效
6、通过增大吞吐量提高系统性能，可以通过设置并行垃圾回收收集器。 
（1）-XX:+UseParallelGC:年轻代使用并行垃圾回收收集器。这是一个关注吞吐量的收集器，可以尽可能的减少垃圾回收时间。 
（2）-XX:+UseParallelOldGC:设置老年代使用并行垃圾回收收集器。


7、尝试使用大的内存分页：使用大的内存分页增加CPU的内存寻址能力，从而系统的性能。-XX:+LargePageSizeInBytes 设置内存页的大小

8、使用非占用的垃圾收集器。-XX:+UseConcMarkSweepGC老年代使用CMS收集器降低停顿。

9、-XXSurvivorRatio=3，表示年轻代中的分配比率：survivor:eden = 2:3

类加载：
一、 概念：类加载器把class文件中的二进制数据读入到内存中，存放在方法区，然后在堆区创建一个java.lang.Class对象，用来封装类在方法区内的数据结构。类加载的步骤如下：

1、加载：查找并加载类的二进制数据（把class文件里面的信息加载到内存里面）

2、连接：把内存中类的二进制数据合并到虚拟机的运行时环境中  
（1）验证：确保被加载的类的正确性。包括：

A、类文件的结构检查：检查是否满足Java类文件的固定格式  
  B、语义检查：确保类本身符合Java的语法规范  
  C、字节码验证：确保字节码流可以被Java虚拟机安全的执行。字节码流是操作码组成的序列。每一个操作码后面都会跟着一个或者多个操作数。字节码检查这个步骤会检查每一个操作码是否合法。 
  D、二进制兼容性验证：确保相互引用的类之间是协调一致的。

（2）准备：为类的静态变量分配内存，并将其初始化为默认值  
（3）解析：把类中的符号引用转化为直接引用（比如说方法的符号引用，是有方法名和相关描述符组成，在解析阶段，JVM把符号引用替换成一个指针，这个指针就是直接引用，它指向该类的该方法在方法区中的内存位置）

3、初始化：为类的静态变量赋予正确的初始值。当静态变量的等号右边的值是一个常量表达式时，不会调用static代码块进行初始化。只有等号右边的值是一个运行时运算出来的值，才会调用static初始化。

二、双亲委派模型：

1、当一个类加载器收到类加载请求的时候，它首先不会自己去加载这个类的信息，而是把该  


mysql:

1. InnoDB 支持事务，MyISAM 不支持事务。这是 MySQL 将默认存储引擎从 MyISAM 变成 InnoDB 的重要原因之一；
2. InnoDB 支持外键，而 MyISAM 不支持。对一个包含外键的 InnoDB 表转为 MYISAM 会失败； 
3. InnoDB 是聚集索引，MyISAM 是非聚集索引。聚簇索引的文件存放在主键索引的叶子节点上，因此 InnoDB 必须要有主键，通过主键索引效率很高。但是辅助索引需要两次查询，先查询到主键，然后再通过主键查询到数据。因此，主键不应该过大，因为主键太大，其他索引也都会很大。而 MyISAM 是非聚集索引，数据文件是分离的，索引保存的是数据文件的指针。主键索引和辅助索引是独立的。
4. InnoDB 不保存表的具体行数，执行 select count(*) from table 时需要全表扫描。而MyISAM 用一个变量保存了整个表的行数，执行上述语句时只需要读出该变量即可，速度很快；    
5. InnoDB 最小的锁粒度是行锁，MyISAM 最小的锁粒度是表锁。一个更新语句会锁住整张表，导致其他查询和更新都会被阻塞，因此并发访问受限。

一、为什么B+树比B树更适合做索引 

概念：B+树：非叶子节点只存储索引和指向，不存储具体信息，所有的节点信息都在叶子节点上。B树：非叶子节点也保存节点信息。
1、磁盘IO读写次数相比B树降低了，一次IO读更多的索引数据
  在B+树中，其非叶子的内部节点都变成了key值，因此其内部节点相对B 树更小。如果把所有同一内部节点的key存放在同一盘块中，那么盘块所能容纳的key数量也越多。一次性读内存中的需要查找的key值也就越多。相对来说IO读写次数也就降低了。
2、每次查询的时间复杂度是稳定的，稳定很重要
  在B+树中，由于分支节点只是叶子节点的索引，所以对于任意关键字的查找都必须从根节点走到分支节点，所有关键字查询路径长度相同，每次查询的时间复杂度是固定的。但是在B树中，其分支节点上也保存有数据，对于每一个数据的查询所走的路径长度是不一样的，所以查询效率也不一样。
3、遍历效率更高 叶子节点遍历效率高
  由于B+树的数据都存储在叶子节点上，分支节点均为索引，方便扫库，只需扫一遍叶子即可。但是B树在分支节点上都保存着数据，要找到具体的顺序数据，需要执行一次中序遍历来查找。所以B+树更加适合范围查询的情况，在解决磁盘IO性能的同时解决了B树元素遍历效率低下的问题。

哈希索引，等于的时候比较有优势

一致性hash:加机器时，重新计算缓存，导致大面积瘫痪。改成环-找下一个，或者虚拟节点。

表分区：
表分区，是指根据一定规则，将数据库中的一张表分解成多个更小的，容易管理的部分。从逻辑上看，只有一张表，但是底层却是由多个物理分区组成。

RANGE分区： 这种模式允许将数据划分不同范围。例如可以将一个表通过年份划分成若干个分区

LIST分区： 这种模式允许系统通过预定义的列表的值来对数据进行分割。按照List中的值分区，与RANGE的区别是，range分区的区间范围值是连续的。

HASH分区 ：这中模式允许通过对表的一个或多个列的Hash Key进行计算，最后通过这个Hash码不同数值对应的数据区域进行分区。例如可以建立一个对表主键进行分区的表。

KEY分区 ：上面Hash模式的一种延伸，这里的Hash Key是MySQL系统产生的。

MVCC:读快照，为了
Ta   a = 0   Tb
a=1          
            select a // x
commit
            select a // y
            commit
                          x   y
read uncommit(脏读)       1   1
read commit（幻读）        0   1   mvcc读快照每查一次读一次
repeated read（不可重复读） 0   0   mvcc读快照是在事物开始第一次
serializable（序列化）     0   0

MyiSAM  表锁
Innodb  表锁  行锁  
表锁开销小，加锁快，不会出现死锁，锁粒度大，并发低
行锁开销大，加锁慢，会出现死锁，锁粒度小，并发低
页锁介于两者之间

表锁更适用于以查询为主，只有少量按索引条件更新数据的应用；
行锁更适用于有大量按索引条件并发更新少量不同数据，同时又有并发查询的应用。


https://blog.csdn.net/mysteryhaohao/article/details/51669741

MyISAM在执行查询语句（SELECT）前，会自动给涉及的所有表加读锁，在执行更新操作（UPDATE、DELETE、INSERT等）前，会自动给涉及的表加写锁。
对MyISAM表的读操作，不会阻塞其他用户对同一表的读请求，但会阻塞对同一表的写请求；对 MyISAM表的写操作，则会阻塞其他用户对同一表的读和写操作
session1 select tableA ,session2 也可以select ,但是不能写
session1 update insert delete tableA,session2即不能读，也不能写

LOCK TABLES给表显式加表锁时，必须同时取得所有涉及到表的锁。比如lock tableA之后，不能查没有加锁的tableB

InnoDb ：
行锁：只有通过索引条件检索数据，InnoDB才使用行级锁，否则，InnoDB将使用表锁！
LOCK IN SHARE MODE 共享锁 session1 加锁，session1也可以加，俩锁上去之后都不能更新
for update 排他锁 session1 加锁，session1不能加

行锁是针对索引加的锁，不是针对记录加的锁，所以虽然是访问不同行的记录，但是如果是使用相同的索引键，是会出现锁冲突的
间隙锁，条件不存在也能加锁，id=101 不存在，session1 select 1d=101 for update ,session2 select 1d=101 for update 阻塞。

什么时候使用表锁
对于InnoDB表，在绝大部分情况下都应该使用行级锁，因为事务和行锁往往是我们之所以选择InnoDB表的理由。但在个别特殊事务中，也可以考虑使用表级锁。
第一种情况是：事务需要更新大部分或全部数据，表又比较大，如果使用默认的行锁，不仅这个事务执行效率低，而且可能造成其他事务长时间锁等待和锁冲突，这种情况下可以考虑使用表锁来提高该事务的执行速度。
第二种情况是：事务涉及多个表，比较复杂，很可能引起死锁，造成大量事务回滚。这种情况也可以考虑一次性锁定事务涉及的表，从而避免死锁、减少数据库因事务回滚带来的开销。

SET AUTOCOMMIT=0;
LOCK TABLES t1 WRITE, t2 READ, ...;
[do something with tables t1 and t2 here];
COMMIT;
UNLOCK TABLES;
先commit 再unlock

数据库死锁 1、访问表的顺序不一致导致
session1        session2

select tablea   select tableb   （2、insert id =2 也会导致）   
where id = 1    where id = 1
for update      for update

select tableb
where id = 1
for update （insert id =2 也会导致）   
// 等待
                select tablea where ID=1 for update //死锁

（3）前面讲过，在REPEATABLE-READ隔离级别下，如果两个线程同时对相同条件记录用SELECT...FOR UPDATE加排他锁，在没有符合该条件记录情况下，两个线程都会加锁成功。程序发现记录尚不存在，就试图插入一条新记录，如果两个线程都这么做，就会出现死锁。这种情况下，将隔离级别改成READ COMMITTED，就可避免问题，如下所示。

SELECT ... FOR UPDATE 走的是IX锁(意向排它锁) sessionA 加了排他锁，sessionB不能加排他锁，但是可以读快照
SELECT ... LOCK IN SHARE MODE走的是IS锁(意向共享锁)，sessionA 加了共享锁，sessionB 也可以加共享锁，但是无法操作，直到sessionA 完成操作。否则锁等待超时
间隙锁，左开右闭，为了解决Repeatable read 幻读的问题，小心会造成死锁
使用普通索引锁定；
使用多列唯一索引；
使用唯一索引锁定多行记录。

https://www.jianshu.com/p/32904ee07e56

事务日志包括：重做日志redo 和 回滚日志undo
redo log（重做日志） 实现持久化和原子性 事务开启之后，先会把 redo log buffer --> os buffer --> redo log file，俗称日志先行，然后buffer pool中的数据文件才会持久化到disk.
如果此时数据库崩溃了，就可以根据redo log来恢复到崩溃前的一个状态，是选择继续执行还是回滚，取决于恢复策略。
undo log（回滚日志） 实现一致性 insert 记录一个delete，反之依然，update a->b 记录 b->a ，为了回滚
日志的种类：错误日志、查询日志、慢查询日志、redo undo日志 二进制日志：记录所有的更改 中继日志：用来跟slave库恢复

####
mybaits
一级缓存，同一个事务内，同一个session,默认开启。




分布式配置中心
cfg4j, disconf，spring cloud config


docker
共享操作系统
docker build run pull
NameSpace 控制隔离
Control groups 限制资源访问


mybaits:
执行流程
https://mmbiz.qpic.cn/mmbiz_png/icu8ekKAcwiaZ3ofMoCfPuefIGE58NEm1mRKQS0ibgGJS3GLEIBsCPowMs6sZS7ibdatIv3ZOdAW48JI2p0LT4y60g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1

1、
InputStream inputStream = Resources.getResourceAsStream("mybaits.xml");
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
-->解析mybaits.xml文件，XMLConfigBuilder--> 都加载到 configuration里，比如 db ip，mapper接口
2、sqlSession.getMapper(UserMapper.class);
configuration-->MapperRegistry --> Map<Class<?>, MapperProxyFactory<?>> knownMappers, 返回一个 MapperProxy
3、mapper.select










dubbo、 zk、 mysql、 mybaties，spring设计模式、 系统设计、 jvm、限流，熔断，降级

负载均衡算法:权重随机、权重轮训、一致性hash、最小活跃数。














