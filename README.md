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



