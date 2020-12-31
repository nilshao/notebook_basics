# 第二章 第四节 死锁

## 死锁的概念

### 死锁的概念

在并发环境下，各个进程因为竞争资源而造成的一种**互相等待**对方手里的资源，导致各个进程**都阻塞**，都无法向前推进的现象，就是死锁。发生死锁后若无外力干涉，这些进程都将无法推进。

### 死锁，饥饿，死循环

死锁：“死锁概念”，至少是两个或以上的进程才能发生死锁，一定是在阻塞态

饥饿：可以是只有一个进程发生饥饿，长时间得不到资源，可能是在阻塞态，可能是在就绪态

死循环：程序执行过程中一直跳不出某个循环的现象。

### 死锁产生的必要条件

产生死锁必须同时满足以下四个条件：

1. 互斥条件：只有必须对互斥使用的资源争抢才会导致死锁

2. 不剥夺条件：进程所获得的资源在未使用完之前，不能由其他进程强行夺走，只能主动释放

3. 请求和保持条件：进程已经保持了至少一个资源，但又提出了新的资源请求，而该资源又被其他进程占有，此时请求进程被阻塞，但又对自己已有的资源保持不放

4. 循环等待条件：存在一种进程资源的**循环等待链**，链中的每一个进程已获得的资源同时被下一个进程所请求。

### 什么时候发生死锁：

1. 对系统资源的竟争。各进程对**不可剥夺**的资源(如打印机)的竞争可能引起死锁,对**可剥夺**的资源(CPU)的竟争是**不会**引起死锁的
2. 进程推进顺序非法。**请求和释放资源的顺序不当**,也同样会导致死锁。例如,并发执行的进程P1、P2分别申请并占有了资源R1、R2,之后进程P1又紧接着申请资源R2,而进程P2又申请资源R1, 两者会因为申请的资源被对方占有而阻塞,从而发生死锁
3. **信号量的使用不当**也会造成死锁。如生产者-消费者问题中,如果实现互斥的P操作在实现同步的P操作之前,就有可能导致死锁。(可以把互斥信号量、同步信号量也看做是一种抽象的系统资源)
   
总之,对不可剥夺资源的不合理分配,可能导致死锁。

### 死锁的处理策略

1. 预防死锁：破坏死锁产生的四个必要条件中的一个或几个

2. 避免死锁，用某种方法防止系统进入不安全状态

3. 死锁的检测和解除：允许死锁的发生，不过操作系统会负责检测出死锁的发生，然后采取某种措施解除死锁


### 预防死锁

**破坏互斥条件**

把只能互斥使用的资源改造为允许共享使用，比如SPOOLing技术。spooling队列

缺点：并不是所有的资源都可以改造成可共享使用的资源，而且为了系统安全，很多地方还必须保持互斥性

**破坏不剥夺条件**

不剥夺条件：进程所获得的资源在未使用完之前，不能由其他进程强行夺走，只能主动释放

1. 方案一：当某个进程请求新的资源而得不到满足时，它必须立即释放保持的所有资源，待以后需要时再重新申请，也就是说，即使某些资源尚未使用完，也需要主动释放，这样就破坏了不可剥夺事件。

2. 方案二：当某个进程需要的资源被其他进程所占有的时候，可以由操作系统协助，将想要的资源强行剥夺，这种方式一般需要考虑各进程的优先级，比如剥夺调度方式，就是将处理机的资源强行剥夺给优先级更高的进程使用

缺点：

* 实现起来比较复杂。

* 释放已经获得的资源可能会导致前一阶段的工作失效，因此本方法一般只适用于易保存和易恢复状态的资源，如CPU。

* 反复申请和释放资源会增加系统开销，降低系统吞吐量。

* 若采用方案一，意味着只要暂时得不到某个资源，之前获得的那些资源就都需要放弃，以后再重新申请，如果一直发生这样的情况：进程饥饿。

