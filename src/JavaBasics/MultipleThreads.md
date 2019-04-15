### 多线程
 
多线程的面试问题：[参考博客](https://www.cnblogs.com/dolphin0520/p/3958019.html)


#### 基础知识

- 以下知识点(1-5)参考此博客[参考博客](https://www.cnblogs.com/dolphin0520/p/3920373.html)

1.缓存一致性协议和总线锁机制
  - 出现原因：多个cpu操作共享变量会出现数据一致性问题
  - 缓存一致性协议：某个cpu向缓存写入数据时候，同时通知其他缓存中的数据无效，这样其他缓存读取数据的时候就会从
  内存中重新读取。
  - 总线锁：如果在执行这段代码的过程中，在总线上发出了LCOK#锁的信号，那么只有等待这段代码完全执行完毕之后，
  其他CPU才能从变量i所在的内存读取变量，但是这样效率会很低下。
  - ![参考图片](https://images0.cnblogs.com/blog/288799/201408/212219343783699.jpg)
  
2.并发编程中三个概念
  - 原子性：即一个操作或者多个操作 要么全部执行并且执行的过程不会被任何因素打断，要么就都不执行。
  - 可见性:是指当多个线程访问同一个变量时，一个线程修改了这个变量的值，其他线程能够立即看得到修改的值。
  - 有序性：即程序执行的顺序按照代码的先后顺序执行。
  - (要想并发程序正确地执行，必须要保证原子性、可见性以及有序性。只要有一个没有被保证，就有可能会导致程序运行不正确。)

3.Java语言对原子性、可见性以及有序性提供了哪些保证
  - 对原子性的操作：对基本数据类型的变量的读取和赋值操作是原子性操作（参看博客中例子）
  - 对于可见性，Java提供了volatile关键字来保证可见性。
    当一个共享变量被volatile修饰时，它会保证修改的值会立即被更新到主存，
  当有其他线程需要读取时，它会去内存中读取新值。
  - 对于有序性：
    由happens-before原则保证：如果两个操作的执行次序无法从happens-before原则推导出来，
  那么它们就不能保证它们的有序性，虚拟机可以随意地对它们进行重排序。
    四条较重要的happens-before原则
    (1)程序次序规则：一个线程内，按照代码顺序，书写在前面的操作先行发生于书写在后面的操作
    (2)锁定规则：一个unLock操作先行发生于后面对同一个锁额lock操作
    (3)volatile变量规则：对一个变量的写操作先行发生于后面对这个变量的读操作
    (4)传递规则：如果操作A先行发生于操作B，而操作B又先行发生于操作C，则可以得出操作A先行发生于操作C
    
4.volatile关键字
  - 一旦一个共享变量（类的成员变量、类的静态成员变量）被volatile修饰之后，那么就具备了两层语义：
      >1）保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，
      这新值对其他线程来说是立即可见的。
      >2）禁止进行指令重排序。
  - volatile保证多线程可见性：多线程使用volatile关键字发生了什么
      > （1）线程1修改值（2）volatile强制将修改的值立即写入主存（3）线程2缓存变量无效
      （4）线程2重新读取变量
  - volatile不能保证对变量的操作是原子性
      > (1)线程1先读取了变量inc的原始值，被阻塞
        (2)线程2对变量进行自增操作
        (3)虽然此时线程2写入数据让线程1的数据无效，但是线程1已经读取过数据，并没有重新读取数据
        这样再进行增，得到和线程2同样的结果
        (4)两个线程执行自增最后加1
  - volatile能在一定程度上保证有序性
      > volatile变量是一个分隔符，执行到此的时候前边的指令一定执行结束，后边的指令一定还没有执行
      
5.使用volatile的场景
  - 状态标记量 
  - 单例模式中的double check（具体参考博客）
  
- 以下知识点(6)参考此博客[参考博客](https://www.cnblogs.com/baizhanshi/p/6419268.html)

6.synchronized的缺陷
  - 缺陷：获取锁的进程被阻塞了，但是又没有释放锁，那么其他线程便只能干巴巴地等待
  - Lock可以不让等待的线程一直无期限地等待下去（比如只等待一定的时间或者能够响应中断）
  
7.各种锁的函数用法参看上边的博客
  - void lock();
    void lockInterruptibly() throws InterruptedException;
    boolean tryLock();
    boolean tryLock(long time, TimeUnit unit) throws InterruptedException;
    void unlock();
    Condition newCondition();
    
8.锁的几个概念：可重入，可中断锁，公平锁
  - 可重入：锁基于线程的分配，而不是基于方法调用的分配。也就是一个线程获取到该锁之后，在其中再次需要该锁的时候
    不需要重新申请该锁。
  - 可中断锁：如果某一线程A正在执行锁中的代码，另一线程B正在等待获取该锁，可能由于等待时间过长，线程B不想等待
    了，想先处理其他事情，我们可以让它中断自己或者在别的线程中中断它，这种就是可中断锁。
  - 公平锁：即尽量以请求锁的顺序来获取锁，先到先得。
    
9.读写锁
  - 读写锁将对一个资源（比如文件）的访问分成了2个锁，一个读锁和一个写锁。
    正因为有了读写锁，才使得多个线程之间的读操作不会发生冲突。
    
#### 面试问题以及解答

1.volatile怎么保证可见性
  > （1）线程1修改值（2）volatile强制将修改的值立即写入主存（3）线程2缓存变量无效
    （4）线程2重新读取变量

2.synchronized和lock的区别
  - [参考博客](https://www.cnblogs.com/iyyy/p/7993788.html)
  - 在jvm层面：synchronized是java内置关键字,Lock是个java接口
  - synchronized无法判断是否获取锁的状态，Lock可以判断是否获取到锁(获取不到返回false)
  - synchronized会自动释放锁，Lock需在finally中手工释放锁
     >synchronized会自动释放锁时机：a 线程执行完同步代码会释放锁 ；b 线程执行过程中发生异常会释放锁
  - 用synchronized关键字的两个线程1和线程2，如果当前线程1获得锁，线程2线程等待。如果线程1阻塞，线程2则会一直等待下去；
    Lock锁就不一定会等待下去，如果尝试获取不到锁，线程可以不用一直等待就结束了；
  - synchronized的锁可重入、不可中断、非公平，而Lock锁可重入、可中断、可公平（两者皆可）
  - Lock锁适合大量同步的代码的同步问题，synchronized锁适合代码少量的同步问题
  

3.synchronized的底层实现
  - [参考博客](https://www.cnblogs.com/paddix/p/5367116.html)
    看此博客的代码示例
  - synchronized三种用法：
    （1）修饰普通方法
    （2）修饰静态方法
    （3）修饰代码块
  - Synchronized对代码块同步的原理：(0->1,1++,阻塞)
    > 每个对象有一个监视器锁（monitor）。
      当monitor被占用时就会处于锁定状态，线程执行monitorenter指令时尝试获取monitor的所有权
      1.如果monitor的进入数为0，则该线程进入monitor，然后将进入数设置为1，该线程即为monitor的所有者。    
      2.如果线程已经占有该monitor，只是重新进入，则进入monitor的进入数加1.
      3.如果其他线程已经占用了monitor，则该线程进入阻塞状态，直到monitor的进入数为0，再重新尝试获取monitor的所有权。
  - Synchronized对方法同步的原理(先检查标志位，再申请monitor对象)
    > 当方法调用时，调用指令将会检查方法的 ACC_SYNCHRONIZED 访问标志是否被设置，
    如果设置了，执行线程将先获取monitor，获取成功之后才能执行方法体，方法执行完后再释放monitor。
    在方法执行期间，其他任何线程都无法再获得同一个monitor对象。
    
4.sleep和wait的区别，sleep会不会释放锁
  - 参考例子
    [参考博客](https://www.cnblogs.com/hongten/p/hongten_java_sleep_wait.html)
    参考总结
    [参考博客](https://www.cnblogs.com/plmnko/archive/2010/10/15/1851854.html)
  - 对于sleep()方法，我们首先要知道该方法是属于Thread类中的
    而wait()方法，则是属于Object类中的。
    > sleep是Thread的静态类方法，谁调用的谁去睡觉，
      即使在a线程里调用了b的sleep方法，实际上还是a去睡觉，要让b线程睡觉要在b的代码中调用sleep。
  - sleep方法没有释放锁，而wait方法释放了锁
    > sleep使用interrupt()强行打断，而wait使用notify()或者notifyAll唤醒
  - sleep可以在任何地方使用
    wait，notify和notifyAll只能在同步控制方法或者同步控制块里面使用
  - sleep必须捕获异常
    wait，notify和notifyAll不需要捕获异常

5.notify和notifyAll的区别 
  - [参考博客](https://blog.csdn.net/djzhao/article/details/79410229)
  - notifyAll()方法是唤醒所有 wait 线程
    notify()方法是只随机唤醒一个 wait 线程
  - wait()方法:让进程进入该对象的等待池中，不会竞争该对象的锁
    notify()或者notifyAll()方法：进程从等待池进入锁池
    > (1)锁池:假设线程A已经拥有了某个对象(注意:不是类)的锁，而其它的线程想要调用这个对象的某个synchronized
      方法(或者synchronized块)，由于这些线程在进入对象的synchronized方法之前必须先获得该对象的锁的拥
      有权，但是该对象的锁目前正被线程A拥有，所以这些线程就进入了该对象的锁池中。
      (2)等待池:假设一个线程A调用了某个对象的wait()方法，线程A就会释放该对象的锁后，
      进入到了该对象的等待池中
  - [参考博客](https://www.jianshu.com/p/25e243850bd2?appinstall=0)
  - 永远都要把wait()放到循环语句里面:因为线程要执行，不仅需要被唤醒，还需要得到对象的锁
    ```
        while (mBuf.isFull()) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    ```
  - 应该尽量使用notifyAll()的原因就是，notify()非常容易导致死锁
    > 具体死锁过程见博客内容
      总体来说：是生产者A1生产，notify唤醒了消费者B1，A1结束
      这时候B1消费notify唤醒了B2,B2发现缓冲区为空继续wait,
      从此没有人执行notify,而A2和B2就一直在等待。
   
6.线程的局部变量
  - [参考博客](https://blog.csdn.net/qianyiyiding/article/details/77864118)
  - 总结：成员变量相互影响，局部变量没有影响
    如果一个变量是成员变量，那么多个线程对同一个对象的成员变量进行操作时，它们对该成员变量是彼此影响的，
    也就是说一个线程对成员变量的改变会影响到另一个线程。
    如果一个变量是局部变量，那么每个线程都会有一个该局部变量的拷贝（即便是同一个对象中的方法的局部变量，
    也会对每一个线程有一个拷贝），一个线程对该局部变量的改变不会影响到其他线程。
    ```
        /**
         * Java局部变量和成员变量代码演示
         */
        public class TTTTTT {
            public static void main(String[] args)
            {
                DemoThread  r = new DemoThread ();
                Thread t1 = new Thread(r);
                Thread t2 = new Thread(r);
                t1.start();
                t2.start();
                //可以看到控制打印了20次
            }
        }
        class DemoThread  implements Runnable {
           // int i;//这里成员变量,两个Thread共享了一个r,所以这里共享了一个i,打印次数一共20次数
            @Override
            public void run() {
                int i=0;//这里的i为局部变量,每个线程自己都有一个i,所以这里不会共享,打印的次数为40次
                while (true) {
                    System.out.println("测试打印次数: " + i++);
                    try {
                        Thread.sleep((long) Math.random() * 10000);
                    }
                    catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    if (20 == i) {
                        break;
                    }
                }
            }
        }
    ```

7.线程池参数 
  - corePoolSize：核心线程数
  - queueCapacity：任务队列容量（阻塞队列）
  - maxPoolSize：最大线程数
    > 当线程数>=corePoolSize，且任务队列已满时。线程池会创建新线程来处理任务
      当线程数=maxPoolSize，且任务队列已满时，线程池会拒绝处理任务而抛出异常

8.死锁产生的必要条件（互不球袋）
  - 互斥条件：一个资源每次只能被一个进程使用。
  - 不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺。
  - 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
  - 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。

9.什么情况会发生死锁，死锁的处理方法 
  - [参考博客](https://blog.csdn.net/qweqwruio/article/details/81359744)
  - 发生死锁的情况:指两个或多个线程之间，由于互相持有对方需要的锁，而永久处于阻塞的状态
  
  - [参考博客](https://www.cnblogs.com/sunting0706/p/5697209.html)
  - 死锁的处理方法
     > 预防死锁。该方法是通过设置某些限制条件，去破坏产生死锁的四个必要条件中的一个或几个来预防产生死锁。
       避免死锁。在资源的动态分配过程中，用某种方法防止系统进入不安全状态，从而可以避免产生死锁。
       检测死锁。通过检测机构及时地检测出死锁的发生，然后采取适当的措施，把进程从思索中解脱出来。
       解除死锁。当检测到系统中已发生死锁时，就采取相应的措施，将进程从死锁状态中解脱出来。常用方法是---撤销一些进程，回收他们的资源，将他们分配给已处于阻塞状态的进程，使其能继续运行。
  - 对应到java的解决方法：一种是用synchronized，一种是用Lock显式锁实现。
  
10.volatile和synchronized的区别 
  - volatile本质是在告诉jvm当前变量在寄存器（工作内存）中的值是不确定的，
    需要从主存中读取； synchronized则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。
  - volatile仅能使用在变量级别；synchronized则可以使用在变量、方法、和类级别的
  - volatile仅能实现变量的修改可见性，不能保证原子性；
    而synchronized则可以保证变量的修改可见性和原子性
  - volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。
  - volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化

11.线程等待时位于哪个区域
 
12.java实现多线程的方式 
  - [参考博客](https://www.cnblogs.com/felixzh/p/6036074.html)
  - 继承Thread类创建线程
  - 实现Runnable接口创建线程
  - 实现Callable接口通过FutureTask包装器来创建Thread线程
  - 使用ExecutorService、Callable、Future实现有返回结果的线程

13.进程和线程，什么是线程安全，怎么保证多线程线程安全

14.哪些是可重入锁
  - [参考博客](https://www.cnblogs.com/dj3839/p/6580765.html)
  - synchronized
    java.util.concurrent.locks.ReentrantLock

15.为什么要用线程池，线程池的好处

16.高并发怎么处理（没有回答上来）
