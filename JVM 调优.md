---
title: JVM 调优 
tags: jvm
---
整个堆大小=年轻代大小 + 年老代大小 + 持久代大小.
-Xms		初始堆大小（最小限制）
-Xmx        最大堆大小
-Xmn        年轻代大小(1.4or lator)
-XX:SurvivorRatio 	Eden区与Survivor区的大小比值 	设置为8,则两个Survivor区与一个Eden区的比值为2:8,一个Survivor区占整个年轻代的1/10
-XX:TargetSurvivorRatio=90 

-XX:PretenureSizeThreshold      对象超过多大是直接在旧生代分配  	单位字节 新生代采用Parallel Scavenge GC时无效
另一种直接在旧生代分配的情况是大的数组对象,且数组中无外部引用对象.

-XX:MaxTenuringThreshold=1      垃圾最大年龄
如果设置为0的话,则年轻代对象不经过Survivor区,直接进入年老代. 对于年老代比较多的应用,可以提高效率.如果将此值设置为一个较大值,则年轻代对象会在Survivor区进行多次复制,这样可以增加对象再年轻代的存活 时间,增加在年轻代即被回收的概率
该参数只有在串行GC时才有效.

-XX:MinHeapFreeRatio       参数用来设置堆空间最小空闲比例，默认值是 40。当堆空间的空闲内存小于这个数值时，JVM 便会扩展堆空间。 

-XX:MaxHeapFreeRatio        参数用来设置堆空间最大空闲比例，默认值是 70。当堆空间的空闲内存大于这个数值时，便会压缩堆空间，得到一个较小的堆.

当-Xmx 和-Xms 相等时，-XX:MinHeapFreeRatio 和-XX:MaxHeapFreeRatio 两个参数无效。

–XX:+LargePageSizeInBytes：设置大页的大小。

###增大吞吐量提升系统性能
–Xmx380m –Xms3800m：设置 Java 堆的最大值和初始值。一般情况下，为了避免堆内存的频繁震荡，导致系统性能下降，我们的做法是设置最大堆等于最小堆。假设这里把最小堆减少为最大堆的一半，即 1900m，那么 JVM 会尽可能在 1900MB 堆空间中运行，如果这样，发生 GC 的可能性就会比较高；

-Xss128k：减少线程栈的大小，这样可以使剩余的系统内存支持更多的线程；

-Xmn2g：设置年轻代区域大小为 2GB；

–XX:+UseParallelGC：年轻代使用并行垃圾回收收集器。这是一个关注吞吐量的收集器，可以尽可能地减少 GC 时间。

–XX:ParallelGC-Threads：设置用于垃圾回收的线程数，通常情况下，可以设置和 CPU 数量相等。但在 CPU 数量比较多的情况下，设置相对较小的数值也是合理的；

–XX:+UseParallelOldGC：设置年老代使用并行回收收集器.

### 使用非占有的垃圾回收器
–XX:ParallelGCThreads=20：设置 20 个线程进行垃圾回收；

–XX:+UseParNewGC：年轻代使用并行回收器；

–XX:+UseConcMarkSweepGC：年老代使用 CMS 收集器降低停顿；

–XX:+SurvivorRatio：设置 Eden 区和 Survivor 区的比例为 8:1。稍大的 Survivor 空间可以提高在年轻代回收生命周期较短的对象的可能性，如果 Survivor 不够大，一些短命的对象可能直接进入年老代，这对系统来说是不利的。

–XX:TargetSurvivorRatio=90：设置 Survivor 区的可使用率。这里设置为 90%，则允许 90%的 Survivor 空间被使用。默认值是 50%。故该设置提高了 Survivor 区的使用率。当存放的对象超过这个百分比，则对象会向年老代压缩。因此，这个选项更有助于将对象留在年轻代。

