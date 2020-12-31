# 第二章 第三节 进程同步

## 进程同步

回顾：进程有异步性的特征，各并发执行的进程以各自独立，不可预知的速度向前推进。因此需要进程进程同步解决这种**异步问题**。有的进程之间需要**相互配合**地完成工作，各进程的工作推进需要遵循一定的先后顺序

“同步”也可称为直接制约关系

## 进程互斥

各个并发执行的进程不可避免地需要共享一些系统资源，资源共享分为

* 互斥共享：系统中的某些资源虽然可以提供给多个进程使用，但是一个时间段内只允许一个进程访问该资源。
* 同时共享：系统中某些资源，允许一个时间段内由多个进程“同时”对他们进行访问。

把一个时间段内只允许一个进程使用的资源称为**临界资源**，对临界资源的访问必须**互斥**地执行，即同一时间段内只能允许一个进程访问该资源，对临界资源的互斥访问分为四个部分：

![临界资源访问](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/临界资源访问.png)

注意:
* **进入区** 负责检查是否可进入临界区,若可进入,则应设置正在问临界资源的标志(可理解为 **“上锁”**),以阻止其他进程同时进入临界区

* **临界区**是访问临界资源的那段代码，是进程中访问临界资源的代码段。临界区也可称为“临界段”。

* **退出区** 负责解除正在访问临界资源的标志(可理解为“解锁”)，进入区和退出区是负责实现互斥的代码段。 
* 退出区可以做一些其他处理

为了实现对临界资源的**互斥访问**，同时保证整体性能，需要遵循以下**原则**：

1. 空闲让进：临界区空闲时，可以允许一个请求进入临界区的进程立即进入临界区


2. 忙则等待：当已有进程进入临界区时，其他试图进入临界区的进程必须等待

3. 有限等待：对请求访问的进程，应保证能在有限时间内进入临界区（保证不会饥饿）

4. 让权等待：当进程不能进入临界区时，应立即释放处理机，防止进程忙等待

### 总结

![进程互斥总结](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/进程互斥总结.jpeg)

## 进程互斥的软件实现

单标志法，双标志先检查，双标志后检查，Peterson算法。

    每个方法考虑：
    1. 各个算法的思想、原理
    2. 结合“实现互斥的四个逻辑部分”，理解各个算法在进入区，退出区都做了什么
    3. 结合“实现互斥的四个原则”，分析各个算法的缺陷

### 单标志法

思想：两个进程在访问完临界区后会把使用临界区的权限转交给另一个进程，也就是说每一个进程进入临界区的权限只能被另一个进程赋予。

![单标志法](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/单标志法.png)

这种必须“轮流访问”带来的问题是,如果此时允许进入临界区的进程是PO,而PO一直不访问临界 区,那么虽然此时临界区空闲,但是并不允许P1访问。**违背了空闲让进原则。**

### 双标志先检查法

思想：设置一个布尔型数组flag[]，数组中各个元素用来标记各进程想进入临界区的意愿，比如flag[0]=true标识0号进程现在想要进入临界区。每个进程进入临界区之前先检查当前有没有别的进程想要进入临界区，如果没有，则把自己对应的标志flag[i]设置为true，之后开始访问临界区。

![双标志先检查法](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/双标志先检查法.png)

有可能多个进程**同时访问到临界区**：进入区的“检查”和“上锁”两个处理不是一气呵成的，“检查”后，“上锁”前可能发生进程切换。**违反“忙则等待”原则**

### 双标志后检查法

改进双标志先检查法，前一个算法的问题时先检查后上锁，但是这两个操作没法一气呵成，因此可能两个进程同时进入临界区，所以可以先上锁后检查

![双标志后检查法](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/双标志后检查法.png)

但是违反了空闲让进和有限等待，两个进程都想进入临界区但是互不相让，可能会产生饥饿现象。

### Peterson算法

双标志后检查算法中，两个算法互相争抢，Peterson算法中，如果双方都争着想进入临界区，那可以让进程主动让对方先使用临界区

