---
title: Java高级工程师常见面试题 
tags: 面试题
---
### java基础

 1. String类为什么是final的

 2. HashMap的源码，实现原理，底层结构
简单地说，HashMap 在底层将 key-value 当成一个整体进行处理，这个整体就是一个 Entry 对象。HashMap 底层采用一个 Entry[] 数组来保存所有的 key-value 对，当需要存储一个 Entry 对象时，会根据 hash 算法来决定其在数组中的存储位置，在根据 equals 方法决定其在该数组位置上的链表中的存储位置；当需要取出一个Entry 时，也会根据 hash 算法找到其在数组中的存储位置，再根据 equals 方法从该位置上的链表中取出该Entry

 3. 说说你知道的几个Java集合类：list、set、queue、map实现类
list: ArrayList,LinkedList,Vector
set: HashSet,EnumSet,TreeSet
queue: PriorityQueue
map: HashMap,TreeMap,Hashtable

 4. 描述一下ArrayList和LinkedList各自实现和区别

 5. Java中的队列都有哪些，有什么区别
add(E e) //Inserts the specified element into this queue 
offer(E e) //Inserts the specified element into this queue 
element() //Retrieves, but does not remove, the head of this queue,
peek()  //Retrieves, but does not remove, the head of this queue,
poll() //Retrieves and removes the head of this queue
remove() //Retrieves and removes the head of this queue.

 6. 反射中，Class.forName和classloader的区别
java中class.forName()和classLoader都可用来对类进行加载。
class.forName()前者除了将类的.class文件加载到jvm中之外，还会对类进行解释，执行类中的static块。
而classLoader只干一件事情，就是将.class文件加载到jvm中，不会执行static中的内容,只有在newInstance才会去执行static块。
Class.forName(name, initialize, loader)带参函数也可控制是否加载static块。并且只有调用了newInstance()方法采用调用构造函数，创建类的对象

 7. Java7、Java8的新特性(baidu问的,好BT)

 8. Java数组和链表两种结构的操作效率，在哪些情况下(从开头开始，从结尾开始，从中间开始)，哪些操作(插入，查找，删除)的效率高

 9. Java内存泄露的问题调查定位：jmap，jstack的使用等等

 10. string、stringbuilder、stringbuffer区别
String 字符串常量
StringBuffer 字符串变量（线程安全）
StringBuilder 字符串变量（非线程安全）

 11. hashtable和hashmap的区别
hashtable:线程anquan
hashMap:非线程安全

 12. 异常的结构，运行时异常和非运行时异常，各举个例子

![enter description here][1]

==>Throwable类是所有异常或错误的超类，它有两个子类：Error和Exception，分别表示错误和异常。其中异常Exception分为运行时异常(RuntimeException)和非运行时异常，也称之为不检查异常(Unchecked Exception)和检查异常(Checked Exception)。

==>一般是指java虚拟机相关的问题，如系统崩溃、虚拟机出错误、动态链接失败等，这种错误无法恢复或不可能捕获，将导致应用程序中断，通常应用程序无法处理这些错误，因此应用程序不应该捕获Error对象，也无须在其throws子句中声明该方法抛出任何Error或其子类。

==>运行时异常都是RuntimeException类及其子类异常，如NullPointerException、IndexOutOfBoundsException等，这些异常是不检查异常，程序中可以选择捕获处理，也可以不处理。这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生。

==>非运行时异常是RuntimeException以外的异常，类型上都属于Exception类及其子类。如IOException、SQLException等以及用户自定义的Exception异常。对于这种异常，JAVA编译器强制要求我们必需对出现的这些异常进行catch并处理，否则程序就不能编译通过。所以，面对这种异常不管我们是否愿意，只能自己去写一大堆catch块去处理可能的异常。

 13. String a= “abc” String b = “abc” String c = new String(“abc”) String d = “ab” + “c” .他们之间用 == 比较的结果

 14. String 类的常用方法

 15. Java 的引用类型有哪几种
