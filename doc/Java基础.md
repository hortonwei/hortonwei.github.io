# Java基础

## 对面向对象三大特征

**封装**
把抽象出的属性和方法封装在一起，数据被保护在内部，只有通过被授权的方法才能对数据进行操作；
好处：调用者可以直接使用提供的方法进行操作，不用关心其实现细节；
**继承**
子类不需要重新定义，就可以使用父类的属性和方法；
好处：提高复用性、扩展性；
**多态**
指一个类实例的相同方法在不同情形有不同表现形式；
格式：父类类名 引用名称 = new 子类类名();
条件：子类必须继承父类；子类必须重写父类的方法；父类引用指向子类对象；
好处：降低代码耦合度，减少冗余代码；
案例：方法传参类型是list而不是arraylist

## 访问修饰符和访问权限

public：可以被所有类所访问；（外部类）
protected：同一包中的类+不同包下的子类可访问；（本包+子类）能防止不同包下的非子类访问
默认：同一包中的类可以访问；（本包）
private：只能被自己访问；（自己）

## equals 和 == 区别

1、== 对于基本类型来说是值比较，对于引用类型来说是比较的是引用地址；
2、equals默认：比较的是引用地址；重写equals方法：比较对象的属性值是否相等；

## 为什么重写equals时必须重写hashCode方法

影响hashset、hashmap的去重；
只重写equals方法，Set进行去重操作时，会先判断两个对象的hashCode是否相同，此时因为没有重写hashCode方法，所以会直接执行Object中的hashCode方法，而Object中的hashCode方法对比的是两个不同引用地址的对象，所以结果是false，判定对象不相等；

<img src="https://s2.loli.net/2023/03/04/hORycbkq42dTi39.webp" alt="16af8aa4340aaf42.jpg" align=left style="zoom:67%;" />

## 集合

<center class="half">
<img src="https://s2.loli.net/2023/03/04/CiSB87ZRa49ncWE.png" zoom = "25%" align=left />
<img src="https://s2.loli.net/2023/03/04/WdHq7orYFeUADp8.png" zoom = "25%" align=left />
<center/>











Collection：单列集合
Map：双列集合（key value成双成对存储）
List：元素有序可重复
Set：元素无序不可重复

### ArrayList和LinkedList的区别

1、ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构；
2、ArrayList支持随机访问（通过index下标访问），LinkedList不支持随机访问，支持顺序访问；
3、ArrayList头部或中间插入会移动数据，或遇到扩容，性能会降低；LinkedList头尾插入性能高，中间插入需要先找到位置，性能会降低；
4、arrayList受益于局部性原理，使用连续内存存储数据，CPU缓存会读取相邻数据一次性加载到缓存中，读取速度更快；链表，不是连续内存，不能放入缓存；
5、遍历方式；ArrayList用普通for循环，LinkedList用迭代器遍历；

<img src="https://s2.loli.net/2023/03/04/C7Xe58vkdwDsR4f.jpg" alt="v2-894d2f03f6672c9b6a1ced07fe27e1be_r.jpg" style="zoom: 33%;" />
结构：数组+链表+红黑树
1、哈希算法算出key的数组下标；
2、如果数组下标位置元素为空，则将key和value封装后存入；
3、如果数组下标位置元素不为空（哈希碰撞），先判断该位置的节点类型是链表还是红黑树；
4、如果是链表则需要遍历该节点下的链表，如果发现key相同，则更新value，如果key不相同，则插入链表末尾，如果链表长度大于8则转为红黑树；
如果是红黑树则会判断红⿊树中是否存在当前key，如果存在则更新value，如果不存在则放到对应的位置；
5、判断是否需要进⾏扩容，如果需要就扩容，如果不需要就结束；

### foreach和for和迭代器遍历的区别

foreach用于遍历数组结构的集合时会转为普通for循环；
foreach用于遍历链表结构的集合时会转为迭代器遍历；

### 线程安全的list