![peterson算法](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/peterson算法.png)

遵循了空闲让进，忙则等待，有限等待，但是没有遵循**让权等待**

## 进程互斥的硬件实现

中断屏蔽，TestAndSet（TS指令，或TSL指令），Swap指令（XCHG指令）

### 中断屏蔽

利用**开中断、关中断指令**实现（与原语的实现思想相同，即在某进程开始访问临界区到结束访问为止都不允许被中断，也就不能发生进程切换，因此不可能发生两个同时访问临界区的情况）

优点：简单高效

缺点：不适用于多处理机，只适用于操作系统内核进程，不适用于用户进程（因为开中断关中断指令只能运行在内核态，不能让用户随意应用）

### TestAndSet指令

用硬件实现的，执行的过程中不允许被中断，只能一气呵成，

![testandset](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/testandset.png)

若刚开始lock是false,则TSL返回的old值为 false, while循环条件不满足,直接跳过循环,进入临界区。若刚开始lock是true,则执行TLS后old返回的值为true, while循环条件满足,会一直循环,直到当前访问临界区的进程在退出区进行“解锁”。

相比软件实现方法,TSL指令把“上锁”和“检査”操作用硬件的方式变成了一气呵成的原子操作。 

优点:实现简单,无需像软件实现方法那样严格检查是否会有逻辑漏洞;适用于多处理机环境

缺点：不满足让权等待原则，暂时无法进入临界区的进程会占用CPU并循环执行TSL指令，从而导致“忙等”

### Swap指令
有的地方也叫 Exchange指令,或简称XCHG指令。

Swap指令是用硬件实现的,执行的过程不允许被中断,只能一气呵成。以下是用C语言描述的逻辑

![Swap指令](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/Swap指令.png)

逻辑上来看Swap和TSL并无太大区别,都是先记录下此时临界区是否已经被上锁(记录在old变量上),再将上锁标记lock设置为true,最后检查old,如果old为false则说明之前没有别的进程对临界区上锁,则可跳出循环,进入临界区。

优点：实现简单，无需像软件实现方法那样严格检查是否会有逻辑漏洞，适用于多处理机环境

缺点：不满足“让权等待”，暂时无法进入临界区的进程会占用CPU并循环执行TSL指令，从而导致“忙等”

### 总结

![进程互斥的硬件实现总结](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/进程互斥的硬件实现总结.png)

## 信号量机制

**整形信号量，记录型信号量**	

回顾：以上解决方案均无法实现**让权等待**，引入信号量机制。

用户进程可以通过使用操作系统提供的一对原语来对信号量进行操作，从而很方便地实现了进程互斥、进程同步。

信号量其实就是一个**变量**（可以是一个整数，可以是更复杂的记录型变量），可以用一个信号量来表示系统中某种资源的数量。比如系统中只有一台打印机，就可以设置一个初始值为1的信号量

原语是一种特殊的程序段，其执行不能被中断，只能一气呵成。原语是由开中断/关中断指令实现的，软件解决方案的主要问题是由“进入区的各种操作无法一气呵成”。因此如果能把进入区、退出区的操作都用“原语”实现，使这些操作能“一气呵成”就能避免问题

一对原语：wait(S)和signal(S)，也被写为**P(S)**和**V(S)**

wait(S)和signal(S)是自己写的函数，S就是信号量

### 整形信号量

整形信号量，用来表示系统中某种资源的数量，如系统中有一台计算机 S为1

![整形信号量](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/整形信号量.png)

对信号量的操作只有三种：初始化，P操作和V操作

检查和上锁作为原语一气呵成，避免并发、异步导致的问题

问题：不满足让权等待原则，会发生忙等

### 记录型信号量

用记录型数据结构表示的信号量

![记录型信号量](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/记录型信号量.png)

释放资源后，若还有别的进程在等待这种资源，则使用wakeup原语唤醒等待队列中的一个进程，该进程从阻塞态变为就绪态

