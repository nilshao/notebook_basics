# 第二章 第二节 处理机调度

## 调度，处理机调度的概念

有一堆任务要处理，但是资源有限，这些事情没办法同时处理，就需要确定某种规则来决定**处理这些任务的顺序**，这就是调度

在多道程序系统中，进程的数量往往多于处理机的个数，这样不可能同时并行地处理各个进程，处理机调度就是从就绪队列中按照一定的算法选择一个进程并将处理机分配给它运行，以实现进程的并发执行

## 调度的三个层次：

## 高级调度。

由于内存空间有限，有时无法将用户提交的作业全部放入内存，因此就需要确定某种规则来决定将作业**调入内存**的顺序。

高级调度（作业调度）：按照一定的原则从外存上处于后备队列的作业中挑选一个（或多个）作业，给他们分配内存等必要资源，并建立相应的进程（建立PCB），以使他们获得竞争处理机的权利。

高级调度是外存与内存之间的调度，每个作业只调入一次，调出一次。掉入作业时会建立相应的pcb，调出时撤销pcb。高级调度主要是指掉入的问题，因为只有掉入时机需要操作系统来确定，但调出的时机必然是作业运行结束才调出。

## 中级调度

引入了虚拟存储技术之后，可将暂时不能运行的进程调至外存等待，等它重新具备了运行条件且内存又稍有空闲时，再重新调入内存。

这么做的目的是为了提高内存利用率和系统吞吐量。
暂时调到外存等待的进程状态为挂起状态。值得注意的是，PCB并不会一起调到外存，而是会常驻内存，pcb中会记录进程数据在外存中的存放位置，进程状态等信息，操作系统通过内存中的pcb来保持对各个进程的管理、监控，被挂起的进程pcb会被放到挂起队列中
 

中级调度（内存调度），就是要决定将哪个处于挂起状态的进程重新调入内存。
一个进程可能被多次调入调出内存，因此中级调度发生的频率要比高级调度更高。

进程的挂起态和七状态模型