vector
CopyOnWriteArrayList
CopyOnWriteArrayList使用了一种叫写时复制的方法，当有新元素添加到CopyOnWriteArrayList时，先从原有的数组中拷贝一份出来，然后在新的数组做写操作，写完之后，再将原来的数组引用指向到新数组。

### 线程安全的HashMap

HashTable，HashTable和HashMap的实现原理几乎一样，差别是HashTable不允许key和value为null，HashTable是线程安全的，但是HashTable线程安全的策略实现代价却太大了，简单粗暴，get/put所有相关操作都是synchronized的。

## 多线程

### 创建多线程的方式

**继承Thread类**；重写run方法，创建对象调用start方法启动线程；
**实现Runnable接口**；实现run方法，创建实现类对象，调用start方法启动线程；
优点：
1、类是单继承多实现的；
2、适合数据共享，如果线程中有一个成员变量，使用Runnable创建的线程，多个线程之间可以共同访问这个变量，可以多创建几个Thread类，把Runnale作为参数，让多个线程一起运行；Thread创建的线程，每次都是new的一个新的线程对象；
**通过Callable和Future创建线程**；定义一个Callable的实现类，并实现call方法；
优点：有返回值

### 线程状态

<img src="https://s2.loli.net/2023/03/04/tmyKIMkWnhqV6dD.png" alt="图片1.png" style="zoom:33%;" />

