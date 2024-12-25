
大家好，我是[呼噜噜](https://github.com)，今天我们来介绍计算机的储存器之一，CPU高速缓冲存储器也叫高速缓存，**CPU Cache**


缓存这个专业术语，在计算机世界中是经常使用到的。它并不是CPU所独有的，比如cdn缓存网站信息，浏览器缓存网页的图像视频等，但本文讲述的是狭义Cache，主要指的是`CPU Cache`，本文将其简称为"缓存"或者"Cache"


## 计算机性能的瓶颈


在冯诺依曼架构下，计算机存储器是分层次的，存储器的层次结构如下图所示，是一个金字塔形状的东西。从上到下依次是寄存器、缓存、主存(内存)、硬盘等等
![](https://img2024.cnblogs.com/blog/2795476/202412/2795476-20241224170403729-809015092.png)


**离CPU越近的存储器，访问速度越来越快，容量越来越小，每字节的成本也越来越昂贵**


比如一个主频为3\.0GHZ的CPU，寄存器的速度最快，可以在1个时钟周期内访问，一个时钟周期(CPU中基本时间单位)大约是0\.3纳秒，内存访问大约需要120纳秒，固态硬盘访问大约需要50\-150微秒，机械硬盘访问大约需要1\-10毫秒，最后网络访问最慢，得几十毫秒左右。


这个大家可能对时间不怎么直观，那如果我们把**一个时钟周期如果按1秒算的话，那寄存器访问大约是1s，内存访问大约就是6分钟 ，固态硬盘大约是2\-6天 ，传统硬盘大约是1\-12个月，网络访问就得几年了**！我们可以发现CPU的速度和内存等存储器的速度，完全不是一个量级上的。


![](https://img2024.cnblogs.com/blog/2795476/202412/2795476-20241224170403567-1594509320.png)


电子计算机刚出来的时候，其实CPU是没有缓存Cache的，那个时候的CPU主频很低，甚至没有内存高，CPU都是直接读写内存的


随着时代的发展，技术的革新，从1980年代开始，差距开始迅速扩大，**CPU的速度远远超过内存的速度**，在冯诺依曼架构下，**CPU访问内存的速度也就成了计算机性能的瓶颈！！！**


![](https://img2024.cnblogs.com/blog/2795476/202412/2795476-20241224170403572-1446790152.png)



> DRAM为内存颗粒,也叫动态随机存取存储器， 图片来源于：[How L1 and L2 CPU Caches Work, and Why They're an Essential Part of Modern Chips](https://github.com)


为了弥补CPU与内存两者之间的性能差异，也就是要加快CPU访问内存的速度，就引入了`缓存CPU Cache`，缓存的速度仅次于寄存器，充当了CPU与内存之间的中间角色


## 缓存及其发展历史


`缓存CPU Cache`用的是 **SRAM**(Static Random\-Access Memory)的芯片，也叫**静态随机存储器。**其只要有电，数据就可以保持存在，而一旦断电，数据就会丢失。


CPU Cache 如今通常分为大小不等的**3级缓存**，分别是 **L1 Cache**、**L2 Cache** 和 **L3 Cache**，




| **部件** | **CPU访问所需时间** | **备注** | 大小 |
| --- | --- | --- | --- |
| L1 高速缓存 | 2\~4 个时钟周期 | 每个 CPU 核心都有一块属于自己的 L1 高速缓存，L1 高速缓存通常分成**指令缓存**和**数据缓存**。 | 一般256KB\~1MB |
| L2 高速缓存 | 10\~20 个时钟周期 | L2 高速缓存同样是每个 CPU 核心都有的 | 一般2\~8MB |
| L3 高速缓存 | 20\~60个时钟周期 | L3 高速缓存是**多个 CPU 核心共用**的 | 一般10\~64MB |


我们可以发现**越靠近 CPU 核心的缓存，其访问速度越快，其大小越来越小，其制造成本也越昂贵**，常见的Cache典型分布图如下：


![chunjianbase.cn](https://img2024.cnblogs.com/blog/2795476/202412/2795476-20241224170403708-2034580379.png)


回顾Cache发展历史，我们可以发现Cache其实一开始并不是在CPU的内部，我们这里以intel系列为例


在80286之前，那个时候是没有缓存Cache的，那个时候的CPU主频很低，甚至没有内存高，CPU都是直接读写内存的
![](https://img2024.cnblogs.com/blog/2795476/202412/2795476-20241224170403602-216488668.png)


从80386开始，这个CPU速度和内存速度不匹配问题已经开始展露，并且差距开始迅速扩大，慢速度的内存成为了计算机的瓶颈，无法充分发挥CPU的性能，为解决这个问题，Intel主板支持`外部Cache`，来配合80386运行


![](https://img2024.cnblogs.com/blog/2795476/202412/2795476-20241224170403675-1205595671.png)


80486将`L1 Cache(大小8KB)`放到CPU内部，同时支持外接Cache，即L2 Cache（大小从128KB到256KB），但是不分指令和数据Cache
![](https://img2024.cnblogs.com/blog/2795476/202412/2795476-20241224170403608-1645770680.png)


虽然`L1 Cache`大小只有8KB，但其实对那时候CPU来说够用了，我们来看一副缓存命中率与L1、L2大小的关系图：
![](https://img2024.cnblogs.com/blog/2795476/202412/2795476-20241224170403593-2145997335.png)



> 图片来源于：[How L1 and L2 CPU Caches Work, and Why They're an Essential Part of Modern Chips](https://github.com)


从上图我们可以发现，增大`L1 cache`对于CPU来说好处不太明显，缓存命中率并没有显著提升，成本还会更昂高，所以性价比不高。


而随着 `L2 cache` 大小的增加，缓存总命中率会急剧上升，因此容量更大、速度较慢、更便宜的L2成为了更好的选择


等到`Pentium-1/80586`，也就是我们熟悉的奔腾系列，由于Pentium采用了双路执行的超标量结构，有2条并行整数流水线，需要对数据和指令进行双重的访问，为了使得这些访问互不干涉，于是`L1 Cache`被一分为二，分为指令Cache和数据Cache(大小都是8K)，此时的`L2 Cache`还是在主板上，再后来Intel推出了`[Pentium Pro](https://link.zhihu.com/?target=http%3A//en.wikipedia.org/wiki/Pentium_Pro)/80686`，为了进一步提高性能`L2 Cache`**被正式放到CPU内部**
![](https://img2024.cnblogs.com/blog/2795476/202412/2795476-20241224170403579-461575683.png)


后来CPU多核时代来临，Intel的`Pentium D、Pentium EE`系列，CPU内部每个核心都有自己的`L1、L2 Cache`，但他们并不共享，只能依靠总线来传递同步缓存数据。最后`Core Duo`酷睿系列的出现，`L2 Cache`变成多核共享模式，采用Intel的“Smart cache”共享缓存技术，到此为止，就确定了现代缓存的基本模式
![](https://img2024.cnblogs.com/blog/2795476/202412/2795476-20241224170403613-463052921.png)
如今`CPU Cache` 通常分为大小不等的3级缓存，分别是 `L1 Cache、L2 Cache 和 L3 Cache`，**L3 高速缓存为多个 CPU 核心共用的，而L2则被每个核心单独占据**，另外现在有的CPU已经有了`L4 Cache`，未来可能会更多


## 缓存如何弥补CPU与内存的性能差异？


我们可以思考一个问题：缓存是如何弥补CPU与内存两者之间的性能差异？
![](https://img2024.cnblogs.com/blog/2795476/202412/2795476-20241224170403577-1003483457.gif)


缓存主要是利用**局部性原理**，来提升计算机的整体性能。因为缓存的性能仅次于寄存器，而CPU与内存两者之间的产生的分歧，主要是二者存取速度数量级的差距，那**尽可能多地让CPU去存取缓存，同时减少CPU直接访问主存的次数**，这样计算机的性能就自然而然地得到巨大的提升



> [https://chunjianbase.cn](https://github.com)


所谓局部性原理，主要分为`空间局部性`与`时间局部性`：


1. **时间局部性：**被引用过一次的存储器位置在未来会被多次引用（通常在循环中）。
2. **空间局部性：**如果一个存储器的位置被引用，那么将来他附近的位置也会被引用


缓存这里，会去把CPU最近访问`主存(内存)中的指令和数据`，**临时储存着**，因为根据局部性原理，这些指令和数据**在较短的时间间隔内**很可能会被以后多次使用到，其次是当从主存中取回这些数据时，**会同时取回与其位置相邻的主存单元的存放的数据 临时储存到缓存中**，因为该指令和数据附近的内存区域，**在较短的时间间隔内**也可能会被多次访问。


那以后CPU去访问这些指令和数据时，首先去命中`L1 Cache`，如果命中会直接从对应的缓存中取数据，而不必每次去访问主存，如果没命中，会再去`L2 Cache`中找，依次类推，如果`L3 Cache`中不存在，就去内存中找。


## 尾语


本文简单介绍了计算机性能瓶颈的原因，缓存及其发展历史，最后讲解了缓存弥补CPU和内存性能差异的原理，后面我们会继续更详细深入地介绍Cache的组织结构、缓存一致性，以及如何利用缓存提升我们代码的性能等


参考资料：
[https://www.extremetech.com/extreme/188776\-how\-l1\-and\-l2\-cpu\-caches\-work\-and\-why\-theyre\-an\-essential\-part\-of\-modern\-chips](https://github.com)
[http://www.cpu\-zone.com/80486\.htm](https://github.com):[楚门加速器](https://shexiangshi.org)




---


作者：[小牛呼噜噜](https://github.com) ，关注公众号「[小牛呼噜噜](https://github.com)」，更多高质量好文等你！


[Linux0\.12内核源码解读(5\)\-head.s](https://github.com)


[突破计算机性能瓶颈的利器CPU Cache](https://github.com)