**强引用**:这是JAVA程序中最常见的引用方式，程序创建一个对象，并把这个对象赋给一个引用变量，这个引用变量就是强引用。(由于JVM肯定不会回收强引用所引用的JAVA对象，因此强引用是造成JAVA内存泄露的主要原因之一)
**软引用**:软引用需要通过java.long.ref.SoftReference类来实现，当一个对象只具有软引用是，它有可能被垃圾回收机制回收。对于只有软引用的对象而言，当系统内存空间足够时，它不会被系统回收，程序也可使用该对象；当系统内存空间不足时，系统将会回收它。
**弱引用**：弱引用通过java.long.ref.WeakReference类或java.util.WeakHashMap来实现，弱引用和软引用很像，但弱引用的引用级别更低。对于只有弱引用的对象而言，当系统垃圾回收机制运行时，不管系统内存是否足够，总会回收该对象所占用的内存。注意：采用String str = “hello”;代码定义字符串时，系统会缓存这个字符串直接量（会使用强引用来引用它），系统不会回收被缓存的字符串常量。
**虚引用**：就是跟踪对象被垃圾回收的状态，通过java.long.ref.PhantomReference类实现，它完全类似于没有引用。程序可以通过检查与虚引用关联的引用队列中是否已经包含指定的虚引用，从而了解虚引用所引用对象是否即将被回收。引用队列由java.lang.ref.ReferenceQueue类表示，它用于保存被回收后对象的引用。当把软引用、弱引用和引用队列联合使用时，系统回收被引用的对象之后，将会把被回收对象对应的引用添加到关联的引用队列中。与软引用和弱引用不同的事，虚引用在对象被释放之前，将把它对应的虚引用添加到它的关联的引用队列中，这使得可以在对象被回收之前采取行动。

 16. 抽象类和接口的区别

 17. java的基础类型和字节大小
byte 1, short 2, char 2, int 4, long 8,float 4, double 8

 18. Hashtable,HashMap,ConcurrentHashMap 底层实现原理与线程安全问题（建议熟悉 jdk 源码，才能从容应答）

 19. 如果不让你用Java Jdk提供的工具，你自己实现一个Map，你怎么做。说了好久，说了HashMap源代码，如果我做，就会借鉴HashMap的原理，说了一通HashMap实现
 20. Hash冲突怎么办？哪些解决散列冲突的方法？

 21. HashMap冲突很厉害，最差性能，你会怎么解决?从O（n）提升到log（n）咯，用二叉排序树的思路说了一通

 22. rehash

 23. hashCode() 与 equals() 生成算法、方法怎么重写
需要注意记住的事情

尽量保证使用对象的同一个属性来生成hashCode()和equals()两个方法。在我们的案例中,我们使用员工id。
eqauls方法必须保证一致（如果对象没有被修改，equals应该返回相同的值）
任何时候只要a.equals(b),那么a.hashCode()必须和b.hashCode()相等。
两者必须同时重写。

### java IO

 1. 讲讲IO里面的常见类，字节流、字符流、接口、实现类、方法阻塞
![enter description here][2]

 2. 讲讲NIO

 3. String 编码UTF-8 和GBK的区别

 4. 什么时候使用字节流、什么时候使用字符流

 5. 递归读取文件夹下的文件，代码怎么实现
```
public static List<File> getAllFiles(File file){
    List<File> retFiles = new ArrayList();
    File[] files = file.listFiles();
    for (File f : files) {
        if (f.isDirectory()) {
            retFiles.addAll(getAllFiles(f));
        } else {
            retFiles.add(f);
        }
    }
    return retFiles;
}
```

 6. 字节流和字符流转换类
InputSreamReader用于将一个字节流中的字节解码成字符：
有两个构造方法：
　InputStreamReader(InputStream in);
　功能：用默认字符集创建一个InputStreamReader对象
　InputStreamReader(InputStream in,String CharsetName);
  功能：接收已指定字符集名的字符串，并用该字符创建对象
