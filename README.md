# 技能树
### 集合
集合.1-[collection接口(list set queue)](https://www.cnblogs.com/nayitian/p/3266090.html)

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
3.3 Builder模式是为了创建一个复杂的对象，需要多个步骤完成创建，或者需要多个零件组装的场景，且创建过程中可以灵活调用不同的步骤或组件。</br>
3.4 原型模式Prototype 是根据一个现有对象实例复制出一个新的实例，复制出的类型和属性与原实例相同。</br>
3.5 适配器adapter 将一个类的接口转换成客户希望的另外一个接口 1、实现目标接口 2、内部持有一个待转换接口的引用 3、在目标接口的实现方法内部，调用待转换接口的方法 callable ->runnable
3.6 桥接模式bridge 使用桥接模式扩展一种新的品牌和新的核动力引擎，实际应用也非常少，但它提供的设计思想值得借鉴，即不要过度使用继承，而是优先拆分某些部件，使用组合的方式来扩展功能。
3.7 组合 Composite模式使得叶子对象和容器对象具有一致性
3.8 Decorator 装饰器模式
3.9 Facade 门面，外观，类似于builder
3.10 享元（Flyweight）如果一个对象实例一经创建就不可变，那么反复创建相同的实例就没有必要，直接向调用方返回一个共享的实例就行，这样即节省内存，又可以减少创建对象的过程，提高运行速度。比如Integer.valueOf()
3.11 责任链模式（Chain of Responsibility）是一种处理请求的模式，构成处理链路，依次处理，或者随机处理，比如filter
3.12 命令模式：将操作封装到对象内，以便存储，传递和返回，如：java.lang.Runnable
3.13 解释器模式 定义了一个语言的语法，然后解析相应语法的语句，如，java.text.Format
3.14 迭代器 Iterator 提供一种方法顺序访问一个聚合对象中的各个元素，而又不需要暴露该对象的内部表示
3.15 观察者模式（Observer）又称发布-订阅模式（Publish-Subscribe：Pub/Sub）
3.16 状态模式（State）经常用在带有状态的对象中，通过改变状态，来切换同一个借口的不同类。
3.17 策略模式：Strategy 核心思想是在一个计算方法中把容易变化的算法抽出来作为“策略”参数传进去，从而使得新增策略不必修改原有逻辑。
3.18 模板方法（Template Method）是一个比较简单的模式。它的主要思想是，定义一个操作的一系列步骤，对于某些暂时确定不下来的步骤，就留给子类去实现。
3.19 访问者模式（Visitor） 访问者模式是为了抽象出作用于一组复杂对象的操作，并且后续可以新增操作而不必对现有的对象结构做任何改动。












