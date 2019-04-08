### java虚拟机

1.JVM内存的划分
  - 总结：程二栈，堆方法，运行内存
  - 程序计数器
  - java虚拟机栈：每一个栈帧对应一个方法，栈帧对应存储局部变量，常量池引用
  - 本地方法栈：与java虚拟机栈类似，不过是为本地方法服务（操作系统程序）
  - 堆：存储对象的
  - 方法区：类信息，静态变量，代码
  - 运行时常量池：在方法区中，存储常量
  - 直接内存
 
2.java的新生代和老年代
  - [参考博客](https://blog.csdn.net/lynn_kun/article/details/72793084)
  - java堆可以细分为新生代和老年代
    新生代：生命周期比较短的对象。
    老年代：生命周期比较长的对象。
  - 对象在Survivor每熬过一次Minor GC，年龄就加1，
    当年龄达到一定的程度（默认为15）时，就会被晋升到老年代中了
  - 新生代常采用的算法：复制算法
    老年代常采用的算法：标记-清除算法和标记-整理算法
    
3.垃圾收集算法
  - 标记-清除：
    定义：标记要回收的对象，然后清除
    缺点：会产生内存碎片
  - 标记-整理：
    定义：让所有存活的对象移动到一段，然后清理
  - 复制：
    将内存划分为三块，一块为Eden空间，两块为Survivor空间，三者比例是8:1：1
    清理时候将Eden空间和Survivor空间复制到另一个Survivor空间中，再进行清理
    如果一个survivor空间不够，要借用老年代的存储空间

4.Minor GC 和 Full GC
  - Minor GC发生在新生代上，Full GC发生在老年代上
  - Full GC触发条件：
    (1)调用System.gc()
    (2)老年代空间不足
    (3)空间分配担保失败
  - Minor GC触发条件：Eden空间不足
  
5.内存分配策略
  - 对象优先在Eden分配
  - 大对象直接进入老年代（阀值可以自定义）
  - 长期存活的对象进入到老年代（阀值可以设置）
  - 动态对象年龄判断
  - 空间分配担保

6.jvm类加载机制
  - [参考博客](https://blog.csdn.net/noaman_wgs/article/details/74489549)
  - JVM类加载分为5个过程(加验准解初使卸)：加载，验证，准备，解析，初始化，使用，卸载
  - 加载：
    加载主要是将.class文件（并不一定是.class。可以是ZIP包，网络中获取）中的二进制字节流读入到JVM中
  - 验证：主要确保加载进来的字节流符合JVM规范
  - 准备：主要为静态变量在方法区分配内存，并设置默认初始值。对象设置为null,变量设置为0
  - 解析：虚拟机将常量池内的符号引用替换为直接引用的过程，比如String a = "abc"，将a指向"abc"
  - 初始化：根据程序中的赋值语句主动为类变量赋值，先初始化父类，再初始化子类
    这里会按照代码顺序执行例如
    ``` 
        private static Singleton singleton = new Singleton();
        public static int value1;
        public static int value2 = 0;
    ```
    会先初始化Singleton，再初始化value1，value2
    
7.类加载器，双亲委派模型
  - [参考博客](https://blog.csdn.net/noaman_wgs/article/details/74489549)
  - 类加载器实现的功能是即为加载阶段获取二进制字节流
  - 启动类加载器->扩展类加载器->应用程序类加载器->自定义加载器
  - 类加载器之间的这种层次关系叫做双亲委派模型。
  
    ![双亲委派模型](https://img-blog.csdn.net/20170705193137597?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbm9hbWFuX3dncw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
    双亲委派模型工作原理： 
    如果一个类接受到类加载请求，他自己不会去加载这个请求，而是将这个类加载请求委派给父类加载器，
    这样一层一层传送，直到到达启动类加载器（Bootstrap ClassLoader）。
    只有当父类加载器无法加载这个请求时，子加载器才会尝试自己去加载。

7.java堆溢出问题怎么处理，内存泄漏和内存溢出的区别
  - 堆溢出问题：内存映像分析工具（如Eclipsc Memory Analyzer ）对Dump出来的堆储存快照进行分析
  - 内存泄漏memory leak :是指程序在申请内存后，无法释放已申请的内存空间，
    一次内存泄漏似乎不会有大的影响，但内存泄漏堆积后的后果就是内存溢出。
  - 内存溢出 out of memory :指程序申请内存时，没有足够的内存供申请者使用，或者说，给了你一块存储
    int类型数据的存储空间，但是你却存储long类型的数据，那么结果就是内存不够用，此时就会报错OOM,即所谓的内存溢出。 
  - 总结：内存泄漏是申请之后没有归还，内存溢出是使用空间超过拥有空间
  
8.了解垃圾收集器吗，分别介绍介绍 
  - [参考博客](https://www.cnblogs.com/chengxuyuanzhilu/p/7088316.html)
  - Serial收集器是最基本、发展历史最悠久的收集器。是单线程的收集器。
  - ParNew收集器其实就是Serial收集器的多线程版本.
  - Parallel Scavenge收集器是一个新生代收集器，它也是使用复制算法的收集器，又是并行的多线程收集器
  - Serial Old是Serial收集器的老年代版本，它同样是一个单线程收集器
  - Parallel Old 是Parallel Scavenge收集器的老年代版本
  - CMS(Concurrent Mark Sweep)收集器是一种以获取最短回收停顿时间为目标的收集器。
  - G1收集器

9.jvm调优做过没，-Xms和-Xmx分别指什么
  - [参考博客](https://blog.csdn.net/qq_20864311/article/details/81319310)
  - -Xmx用来设置你的应用程序(不是JVM)能够使用的最大内存数
  - -Xms用来设置程序初始化的时候内存栈的大小
 
11.什么是内部类，什么是匿名内部类
  - [参考博客](https://www.cnblogs.com/shen-hua/p/5440285.html)
  - 内部类就是在类的内部定义一个类
  - 如果一个内部类在整个操作中只使用一次的话，就可以定义为匿名内部类。
 
 