OutputStreamWriter用于将写入的字符编码成字节后写入一个字节流。
同样有两个构造方法：
　OutputStreamWriter(OutputStream out);
　功能：用默认字符集创建一个OutputStreamWriter对象；
　OutputStreamWriter(OutputStream out,String  CharSetName);
　功能：接收已指定字符集名的字符串，并用该字符集创建OutputStreamWrite对象
为了避免频繁的转换字节流和字符流，对以上两个类进行了封装。
　BufferedWriter类封装了OutputStreamWriter类；
　BufferedReader类封装了InputStreamReader类；
　封装格式：
　BufferedWriter out=new BufferedWriter(new OutputStreamWriter(System.out));
　BufferedReader in= new BufferedReader(new InputStreamReader(System.in);
　利用下面的语句，可以从控制台读取一行字符串：
　BufferedReader in=new BufferedReader(new InputStreamReader(System.in));
　String line=in.readLine();

### JVM

 1. Java的内存模型以及GC算法
对象存活判断
判断对象是否存活一般有两种方式：
引用计数：每个对象有一个引用计数属性，新增一个引用时计数加1，引用释放时计数减1，计数为0时可以回收。此方法简单，无法解决对象相互循环引用的问题。
可达性分析（Reachability Analysis）：从GC Roots开始向下搜索，搜索所走过的路径称为引用链。当一个对象到GC Roots没有任何引用链相连时，则证明此对象是不可用的。不可达对象。
垃圾收集算法
标记 -清除算法
复制算法
标记-压缩算法
分代收集算法

 2. jvm性能调优都做了什么
主要调优的目的:
==>控制GC的行为.GC是一个后台处理,但是它也是会消耗系统性能的,因此经常会根据系统运行的程序的特性来更改GC行为
==>控制JVM堆栈大小.一般来说,JVM在内存分配上不需要你修改,(举例)但是当你的程序新生代对象在某个时间段产生的比较多的时候,就需要控制新生代的堆大小.同时,还要需要控制总的JVM大小避免内存溢出
==>控制JVM线程的内存分配.如果是多线程程序,产生线程和线程运行所消耗的内存也是可以控制的,需要通过一定时间的观测后,配置最优结果

 3. 介绍JVM中7个区域，然后把每个区域可能造成内存的溢出的情况说明

 4. 介绍GC 和GC Root不正常引用

 5. 自己从classload 加载方式，加载机制说开去，从程序运行时数据区，讲到内存分配，讲到String常量池，讲到JVM垃圾回收机制，算法，hotspot。反正就是各种扩展

 6. jvm 如何分配直接内存， new 对象如何不分配在堆而是栈上，常量池解析
NIO的Buffer提供了一个可以不经过JVM内存直接访问系统物理内存的类——DirectBuffer。 DirectBuffer类继承自ByteBuffer，但和普通的ByteBuffer不同，普通的ByteBuffer仍在JVM堆上分配内存，其最大内存受到最大堆内存的限制；而DirectBuffer直接分配在物理内存中，并不占用堆空间，其可申请的最大内存受操作系统限制。

 7. 数组多大放在 JVM 老年代（不只是设置 PretenureSizeThreshold ，问通常多大，没做过一问便知）

 8. 老年代中数组的访问方式

 9. GC 算法，永久代对象如何 GC ， GC 有环怎么处理

 10. 谁会被 GC ，什么时候 GC

 11. 如果想不被 GC 怎么办

 12. 如果想在 GC 中生存 1 次怎么办

 13. JVM参数设置说明


### 多线程

 1. Java创建线程之后，直接调用start()方法和run()的区别
 2. 常用的线程池模式以及不同线程池的使用场景
 3. newFixedThreadPool此种线程池如果线程数达到最大值后会怎么办，底层原理
 4. 多线程之间通信的同步问题，synchronized锁的是对象，衍伸出和synchronized相关很多的具体问题，例如同一个类不同方法都有synchronized锁，一个对象是否可以同时访问。或者一个类的static构造方法加上synchronized之后的锁的影响
 5. 了解可重入锁的含义，以及ReentrantLock 和synchronized的区别
 6. 同步的数据结构，例如concurrentHashMap的源码理解以及内部实现原理，为什么他是同步的且效率高
 7. atomicinteger和Volatile等线程安全操作的关键字的理解和使用
 8. 线程间通信，wait和notify
 9. 定时线程的使用
 10. 场景：在一个主线程中，要求有大量(很多很多)子线程执行完之后，主线程才执行完成。多种方式，考虑效率。
 11. 进程和线程的区别
 12. 什么叫线程安全？举例说明
 13. 线程的几种状态
 14. 并发、同步的接口或方法
 15. HashMap 是否线程安全，为何不安全。 ConcurrentHashMap，线程安全，为何安全。底层实现是怎么样的。
 16. J.U.C下的常见类的使用。 ThreadPool的深入考察； BlockingQueue的使用。（take，poll的区别，put，offer的区别）；原子类的实现
 17. 简单介绍下多线程的情况，从建立一个线程开始。然后怎么控制同步过程，多线程常用的方法和结构
 18. volatile的理解
 19. 实现多线程有几种方式，多线程同步怎么做，说说几个线程里常用的方法

### java Web

 1. session和cookie的区别和联系，session的生命周期，多个服务部署时session管理。
 2. servlet的一些相关问题
 3. webservice相关问题
 4. jdbc连接，forname方式的步骤，怎么声明使用一个事务。举例并具体代码
 5. 无框架下配置web.xml的主要配置内容
 6. jsp和servlet的区别

### 开源框架

 1. hibernate和ibatis的区别
 2. 讲讲mybatis的连接池
 3. spring框架中需要引用哪些jar包，以及这些jar包的用途
 4. springMVC的原理
 5. springMVC注解的意思
 6. spring中beanFactory和ApplicationContext的联系和区别
 7. spring注入的几种方式（循环注入）
 8. spring如何实现事物管理的
 9. springIOC
 10. spring AOP的原理
 11. hibernate中的1级和2级缓存的使用方式以及区别原理（Lazy-Load的理解）
 12. Hibernate的原理体系架构，五大核心接口，Hibernate对象的三种状态转换，事务管理

### 网络通信

 1. http是无状态通信，http的请求方式有哪些，可以自己定义新的请求方式么

 2. socket通信，以及长连接，分包，连接异常断开的处理。

 3. socket通信模型的使用，AIO和NIO。

 4. socket框架netty的使用，以及NIO的实现原理，为什么是异步非阻塞。
 
5. 同步和异步，阻塞和非阻塞

 6. OSI七层模型，包括TCP,IP的一些基本知识
![enter description here][3]

 7. http中，get post的区别
 8. 说说http,tcp,udp之间关系和区别。
 9. 说说浏览器访问www.taobao.com，经历了怎样的过程。
 10. HTTP协议、  HTTPS协议，SSL协议及完整交互过程；
A.客户端请求SSL连接，并将自己支持的加密规则发给网站。
B.服务器端将自己的身份信息以证书形式发回给客户端。证书里面包含了网站地址，加密公钥，以及证书的颁发机构。
C.获得证书后，客户要做以下工作： 
1)验证证书合法性 
2)如果证书受信任，客户端会生成一串随机数的密码，并用证书提供的公钥进行加密。 
3)将加密好的随机数发给服务器。
D.获得到客户端发的加密了的随机数之后，服务器用自己的私钥进行解密，得到这个随机数，把这个随机数作为对称加密的密钥。
E.之后服务器与客户之间就可以用随机数对各自的信息进行加密，解密
 11. tcp的拥塞，快回传，ip的报文丢弃

 12. https处理的一个过程，对称加密和非对称加密

 13. head各个特点和区别