在考研题目中wait(S)、 signal(S)也可以记为P(S)、V(S),这对原语可用于实现系统资源的“申请”和“释放”。

S.value的初值表示系统中某种资源的数目。

对信号量S的一次P操作意味着进程请求一个单位的**该类资源**,因此需要执行S.value--,表示资源数减1,当S.value<0时表示该类资源已分配完毕,因此进程应调 用block原语进行自我阻塞(当前运行的进程从运行态→阻塞态),主动放弃处理机,并插入该类资源的等 待队列S.L中。可见,该机制遵循了“让权等待”原则, 不会出现“忙等”现象。

对信号量S的一次V操作意味着进程**释放**一个单位的该类资源,因此需要执行S.value++,表示资源数加1, 若加1后仍是S.value<=0,表示依然有进程在等待该类资源,因此应调用wakeup原语唤醒等待队列中的第个进程(被唤醒进程从阻塞态￫就绪态)。

### 信号量机制实现互斥

使用临界资源之前要加锁，使用临界资源后要解锁。对不同临界资源的使用需要设置不同的互斥信号量。P操作V操作必须成对使用

### 信号量机制实现同步

用信号量实现进程同步，“让各个并发进程有序地推进”:
1. 分析什么地方需要实现“同步关系”,即必须保证“一前一后”执行的两个操作(或两句代码)
2. 设置同步信号量S,初始为0
3. 在“前操作”之后执行V(S)
4. 在“后操作”之前执行P(S)

若先执行到P(S)操作,由于S=0,S--后S=-1,表示此时没有可用资源,因此P操作中会执行 block原语,主动请求阻塞。之后当执行完代码2,继而执行V(S)操作,S++,使S变回0, 由于此时有进程在该信号量对应的阻塞队列中,因此会在V操作中执行 wakeup原语,唤醒P2进程。这样P2就可以继续
执行代码4了。

### 信号量机制实现前驱关系

![信号量机制实现前驱关系](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/信号量机制实现前驱关系.png)

### 信号量机制实现各种操作总结

![信号量机制实现各种操作总结](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/信号量机制实现各种操作总结.png)

## 生产者消费者问题

系统中有一组生产者进程和一组消费者进程，生产者每次生产一个“产品”放入缓冲区，消费者每次从缓冲区取出一个“产品”并使用，产品指某种数据。

生产者、消费者共享一个初始为空，大小为n的缓冲区。

只有缓冲区不满时，生产者才能把产品放入缓冲区，否则必须等待。

只有缓冲区不空时，消费者才能从中取出产品，否则必须等待。缓冲区时临界资源，各进程必须互斥访问。

可以用信号量机制实现生产者、消费者进程的这些功能

死锁问题


## 多生产者多消费者 


多生产者，多消费者共享缓冲区 


### 吸烟者问题

### 读者-写者问题

### 哲学家进餐问题


## 管程

信号量机制的问题：编写程序困难、易出错

为了再写程序时不需要再关注复杂的pv操作，引入管程

### 管程的定义和基本特征

管程是一个特殊的软件模块，由这些部分组成

1. 局部于管程的共享数据结构说明

2. 对该数据结构进行操作的一组过程

3. 对局部于管程的共享数据设置初始值的语句

4. 管程有一个名字

管程的基本特征：

1. 局部于管程的数据只能被局部于管程的过程所访问

2. 一个进程只有通过调用管程内过程才能进入管程访问共享数据

3. 每次仅允许一个进程在管程内执行某个内部过程

![用管程解决生产者消费者问题](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/用管程解决生产者消费者问题.png)

由编译器负责实现各进程互斥地进入管程中的过程

### 实现互斥和同步

1. 需要在管程中定义共享数据（如缓冲区）

2. 需要在管程中定义用于访问这些共享数据的“入口”————其实就是一些函数

![管程](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/管程.png)

### 管程总结

![管程总结](https://github.com/nilshao/cpp-notebook/raw/master/operation_system/images/chapter2/管程总结.png)