–XX:MaxTenuringThreshold：设置年轻对象晋升到年老代的年龄。默认值是 15 次，即对象经过 15 次 Minor GC 依然存活，则进入年老代。这里设置为 31，目的是让对象尽可能地保存在年轻代区域。

### Minor GC vs Major GC vs Full GC
 1. Minor GC
从年轻代空间（包括 Eden 和 Survivor 区域）回收内存被称为 Minor GC
触发条件：当 JVM 无法为一个新的对象分配空间时会触发 Minor GC，比如当 Eden 区满了
清理对象：每次 Minor GC 会清理年轻代的内存
 2. Major GC 
 清理对象： Major GC 是清理老年代。
 3. Full GC
清理对象：是清理整个堆空间—包括年轻代和老年代。
触发Full GC天条件：
a.直接调用System.gc

b.老年代空间不足
老年代空间只有在新生代对象转入及创建为大对象、大数组时才会出现不足的现象，当执行Full GC后空间仍然不足，则抛出如下错误:
java.lang.OutOfMemoryError: Java heap space 
为避免以上两种状况引起的Full GC，调优时应尽量做到让对象在Minor GC阶段被回收、让对象在新生代多存活一段时间及不要创建过大的对象及数组。

c.Permanet Generation空间满
Permanet Generation中存放的为一些class的信息等，当系统中要加载的类、反射的类和调用的方法较多时，Permanet Generation可能会被占满，在未配置为采用CMS GC的情况下会执行Full GC。如果经过Full GC仍然回收不了，那么JVM会抛出如下错误信息:
java.lang.OutOfMemoryError: PermGen space 
为避免Perm Gen占满造成Full GC现象，可采用的方法为增大Perm Gen空间或转为使用CMS GC。

d.CMS GC时出现promotion failed和concurrent mode failure
对于采用CMS进行老年代GC的程序而言，尤其要注意GC日志中是否有promotion failed和concurrent mode failure两种状况，当这两种状况出现时可能会触发Full GC。
promotion failed是在进行Minor GC时，survivor space放不下、对象只能放入老年代，而此时老年代也放不下造成的;concurrent mode failure是在执行CMS GC的过程中同时有对象要放入老年代，而此时老年代空间不足造成的。
应对措施为:增大survivor space、老年代空间或调低触发并发GC的比率，但在JDK 5.0+、6.0+的版本中有可能会由于JDK的bug29导致CMS在remark完毕后很久才触发sweeping动作。对于这种状况，可通过设置-XX: CMSMaxAbortablePrecleanTime=5(单位为ms)来避免。

e.统计得到的Minor GC晋升到老年代的平均大小大于老年代的剩余空间
这是一个较为复杂的触发情况，Hotspot为了避免由于新生代对象晋升到老年代导致老年代空间不足的现象，在进行Minor GC时，做了一个判断，如果之前统计所得到的Minor GC晋升到老年代的平均大小大于老年代的剩余空间，那么就直接触发Full GC。
例如程序第一次触发Minor GC后，有6MB的对象晋升到老年代，那么当下一次Minor GC发生时，首先检查老年代的剩余空间是否大于6MB，如果小于6MB，则执行Full GC。
当新生代采用PS GC时，方式稍有不同，PS GC是在Minor GC后也会检查，例如上面的例子中第一次Minor GC后，PS GC会检查此时老年代的剩余空间是否大于6MB，如小于，则触发对老年代的回收。
除了以上4种状况外，对于使用RMI来进行RPC或管理的Sun JDK应用而言，默认情况下会一小时执行一次Full GC。可通过在启动时通过- java -Dsun.rmi.dgc.client.gcInterval=3600000来设置Full GC执行的间隔时间或通过-XX:+ DisableExplicitGC来禁止RMI调用System.gc。

###Concurrent Mark and Sweep

是CMS的全称，官方给予的名称是：“Mostly Concurrent Mark and Sweep Garbage Collector”;
年轻代：采用 stop-the-world mark-copy 算法；

