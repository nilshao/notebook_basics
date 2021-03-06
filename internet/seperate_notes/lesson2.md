# 第二章 物理层

## abstract

1. 通信基础

2. 两个公式lim

3. 看图说话

4. 传输介质

5. 物理层设备

## 基本概念

物理层解决如何在连接各种计算机的传输媒体上传输比特流。而不是指具体的传输媒体

主要任务：确定与**传输媒体接口**有关的一些特性 -> 定义标准

1. 机械特性：定义物理连接的特性，规定物理连接时所采用的规格，接口形状，引线数目，引脚数量和排列状况

2. 电气特性：规定传输二进制位时，线路上信号的电压范围，阻抗匹配，传输速率和距离限制等

3. 功能特性：指明某条线上出现的某一电平表示何种意义，接口部件的信号线的用途

4. 规程特性：（过程特性）定义各条物理线路的工作规程和时序关系

## 数据通信基础知识

典型的数据通信模型：

![典型的数据通信模型](https://github.com/nilshao/notebook_basics/raw/master/internet/pictures/chapter02/典型的数据通信模型.JPG)

### 相关术语：

通信的目的是传送消息。

数据：传送信息的实体，通常是有意义的符号序列

信号：数据的电气/电磁的表现，是数据在传输过程中的形式----数字信号，离散 / 模拟信号，连续

信源：产生和发送数据的源头

信宿：接受数据的终点。

信道：信号的传输媒介，一般用来表示向某个方向传送信息的介质，因此一条通信线路往往包含一条发送信道和一条接收信道。模拟信号（传送模拟信号）/ 数字信道，有线信道/无线信道

### 三种通信方式

1. 单工通信 只有一个方向的通信而没有反方向的交互，仅需一条信道

2. 半双工通信 通信的双方都可以发送或接收信息，但任何一方都不能同时发送和接收，需要两条信道

3. 双工通信 通信双方可以同时发送和接收信息，需要两条信道

### 两种数据传输方式

串行传输：速度慢，费用低，适合远距离

并行传输：速度快，费用高，适合近距离。常用于计算机内部数据传输。

## 码元，波特，速率，带宽

### 码元

码元是指用一个固定市场的信号波形（数字脉冲），代表不同离散数值的基本波形，是数字通信中数字信号的计量单位，
这个时长内的信号称为k进制码元，而该时长称为**码元宽度**。当码元的离散状态有M个时(M>2)，此时码元为M进制码元

1码元可以携带多个比特的信息量，例如在使用二进制编码时，只有两种不同的码元，一种表示0状态，另一种代表1状态。

4进制码元：离散状态有4个，有4种高低不同的信号波形，00，01，10，11

### 速率，波特，带宽

速率也叫数据率，是指数据的传输速率，表示单位时间内传输的数据量。可以用码元传输速率和信息传输速率表示

1. 码元传输速率：简称码元速率，波形速率，调制速率，符号速率等，他表示**单位时间内数字通信系统所传输的码元个数**，
也可称为脉冲个数或信号变化的次数）单位是波特，1波特表示数字通信系统中每秒传输一个码元，可以是任意进制的，
码元速率和进制数无关：1s传输多少个码元。

2. 信息传输速率：别名信息速率，比特率等等。表示单位时间内数字通信系统传输的**二进制**码元个数，即比特数。单位是比特/秒

关系：若一个码元携带n bit的信息量，则M波特的码元传输速率所对应的信息传输速率为 M*n bit/s

3. 带宽：表示在单位时间内从网络中某一点到另一点所能通过的“最高数据率”，常用来表示网络的通信线路所能传输数据的能力，单位是b/s。

### 练习题
![波特练习题](https://github.com/nilshao/notebook_basics/raw/master/internet/pictures/chapter02/波特练习题.JPG)

取对数：16进制码元有 $$\log _{2} 16 = 4$$ 位二进制数 

$\log _{2} 4$


### 总结

系统传输的是比特流，通常比较的是信息传输速率，所以传输十六进制码元的通信系统传输速率较快，如果该系统去传输四进制码元会有更高的码元传输速率

## 奈氏准则和香农定理

### 失真

现实中的信道（带宽受限、有噪声、干扰）

影响失真程度的因素：码元传输速率，信号传输距离，噪声干扰，传输媒体质量。

**信道带宽：** 信道能通过的最高频率和最低频率之差。过高频率：** 码间串扰**，频率太高导致无法区分不同信号之间的波形差异，
接收端收到的信号波形失去了码元之间清晰界限的现象。
过低频率：衰减太快。

### 奈奎斯特定理

or奈氏准则：在理想低通（无噪声，带宽受限）的条件下，为了避免码间串扰，**极限码元传输速率**为2W波特，W是信道带宽，单位是Hz。

区分：极限数据率 2Wlog2 V（b/s）  V：几种码元/码元的离散电平数目

1. 在任何信道中，码元传输的速率是有上限的，若传输速率超过此上限，就会出现严重的码间串扰问题。使接收端对码元的完全正确识别成为不可能。

2. 信道的频带越宽，即能通过的信号高频份量越多，就可以用更高的速率进行码元的有效传输。

3. 奈氏准则 给出了码元传输速率的限制，没有对信息传输速率给出限制

4. 由于码元的传输速度收到奈氏准则的制约，所以要提高数据的传输速率，就必须设法使**每个码元**能携带更多比特的信息量，
这就需要用到多元制的调制方法。

![奈氏定理练习](https://github.com/nilshao/notebook_basics/raw/master/internet/pictures/chapter02/奈氏定理练习.PNG)

### 香农定理

噪声存在于所有电子设备和通信信道中，由于噪声随机产生。他的瞬时值有时会很大，因此噪声会使接收端对码元的判决产生错误，
但是噪声的影响是相对的，若信号较强，那么噪声影响相对较小。因此**信噪比**就很重要。

信噪比 = 信号的平均功率/噪声的平均功率，常记为S/N，并用分贝dB作为度量单位、
公式

![香农定理](https://github.com/nilshao/notebook_basics/raw/master/internet/pictures/chapter02/香农定理.PNG)

### 推论

在带宽受限且有噪声的信道中，为了不产生误差，信息的数据传输速率有上限值。

1. 信道的带宽或信道中的信噪比越大，则信息的极限传输速率就越高。

2. 对一定的传输带宽和一定的信噪比，信息传输速率的上限就确定了。

3. 只要信息的传输速率低于信道的极限传输速率，就一定能找到某种方法来实现无差错的传输。

4. 香农定理得出的为极限信息传输速率，实际信道能达到的传输速率要比它低

5. 从香农定理可以看出，若信道带宽W或信噪比S/N没有上限（不可能），那么信道的极限信息传输速率也没有上限。

### 香农定理，奈氏准则

奈氏准则：避免码间串扰。

香农定理：有噪声条件下的信息传输速率。

## 编码和调制

### 基带信号和宽带信号

信道：信号的传输媒介。一般用来表示向某一个方向传送信息的介质，因此一条通信线路往往包含一条发送信道和一条接收信道。

![基带信号宽带信号](https://github.com/nilshao/notebook_basics/raw/master/internet/pictures/chapter02/基带信号宽带信号.JPG)

数据 -> 数字信号：**编码**

数据 -> 模拟信号：**调制**

### 数字数据编码为数字信号

1. 非归零编码 NRZ：高1低0，编码容易实现，没有检错功能，无法判断一个码元的开始和结束。因此收发双方难以保持同步。

2. 曼彻斯特编码：将一个码元分成两个相等的间隔，前一个间隔为低电平后一个间隔为高电平表示码元1，码元0则相反。
也可以采用相反的规定。该编码的特点是在每一个码元的中间出现电平跳变，位中间的跳变既作为时钟信号有作为数据信号，
但他所占的频带宽度是原始的基带宽度的两倍。每一个码元都被调成两个电平，所以数据传输速率只有调制速率的1/2. 

3. 差分曼斯特编码：同1异0，常用于局域网传输，其规则是：若码元为1，则前半个码元的电平与上一个码元的后半个码元电平相同，
若为0则相反。特点是在每个码元的中间都有一次电平的跳转，可以实现自同步，且抗干扰性强于曼彻斯特编码。

4. 归零编码 RZ：信号电平在一个码元之内都要恢复到0的编码

5. 反向不归零编码 NRZI：信号电平反转表示0，信号电平不变表示1

6. 4B/5B编码：比特流中插入额外比特以打破一连串的0或1，编码变为80%

![数字数据编码为数字信号](https://github.com/nilshao/notebook_basics/raw/master/internet/pictures/chapter02/数字数据编码为数字信号.JPG)

### 数字数据调制为模拟信号

发送端将数字信号调制为模拟信号：调制，接收端将模拟信号解调为数字信号：解调

![数字数据调制为模拟信号](https://github.com/nilshao/notebook_basics/raw/master/internet/pictures/chapter02/数字数据调制为模拟信号.PNG)


习题：某通信链路的波特率是1200Baud，采用4个相位，每个相位有4种振幅的QAM调制技术，则该链路的信息传输速率是多少

（hint：16种信号，16种码元）

### 模拟数据编码为数字信号

一般是数字音频

![模拟数据编码为数字信号](https://github.com/nilshao/notebook_basics/raw/master/internet/pictures/chapter02/模拟数据编码为数字信号.JPG)


### 模拟信号调制为模拟信号

转换到更高频率进行传输，频分复用技术以充分利用带宽资源，等

## 传输介质

传输介质/传输媒体/传输媒介：是传输系统中在发送设备和接收设备之间的物理通路。

传输媒体不是物理层。传输媒体在物理层下面，因为物理层是体系结构的第一层，因此有时称传输媒体为0层。
在传输媒体中传输的是信号，但传输媒体并不知道所传输的信号代表什么意思。但物理层规定了电气特性，因此能够识别所传送的比特流。

传输介质分类：

导向性传输介质：电磁波被导向沿着固体媒介（铜线，光纤）传播

非导向性传输介质：自由空间，空气，海水，真空等

### 双绞线

两根采用一定规则并排绞合的，相互绝缘的铜导线组成。绞合可以减少对相邻导线的电磁干扰。

屏蔽双绞线/非屏蔽双绞线：双绞线外加上一个金属丝编织成的屏蔽层。

双绞线价格便宜，是最常用的传输介质之一，在局域网和传统电话网中普遍使用，模拟传输和数字传输都可以使用双绞线，其通信距离一般为十公里到数十公里。距离太远时对于模拟传输，要用放大器放大衰减的信号。对于数字传输，要用中继器将失真的信号整形。

### 同轴电缆

导致铜质芯线，绝缘层，网状编织屏蔽层和塑料外层。

同轴电缆vs双绞线：由于外导体屏蔽层的作用，同轴电缆抗干扰特性比双绞线好，被广泛用于传输较高速率的数据，其传输距离更远，但价格更贵。

### 光纤

光纤通信就是利用光导纤维传递光脉冲来进行通信。有光脉冲表示1，无光脉冲表示0，而可见光的频率大约是10^8 MHz，因此光纤通信系统的带宽远远大于
目前其他各种传输媒体的带宽。

光纤在发送端由光源，可以采用发光二极管或半导体激光器，在电脉冲作用下产生光脉冲，再在接收端用观点二极管做成光检测器，还原光脉冲。

光纤主要有纤芯和包层构成,光波通过纤芯有较低的折射率，当光线从高折射率的介质射向低折射率的介质时，其折射角将大于入射角，因此如果入射角足够大，就会出现全反射，即光线碰到包层的时候就会折射回纤芯，光沿着光纤传输，超低损耗、超远传输

![光纤](https://github.com/nilshao/notebook_basics/raw/master/internet/pictures/chapter02/光纤.JPG)


单模光纤，多模光纤

![单模光纤多模光纤](https://github.com/nilshao/notebook_basics/raw/master/internet/pictures/chapter02/单模光纤多模光纤.png)

单模光纤几乎相当于直线传播，光纤直径减少到波长

光纤特点：

1. 传输损耗小，中继距离长，对远距离传输特别经济

2. 抗雷电和电磁干扰性好

3. 无串音干扰，保密性好，不易被窃听和截取数据

4. 体积小，重量轻

### 非导向性传输介质

无线电波，信号向所有方向传播：较强穿透能力，可传远距离，广泛用于通信领域（如手机通信）

微波，信号向固定方向传播：微波通信频率较高，频段范围宽，因此数据率很高。地面微博接力通信，卫星通信

红外线、激光，固定方向传播：把传输信号分别转换为各自的信号格式：红外信号and激光信号，再在空间中传播。

## 物理层设备

### 中继器

诞生原因：由于存在损耗，在线路上传输信号功率会衰减，衰减到一定程度时将造成信号失真，因此会导致接收错误。

中继器的功能：对**数字**信号进行**再生**和还原，对衰减的信号进行放大。保持与原数据相同，以增加信号传输的距离，延长网络的长度。

中继器的两端：两端的网络部分是**网段**，而不是子网，适用于完全相同的两类网络的互联，且两个网段速率要相同。
中继器只将任何电缆段上的数据发送到另一端电缆上，他仅作用于信号的电气部分，
并不管数据中是否有错误数据或不适于网段的数据。

两端可连接相同媒体，也可以连接不同媒体。

中继器两端的网段一定是同一个协议（中继器不会存储转发）

5-4-3 规则：网络标准中都对信号的延迟范围做了具体的规定，因而中继器只能在规定的范围内进行，否则会网络故障

5: 最多5个网段

4: 5个网段内最多4个网络设备：中继器/集线器

3：最多三个段可以连接计算机

### 集线器（多口中继器）

再生，放大信号

集线器功能：对信号进行再生放大转发，对衰减信号进行放大，接着转发到其他所有（除输入端口外）出于工作状态的端口上，以增加信号传输的距离，
延长网络长度。不具备信号的定向传送能力。是一个共享式设备

广播，星型拓扑

集线器不能分割冲突，各设备平分带宽。