### 数据库MySQL

 1. mysql的存储引擎的不同
 2. 单个索引、联合索引、主键索引
 3. Mysql怎么分表，以及分表后如果想按条件分页查询怎么办(如果不是按分表字段来查询的话，几乎效率低下，无解)
 4. 分表之后想让一个id多个表是自增的，效率实现
 5. MySql的主从实时备份同步的配置，以及原理(从库读主库的binlog)，读写分离
 6. 索引的数据结构，B+树
 7. 事务的四个特性，以及各自的特点（原子、隔离）等等，项目怎么解决这些问题
 8. 数据库的锁：行锁，表锁；乐观锁，悲观锁
 9. 数据库事务的几种粒度
 10. 关系型和非关系型数据库区别

### 设计模式

 1. 单例模式：饱汉、饿汉。以及饿汉中的延迟加载,双重检查
 2. 工厂模式、装饰者模式、观察者模式
 3. 工厂方法模式的优点（低耦合、高内聚，开放封闭原则）

### 算法

 1. 使用随机算法产生一个数，要求把1-1000W之间这些数全部生成。（考察高效率，解决产生冲突的问题）
 2. 两个有序数组的合并排序
 3. 一个数组的倒序
 4. 计算一个正整数的正平方根
 5. 说白了就是常见的那些查找、排序算法以及各自的时间复杂度
 6. 二叉树的遍历算法
 7. DFS,BFS算法
 8. 比较重要的数据结构，如链表，队列，栈的基本理解及大致实现
 9. 排序算法与时空复杂度（快排为什么不稳定，为什么你的项目还在用）
 10. 逆波兰计算器
 11. Hoffman 编码
 12. 查找树与红黑树

### 并发与性能调优

 1. 有个每秒钟5k个请求，查询手机号所属地的笔试题(记得不完整，没列出)，如何设计算法?请求再多，比如5w，如何设计整个系统?
 2. 高并发情况下，我们系统是如何支撑大量的请求的
 3. 集群如何同步会话状态
 4. 负载均衡的原理
 5. 如果有一个特别大的访问量，到数据库上，怎么做优化（DB设计，DBIO，SQL优化，Java优化）
 6. 如果出现大面积并发，在不增加服务器的基础上，如何解决服务器响应不及时问题
 7. 假如你的项目出现性能瓶颈了，你觉得可能会是哪些方面，怎么解决问题
 8. 如何查找 造成 性能瓶颈出现的位置，是哪个位置照成性能瓶颈
 9. 你的项目中使用过缓存机制吗？有没用用户非本地缓存

### 其他

 1. 常用的Linux下的命令


  [1]: ./images/20140825105709593.jpg "20140825105709593"
  [2]: ./images/20180127210359151.jpg "20180127210359151"
  [3]: ./images/%E7%BD%91%E7%BB%9C%E7%BB%93%E6%9E%84.png "网络结构"