**新建**(New) ：线程对象被创建后，就进入了新建状态。例如，Thread thread = new Thread()；
**就绪**(Runnable): 线程对象被创建后，其它线程调用了该对象的start()方法，从而来启动该线程；
**运行**(Running)：线程获取CPU权限进行执行。需要注意的是，线程只能从就绪状态进入到运行状态。
**阻塞**(Blocked)：线程放弃CPU使用权，暂时停止运行。直到线程进入就绪状态，才有机会转到运行状态。阻塞的情况分三种：
等待阻塞 -- 通过调用线程的wait()方法，让线程等待某工作的完成。
同步阻塞 -- 线程在获取synchronized同步锁失败(因为锁被其它线程所占用)，它会进入同步阻塞状态。
其他阻塞 -- 通过调用线程的sleep()或join()或发出了I/O请求时，线程会进入到阻塞状态。当sleep()状态超时、join(）等待线程终止或者超时、或者I/O处理完毕时，线程重新转入就绪状态。
**死亡**(Dead)：线程执行完了或者因异常退出了run()方法，该线程结束生命周期。

### 线程池

线程池就是事先将多个线程对象放到一个容器中，当使用的时候就不用 new 线程而是直接去池中拿线程即可；
降低资源消耗，提高响应速度，提高线程的可管理性。
java.util.concurrent.Executors提供5种创建线程池的方式：newCachedThreadPool，newFixedThreadPool，newSingleThreadExecutor，newScheduleThreadPool，newWorkStealingPool(jdk1.8)
通过Executors以静态方法的方式直接调用，实质上是它们最终调用的是ThreadPoolExecutor的构造方法

阿里巴巴java开发手册
【强制】线程池不允许使用Executors去创建，而是通过ThreadPoolExecutor的方式，这样
的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险。
说明：Executors返回的线程池对象的繁端如下：
1） FixedThreadPool 和 SingleThreadPooL
允许的请求队列长度为Integer.MAX_VALUE，可能会堆积大量的请求，从而导致ooM
2） CachedThreadPool 不 ScheduledThreadPool
允许的创建线程数量为Integer.MAX_VALUE，可能会创建大量的线程，从而导致00M

### ThreadPoolExecutor

corePoolSize：阿里建议 int NUMBER_OF_CORES = Runtime.getRuntime().availableProcessors();
maximumPoolSize：线程池中的最大线程数。表示线程池中最多可以创建多少个线程，很多人以为它的作用是这样的：“当线程池中的任务数超过 corePoolSize 后，线程池会继续创建线程，直到线程池中的线程数小于maximumPoolSize”。其实这种理解是完全错误的，它真正的作用是：当线程池中的线程数等于 corePoolSize 并且 workQueue 已满，这时就要看当前线程数是否大于 maximumPoolSize，如果小于maximumPoolSize 定义的值，则会继续创建线程去执行任务， 否则将会调用去相应的任务拒绝策略来拒绝这个任务。另外超过 corePoolSize的线程被称做"Idle Thread", 这部分线程会有一个最大空闲存活时间(keepAliveTime)，如果超过这个空闲存活时间还没有任务被分配，则会将这部分线程进行回收。
workQueue BlockingQueue<Runnable>
阻塞队列。如果当前线程池中的线程数目>=corePoolSize，则每来一个任务，会尝试将其添加到该队列当中，注意只要超过了 corePoolSize 就会把任务添加到该缓存队列，添加可能成功也可能不成功，如果成功的话就会等待空闲线程去执行该任务，若添加失败(一般是队列已满)，就会根据当前线程池的状态决定如何处理该任务(若线程数 < maximumPoolSize 则新建线程；若线程数 >= maximumPoolSize,则会根据拒绝策略做具体处理)。

### 阻塞队列

<img src="https://s2.loli.net/2023/03/04/owPHpJxM47BGkvf.jpg" alt="Snipaste_2021-05-07_09-35-13.jpg" style="zoom:50%;" />当阻塞队列是空时，从队列中获取元素的操作将会被阻塞。当阻塞队列是满时，往队列里添加元素的操作将会被阻塞。

<img src="https://s2.loli.net/2023/03/04/KWqulVkPm4eAi3w.jpg" alt="Snipaste_2021-05-07_09-35-13 _1_.jpg" style="zoom: 50%;" />
BlockingQueue实现类有7个，其中3个比较重要，是线程池的参数。
ArrayBlockingQueue:由数组结构组成的有界阻塞队列。
LinkedBlockingQueue :由链表结构组成的有界(但大小默认值为Integer.MAX_ VALUE)
PriorityBlockingQueue:支持优先级排序的无界阻塞队列。
DelayQueue:使用优先级队列实现的延迟无界阻塞队列。
SynchronousQueue:不存储元素的阻塞队列，也即单个元素的队列。
LinkedTransferQueue:由链表结构组成的无界阻塞队列。
LinkedBlockingDeque:由链表结构组成的双向阻塞队列。

### 线程池最大线程数设置多少

一、CPU密集型
CPU核数 + 1；Runtime.getRuntime().availableProcessors()
二、I0密集型
（1）由于IO密集型任务线程并不是一直在执行任务，则应配置尽可能多的线程，如CPU核数*2
（2）I0密集型，即该任务需要大量的IO，即大量的阻塞。在单线程上运行I0密集型的任务会导致浪费大量的CPU运算能力浪费在等待。
所以在I0密集型任务中使用多线程可以大大的加速程序运行，即使在单核CPU上，这种加速主要就是利用了被浪费掉的阻塞时间。
I0密集型时，大部分线程都阻塞，故需要多配置线程数: 参考公式: CPU核数 / （1- 阻塞系数）
阻塞系数在0.8~0.9之间
比如8核CPU: 8 / （1 - 0.9）= 80个线程数

典型执行步骤：
1当调度进程送了一个新的任务进入线程池时，会检查核心线程池中是否有空闲的线程，如果有直接拿来用，如果没有进入步骤二；
2检查一下核心线程池中的线程数量是否小于corePoolSize，如果是则建一个新线程放入核心线程池中，并拿来用，如果已经满了进入步骤三；
3检查一下阻塞队列是不是满了，如果没有，将新任务放到队尾，排队等待核心线程池来执行，如果满了进入步骤四；
4检查一下非核心线程池是否有空闲线程，如果有直接拿来用，如果没有进入步骤五；
5检查一下，全部线程数是否小于maximumPoolSiz，如果是，创建一个带有过期时间的新线程放入线程池并拿来用，如果满了，恭喜你是最倒霉的一个任务。
常见的阻塞队列有4种：
ArrayBlockingQueue
基于数组的先进先出队列，此队列创建时必须指定大小。使用这个队列时，遵从上边典型的5步过程。
LinkedBlockingQueue
基于链表的先进先出队列，如果创建时没有指定此队列大小，则默认为Integer.MAX_VALUE；
synchronousQueue
直接拒绝队列，也就是任何任务都不会被放到队列里去排队。既然没有队列，那么所有提交给池子的任务，都会被直接交给线程去执行。先找核心线程，再找非核心线程。但是，这是会有一个关键的问题：如果池子里没有空闲的线程是不是要抛异常了？所以，当我们选择使用SynchronousQueue时，通常会将maximumPoolSize指定Integer.MAX_VALUE，即无限大。但是此时要注意，当瞬时并发量暴增时，池子中的线程数会非常大。此时，如果你不想使用队列，又不想让线程数失控，可以指定maximumPoolSize，并面对新任务执行失败的风险。
DelayQueue
延时队列，进入该队列的任务必须实现Delayed接口。进入了这个队列的任务，只有达到了指定的延时时间，才会被执行。
threadFactory
handler
四种拒绝策略
下面，我们再来看一下属于倒霉蛋的礼物--拒绝策略。
AbortPolicy 默认的拒绝策略：
直接拒绝新任务进入线程池，并丢弃该任务，同时RejectedExecutionException。
适合并发量受限，且比较重要的业务场景，可以通过抛出异常的情况，对被丢弃的任务做出实弹的处理。
CallerRunsPolicy：
线程池让调度这条任务的线程自己去干。注意哦，并发激增的话，线程数量也会激增。
适合比较重要的，必须被执行的任务。
DiscardPolicy：
对拒绝任务直接无声抛弃，没有异常信息。
适合不重要的业务场景做兜底方案，线程数量可以控制。
DiscardOldestPolicy：
丢弃队列头部的任务，将新任务放入队尾。

![2e2eb9389b504fc228b597b5bf27fb1492ef6d93.jpeg](https://s2.loli.net/2023/03/04/IH1iEehvk46yG3B.jpg)


### 为什么i++不是原子性操作

### j = i是不是原子操作

### volatile关键字

volatile变量是一种比synchronized关键字更轻量级的同步机制

### 并发三要素

**可见性**

是指线程之间的可见性，一个线程修改的状态对另一个线程是可见的。也就是一个线程修改的结果。另一个线程马上就能看到。比如：用volatile修饰的变量，就会具有可见性。volatile修饰的变量不允许线程内部缓存和重排序，即直接修改内存。所以对其他线程是可见的。但是这里需要注意一个问题，volatile只能让被他修饰内容具有可见性，但不能保证它具有原子性。比如 volatile int a = 0；之后有一个操作 a++；这个变量a具有可见性，但是a++ 依然是一个非原子操作，也就是这个操作同样存在线程安全问题。
在 Java 中 volatile、synchronized 和 final 实现可见性。

**原子性**

原子是世界上的最小单位，具有不可分割性。比如 a=0；（a非long和double类型） 这个操作是不可分割的，那么我们说这个操作时原子操作。再比如：a++； 这个操作实际是a = a + 1；是可分割的，所以他不是一个原子操作。非原子操作都会存在线程安全问题，需要我们使用同步技术（synchronized）来让它变成一个原子操作。一个操作是原子操作，那么我们称它具有原子性。java的concurrent包下提供了一些原子类，我们可以通过阅读API来了解这些原子类的用法。比如：AtomicInteger、AtomicLong、AtomicReference等。
在 Java 中 synchronized 和在 lock、unlock 中操作保证原子性。

**有序性**

Java 语言提供了 volatile 和 synchronized 两个关键字来保证线程之间操作的有序性，volatile 是因为其本身包含“禁止指令重排序”的语义，synchronized 是由“一个变量在同一个时刻只允许一条线程对其进行 lock 操作”这条规则获得的，此规则决定了持有同一个对象锁的两个同步块只能串行执行。

### Volatile原理

Java语言提供了一种稍弱的同步机制，即volatile变量，用来确保将变量的更新操作通知到其他线程。当把变量声明为volatile类型后，编译器与运行时都会注意到这个变量是共享的，因此不会将该变量上的操作与其他内存操作一起重排序。volatile变量不会被缓存在寄存器或者对其他处理器不可见的地方（CPU Cache），因此在读取volatile类型的变量时总会返回最新写入的值。（保证读操作时，前序所有对该变量的写操作已生效（写回主存））
在访问volatile变量时不会执行加锁操作，因此也就不会使线程阻塞，因此volatile变量是一种比synchronized关键字更轻量级的同步机制。
synchronized和Lock能够保证可见性的原因是，线程在释放锁之前，会把共享变量值都刷回主存

<img src="https://s2.loli.net/2023/03/04/vlke9XYWmZdSEh4.png" alt="731716-20160708224602686-2141387366.png" style="zoom:50%;" />
当对非 volatile 变量进行读写的时候，每个线程先从内存拷贝变量到CPU缓存中。如果计算机有多个CPU，每个线程可能在不同的CPU上被处理，这意味着每个线程可以拷贝到不同的CPU cache中。而声明变量是 volatile 的，JVM 保证了每次读变量都从内存中读，跳过CPU cache这一步。
Java内存模型规定所有的变量都是存在主存当中，每个线程都有自己的工作内存。线程对变量的所有操作都必须在工作内存中进行，而不能直接对主存进行操作。并且每个线程不能访问其他线程的工作内存。
当一个变量定义为 volatile 之后，将具备两种特性：
1.保证此变量对所有的线程的可见性，这里的“可见性”，如本文开头所述，当一个线程修改了这个变量的值，volatile 保证了新值能立即同步到主内存，以及每次使用前立即从主内存刷新。但普通变量做不到这点，普通变量的值在线程间传递均需要通过CPU内存（详见：Java内存模型）来完成。
2.禁止指令重排序优化。有volatile修饰的变量，赋值后多执行了一个“load addl $0x0, (%esp)”操作，这个操作相当于一个内存屏障（指令重排序时不能把后面的指令重排序到内存屏障之前的位置），只有一个CPU访问内存时，并不需要内存屏障；（什么是指令重排序：是指CPU采用了允许将多条指令不按程序规定的顺序分开发送给各相应电路单元处理）。
volatile的读性能消耗与普通变量几乎相同，但是写操作稍慢，因为它需要在本地代码中插入许多内存屏障指令来保证处理器不发生乱序执行。

### 如何保证原子性

atomic类 CAS机制 unsafe类
CAS：CAS机制中使用了3个基本操作数：内存地址V，预期值A，要修改的新值B。
更新一个变量的时候，只有当变量的预期值A和内存地址V当中的实际值相同时，才会将内存地址V对应的值修改为B。
CAS缺点
（1）CPU开销较大
在并发量比较高的情况下，如果许多线程反复尝试更新某一个变量，却又一直更新不成功，循环往复，会给CPU带来很大的压力。
（2）ABA问题：CAS算法实现一个重要前提需要取出内存中某时刻的数据并在当下时刻比较并替换，那么在这个时间差类会导致数据的变化。
比如说一个线程one从内存位置V中取出A，这时候另一个线程two也从内存中取出A，并且我程two进行了- -些操作将值变成了B，
然后线程two又将V位置的数据变成A，这时候线程one进行CAS操作发现内存中仍然是A，然后线程one操作成功。尽管线程one的CAS操作成功，但是不代表这个过程就是没有问题的。
ABA问题解决：用带时间戳的原子引用类AtomicstampedReference；类似乐观锁，修改变量需要更新时间戳

### CocurrentHashMap如何实现线程安全和高性能

从Java提供的线程安全类的源码来看，实现高效并发的方式有：
1.对可变属性使用volatile修饰
2.get方法不加synchronized关键字
3.涉及到变量的所有修改操作，对需要操作的变量使用synchronized关键字进行同步