**破坏请求和保持条件**

请求和保持条件：进程已经保持了至少一个资源，但是又提出了新的资源请求，而该资源又被其他进程所占有，此时请求进程被阻塞，但又对自己已有的资源保持不放

可以采用**静态分配方法**：即进程在运行前一次申请完它所需要的全部资源，在它的资源未满足前，不让他投入运行，一旦投入运行后，这些资源就一直归他所有，该进程就不会再请求别的任何资源了。

该策略实现起来简单，也有明显的缺点：

有些资源可能只需要用很短的时间，因此如果进程的整个运行期间都一直保持着所有资源，就会造成严重的资源浪费，资源利用率极低。另外，该策略也有可能导致某些进程饥饿。


**破坏循环等待条件**

循环等待条件：存在一种进程资源的循环等待链，链中每一个进程已获得的资源同时被下一个进程所请求

可采用**顺序资源分配法**：首先给系统中的资源编号，规定每个进程必须按编号递增的顺序请求资源，同类资源（编号相同的资源）一次申请完。

原理：一个进程只有已经占有小编号的资源时，才有资格申请更大编号的资源。按此规则,已持有大编号资源的进程不可能逆向地回来申请小编号的资源,从而就不会产生循环等待的现象。

![破坏循环等待条件](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/破坏循环等待条件.png)


缺点：

* 不方便增加新的设备，因为可能需要重新分配所有编号

* 进程实际使用资源的顺序可能和编号递增顺序不一致，会导致资源浪费

* 必须按规定次序申请资源，用户编程麻烦


### 避免死锁

1. 安全序列

安全序列指如果系统按照这个序列完成分配资源，每个进程都能顺利完成，只要能找出一个安全序列，系统就是安全状态。

如果分配资源后找不到任何一个安全序列，系统就进入了不安全状态，这就意味着之后可能所有进程都无法顺利执行下去，当然如果有进程提前归还了一些资源，那系统也可能重新回到安全状态。

可以在资源分配之前就先预先判断这次分配是否会导致系统进入不安全状态，以此决定是否答应资源分配请求，这也是银行家算法的核心思想


2. 银行家算法

在资源分配之前就先预先判断这次分配是否会导致系统进入不安全状态，如果会进入不安全状态，就暂时不答应这次请求，让该进程先阻塞等待。

![银行家算法](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/银行家算法.png)

![银行家算法2](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/银行家算法2.png)


## 死锁的检测和解除

1. 死锁的检测算法

检测系统状态，以确定系统是否发生了死锁

2. 死锁的解除算法

当认定系统中已经发生了死锁，利用该算法可将系统从死锁状态中解脱出来

### 死锁的检测

为了能对系统是否发生了死锁进行检测，必须：

1. 用某种数据结构来保存资源的请求和分配信息

2. 提供一种算法，利用上述信息来检测系统是否已进入死锁状态

![死锁的检测1](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/死锁的检测1.png)

![死锁的检测2](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/死锁的检测2.png)

![死锁的检测3](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/死锁的检测3.png)

### 死锁的解除

一旦检测出死锁的发生，就应该立即解除死锁，并不是系统中所有的进程都是死锁状态，用死锁检测算法简化资源分配图后，还连着边的那些进程就是死锁进程

解除死锁的主要方法：

1. 资源剥夺法：挂起（暂时放到外存上）某些死锁进程，并抢占它的资源，将这些资源分配给其他的死锁进程。但是应防止被挂起的进程长时间得不到资源而饥饿。

2. 撤销进程法：强制撤销部分甚至全部 死锁的进程。简单但是代价太大。

3. 进程回退法：让一个或多个死锁进程退回到足以避免死锁的地步，这就要求系统要记录进程的历史信息，设置还原点，算法麻烦。

如何决定对谁执行操作：

进程优先级，已执行的时间，还要多久能完成，进程已经使用了多少资源，进程是交互式还是批处理式