![七状态模型](https://github.com/nilshao/notebook_basics/raw/master/operation_system/images/chapter2/七状态模型.png)

### 低级调度

低级调度（进程调度），其主要任务是按照某种方法和策略从就绪队列中选取一个进程，将处理机分配给它。

进程调度是操作系统中最基本的一种调度，在一般的操作系统中都必须配置进程调度。

进程调度的频率很高，一般几十毫秒一次。

### 三层调度的联系、对比

![三层调度](https://github.com/nilshao/notebook_basics/raw/master/operation_system/images/chapter2/三层调度.png)

## 进程调度的时机

进程调度（低级调度），就是按照某种算法从就绪队列中选择一个进程为其分配处理机

![进程调度的时机](https://github.com/nilshao/notebook_basics/raw/master/operation_system/images/chapter2/进程调度的时机.png)

**临界资源：** 一个时间段内只允许一个进程使用的资源，各进程需要互斥地访问临界资源。

**内核程序临界区**一般是用来访问某种内核数据结构的，比如进程的就绪队列（由各就绪进程的pcb组成）。

## 进程调度的方式

* 非剥夺调度方式/非抢占方式：只允许进程**主动放弃处理机**，在运行过程中即便有更紧迫的任务到达，当前进程依然会继续使用处理机，直到该进程终止或主动要求进入阻塞态。

	实现简单，系统开销小但是无法及时处理紧急任务，适合早起的批处理系统

* 剥夺调度方式/抢占方式：当一个进程正在处理机上执行时，如果有一个更重要或更紧迫的进程需要使用处理机，则立即暂停正在执行的进程，将处理机分配给更重要紧迫的那个进程

	可以优先处理更紧急的进程，也可以实现让各进程按时间片轮流执行的功能（通过时钟中断），适合于分时操作系统，实时操作系统。

## 进程的切换与过程

“狭义的进程调度与进程切换”的区别:

狭义的进程调度指的是从就绪队列中选中一个要运行的进程。(这个进程可以是刚刚被暂停执行的进程, 也可能是另一个进程，后一种情况就需要进程切换)

进程切换是指一个进程让出处理机,由另一个进程占用处理机的过程。

广义的进程调度包含了选择一个进程和进程切换两个步骤。

进程切换的过程主要完成了:
1. 对原来运行进程各种数据的保存
2. 对新的进程各种数据的恢复
(如:程序计数器、程序状态字、各种数据寄存器等处理机现场信息,这些信息一般保存在进程控制块)
注意:进程切换是有代价的,因此如果过于频繁的进行进程调度、切换,必然会使整个系统的效率降低, 使系统大部分时间都花在了进程切换上,而真正用于执行进程的时间减少


## 调度算法的评价指标

### CPU利用率

CPU忙碌的时间占比总时间的比例

利用率=忙碌时间/总时间

### 系统吞吐量

单位时间内完成作业的数量

系统吞吐量=总共完成了多少道作业/总共花了多少时间

### 周转时间

指从昨夜被提交到系统开始，到作业完成为止的这段时间间隔。

包括四个部分：作业在外存后备队列上等待作业调度（高级调度）的时间，进程在就绪队列上等待进程调度（低级调度）的时间，进程在cpu上执行的时间，进程等待io操作完成的时间。后三项在一个作业的整个处理过程中可能发生多次

（作业的）周转时间 = 作业完成时间 - 作业提交时间

平均周转时间 = 各作业周转时间之和/作业数

### 带权周转

![带权周转](https://github.com/nilshao/notebook_basics/raw/master/operation_system/images/chapter2/带权周转.png)

对于周转时间相同的两个作业,实际运行时间长的作业在相同时间内被服务的时间更多, 带权周转时间更小,用户满意度更高

对于实际运行时间相同的两个作业,周转时间短的带权周转时间更小,用户满意度更高

平均带权周转时间=各作业带权周转时间之和/作业数

### 等待时间

计算机希望自己的作业尽可能少的等待处理机。

等待时间指的是进程/作业处于等待处理机状态时间之和，等待时间越长，用户满意度越低。

对于进程来说，等待时间就是指进程建立之后等待被服务的时间之和，在等待I/O完成的期间其实进程也是在被服务的，所以不计入等待时间。

对于作业来说，不仅要考虑建立进程后的等待时间，还要加上作业在外存后备队列中等待的时间。

一个作业总共需要被CPU服务多久，被I/O设备服务多久一般也是确定不变的，因此调度算法其实只会影响作业/进程的等待时间，当然，与前面指标相似，也有平均等待时间来评价整体性能。

### 响应时间

从用户提交请求到首次产生相应所用的时间。

### 总结

![调度算法的评价和指标](https://github.com/nilshao/notebook_basics/raw/master/operation_system/images/chapter2/调度算法的评价和指标.png)

## 调度算法1

先来先服务（FCFS），短作业优先（SJF），高响应比优先（HRRN）

对于每个算法，考虑：

* 算法思想，算法规则

* 适用于作业调度还是进程调度

* 抢占式，非抢占式

* 优缺点

* 是否会导致**饥饿**：某进程/作业长时间得不到服务

### 先来先服务First Come First Serve

* 思想，规则：公平，排队，先来先得

* 用于作业/进程调度：用于作业调度时，考虑的是哪个作业先到达后备队列，用于进程调度时，考虑的是哪个进程先到达就绪队列

* 非抢占

* 优点：公平、算法实现简单

* 缺点：排在长作业（进程）后面的短作业需要等待很长时间，带权周转时间很大，对短作业来说用户体验不好，即，FCFS算法对长作业有利，对短作业不利

* 是否会导致饥饿：不会

<!--* 例题，计算：

<!--![FCFS例题](https://github.com/nilshao/notebook_basics/raw/master/operation_system/images/chapter2/FCFS例题.png)-->

### 短作业优先Shortest Job First

* 思想，规则：追求最少的平均等待时间，最少的平均周转时间，最少的平均带权周转时间

* 算法规则：最短的作业/进程优先得到服务（要求服务时间最短）

* 用于作业/进程调度：既可以用于作业调度，又可以进程调度，用于进程调度时称为短进程优先（SPF，shortest process first）

* SJF和SPF都是非抢占式算法，但是也有抢占式版本：最短剩余时间优先算法（SRTN，shortest remaining time next）

* 优点：平均等待时间，平均周转时间最短

* 缺点：不公平，对短作业有利，对长作业有力，可能产生饥饿现象。另外，作业/进程的运行时间是由用户提供的，并不一定真实，不一定做到真正的短作业优先。

* 是否会导致饥饿。会，如果短作业源源不断地来，会使得长作业/进程长时间得不到服务，产生饥饿现象

<!--* 例题-->

<!--![SJF例题](https://github.com/nilshao/notebook_basics/raw/master/operation_system/images/chapter2/SJF例题.png)-->

<!--![SRTN例题1](https://github.com/nilshao/notebook_basics/raw/master/operation_system/images/chapter2/SRTN例题1.png)-->

<!--![SRTN例题2](https://github.com/nilshao/notebook_basics/raw/master/operation_system/images/chapter2/SRTN例题2.png)-->

### 高响应比优先Highest Response Ratio Next

* 思想：综合考虑作业/进程的等待时间和要求服务的时间

* 算法规则：每次调度时先计算各个作业/进程的响应比，选择响应比最高的作业/进程为其服务

	响应比=（等待时间+要求服务时间）/要求服务时间

* 用于作业/进程调度：既可以用于作业调度，也可以用于进程调度

* 是否可以抢占：非抢占式算法

* 优点：综合考虑了**等待时间**和**运行时间**（要求服务时间），等待时间相同时，要求服务时间短的优先（SJF的优点），要求服务时间相同时，等待时间长的优先（FCFS的优点），对于长作业来说，等待时间长了响应比也会变大，避免了长作业饥饿的问题。


* 是否会导致饥饿：不会
   
### 三种算法总结

这几种算法主要关心对用户的公平性、平均周转时间、平均等待时间等评价系统整体性能的指标,但是不区分任务的**紧急程度**,因此对于用户来说,**交互性**很糟糕。因此这三种算法一般适合用于早期的批处理系统,当然,FCFS算法也常结合其他的算法使用,在现在也扮演着很重要的角色。

## 调度算法2

时间片轮转调度算法（RR），优先级调度，多级反馈队列调度算法

对于每个算法，考虑：

* 算法思想，算法规则

* 适用于作业调度还是进程调度

* 抢占式，非抢占式

* 优缺点

* 是否会导致饥饿：某进程/作业长时间得不到服务

### 时间片轮转Round-Robin

* 思想：公平地，轮流地为各个进程服务，让每一个进程在一定时间间隔内都可以得到响应

* 规则：按照各进程到达就绪队列的顺序，**轮流让各个进程执行一个时间片**（如100ms），若进程未在一个时间片内执行完，则剥夺处理机，将进程放到就绪队列队尾重新排队。每次都选择排在就绪队列队头的进程

	如果时间片太大，使得每个进程都可以在一个时间片内就完成，则时间片轮转调度算法退化为先来先服务算法，并且会增大进程响应时间，因此时间片不能太大

	时间片太小，进程切换过于频繁，系统消耗大量时间来处理进程切换。

	一般来说设计时间片让切换进程的开销占比不超过1%。

* 用于作业/进程：用于进程调度，因为只有作业放入内存建立了相应的进程后，才能被分配处理机时间片

* 是否是抢占式：**抢占式**，由时钟装置发出时钟中断来通知CPU时间片已到

* 优点：**公平**，响应快，适用于分时操作系统

* 缺点：由于高频率的进程切换，因此会有一定开销；不能区分任务的紧急程度

* 是否会导致饥饿？不会

### 优先级调度算法

* 思想：考虑任务的紧急程度

* 规则：每个作业/进程都有各自的优先级，调度时选择优先级最高的作业/进程

* 用于作业/进程：既可以用于作业调度，也可以用于进程调度，甚至也可以用于I/O调度

* 是否可以抢占：抢占式和非抢占式都有

* 根据优先级是否可变，可分为静态优先级和动态优先级

	通常系统优先级高于用户进程，前台进程优先级高于后台进程，操作系统更偏好I/O型进程（I/O繁忙型进程），因为IO设备可以和cpu并行工作，先让io繁忙型进程进程运行可以让I/O设备尽早投入工作，资源利用率，系统吞吐量提升

* 优点：用优先级区分紧急程度，重要程度，适用于实时操作系统，可灵活地调整对各种作业/进程的偏好程度

* 缺点：若源源不断地有高优先级进程到来，则可能导致饥饿

* 会导致饥饿

### 多级反馈队列调度算法

* 思想：各种调度算法的折衷

* 规则：

	1. 设置多级就绪队列，各级队列优先级从高到低，时间片从小到大

	2. 新进程到达时先进入第1级队列，按FCFS原则排队等待被分配时间片，若用完时间进程还未结束，则进程进入下一级队列队尾。如果此时已经是在最下级的队列，则重新放回该队列队尾

	3. 只有第k级队列为空时，才会为k+1级队头的进程分配时间片

* 用于进程/作业？ 用于进程

* 是否可以抢占：抢占式算法，在k级队列的进程运行过程中，若更上级的队列（1-k-1级）中进入了一个新进程，这个新进程会抢占处理机

* 对各类进程相对公平（FCFS的优点）；每个新到达的进程都可以很快得到响应（RR的优点）；短进程只用较少的时间就可以完成（SPF的优点）；不必实现估计进程的运行时间（避免用户作假）；可灵活地调整对各类进程的偏好程度，如CPU密集型进程、I/O密集型进程

* 会导致饥饿

### 总结

注:比起早期的批处理操作系统来说,由于计算机造价大幅降低,因此之后出现的交互式操作系统(包括 分时操作系统、实时操作系统等)更注重系统的**响应时间、公平性、平衡性**等指标。而这几种算法恰好也能较好地满足**交互式**系统的需求。因此这三种算法适合用于交互式系统。(比如UNX使用的就是多级反馈 队列调度算法)