年老代：采用 Mostly Concurrent mark-sweep 算法；

设计目标：年老代收集的时候避免长时间的暂停；
能够达成该目标主要因为以下两个原因：

1  它不会花时间整理压缩年老代，而是维护了一个叫做 free-lists 的数据结构，该数据结构用来管理那些回收再利用的内存空间；

2  mark-sweep分为多个阶段，其中一大部分阶段GC的工作是和Application threads的工作同时进行的（当然，gc线程会和用户线程竞争CPU的时间），默认的GC的工作线程为你服务器物理CPU核数的1/4；

补充：当你的服务器是多核同时你的目标是低延时，那该GC的搭配则是你的不二选择。

### Java的垃圾回收机制
a、 停止—复制(stop-and-copy)：先暂停程序的运行，然后将所有存活的对象从当前堆复制到另一个堆，没有复制的全部都是垃圾。当对象被复制到新堆时，它们是一个挨着一个的，紧凑的。效率很低：首先，得有两个堆空间占用率200%;其次，垃圾较少时，复制大量的活着的对象，是很大的浪费。

b、 标记—清扫(mark-and-sweep)：从堆栈和静态存储区出发，遍历所有的引用，进而找出所有存活的对象，如果活着，就标记。只有全部标记完毕的时候，清理动作才开始。在清理的时候，没有标记的对象将会被释放，不会发生任何肤质动作。但是剩下的对空间是不连续的，垃圾回收器要是希望得到连续空间的话，就得重新整理剩下的对象。

c、 注意：“停止—复制”的意思是这种垃圾回收动作不是在后台进行的;相反，垃圾回收动作发生的同时，程序将会被暂停。有人将垃圾回收视为低优先级的后台进程，而事实上并不是这样，当可用内存数量比较低的时候，Sun版本的垃圾回收器就会暂停运行程序。同样，“标记-清扫”工作也必须在程序暂停的情况下才能进行。

d、 在java虚拟机中，内存分配是以较大的块为单位的。每个块内都用相应的代数(generation count)来记录它是否还存活。代数随着引用的次数而增加。垃圾回收器将对上次回收动作之后的新分配的块进行整理。这对处理大量短命的临时对象很有帮助。垃圾回收器会定期进行完整的清理动作——大型对象仍然不会被复制(只是代数增加)，内涵小型对象的那些块则被复制并整理。Java虚拟机会进行监视，如果所有对象都很稳定，垃圾回收器的效率降低的话，就切换到“标记—清扫”方式;同样，java虚拟机会追踪“标记—清扫”的效果，要是堆空间出现很多碎片，就会切换到“停止—复制”方式。这就是“自适应”技术。

总结：Java垃圾回收器是一种“自适应的、分代的、停止—复制、标记-清扫”式的垃圾回收器

### 判断对数是否可达与不可以的方式

1.引用计数（Reference Counting）
概述：给对象添加一个引用计数器，每有一个地方引用这个对象，计数器值加1，每有一个引用失效则减1
优点：实现简单、判定效率高
缺点：难以解决对象之间的循环引用问题。
2.可达性分析(Reachability Analysis)
概述：从GC Roots（每种具体实现对GC Roots有不同的定义）作为起点，向下搜索它们引用的对象，可以生成一棵引用树，树的节点视为可达对象，反之视为不可达

JVM使用“可达性分析算法”来判定一个对象是否会可以被回收，有两个细节需要注意：
1.Java的GC Roots如何定义Java中GC Roots包括以下几种对象：a.虚拟机栈（帧栈中的本地变量表）中引用的对象b.方法区中静态属性引用的对象c.方法区中常量引用的对象d.本地方法栈中JNI引用的对象2.不可达对象一定会被回收吗不是。执行垃圾回收前JVM会执行不可达对象的finalize方法，如果执行完毕之后该对象变为可达，则不会被回收它。但一个对象的finalize方法只会被执行一次。

