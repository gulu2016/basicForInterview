### 容器
1.关于HashMap
   - [参考博客](https://blog.csdn.net/l_bestcoder/article/details/79228421)
   - 存储的数据结构：
     (1)HashMap是一个存储Node<K,V>的数组，数组中每一个位置都是一个Node构成的链表。
     (2)Node链表采用的是头插法，当链表长度大于8的时候要转化为红黑树
     (3)在JDK1.7中通过键值对Entry<K,V>中的next属性来把hash冲突的所有Entry连接起来，增删改查操作的时间复杂度为O（n）
     而在JDK1.8中，当链表长度大于8时，会将链表转化为一棵红黑树，增删改查操作的时间复杂度为O（log（n））。
     ![存储结构示意图](https://img-blog.csdn.net/20180201152654448?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTF9CZXN0Q29kZXI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast )
   - 求key的hash值
     （1）计算对象自己的hashCode()值
     （2）计算上步得到的值高16位于低16位相与。否则在容器length较小时，无法发挥高位的作用，这样能使 得hash分布更加均匀，减少冲突。
     （3）定位操作，对上步计算得到hash值进行取模操作
   - 扩容操作
     （1）每次扩容为之前的两倍
     （2）在扩容之后就要把具体键值对搬迁到新的table数组中
   - put方法具体流程
     注意事项：
     (1)key->hashCode->目标的红黑树或者链表
     (2)目标的红黑树或者链表中没有该key，直接插入红黑树或者链表中
        目标的红黑树或者链表中有该key，就直接覆盖原来的值
     ![put方法具体流程](https://img-blog.csdn.net/20180202160207804?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTF9CZXN0Q29kZXI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
2.红黑树
   - [参考博客](https://www.cnblogs.com/liwei2222/p/8013367.html)
   - 定义
     (1)红黑树是一种近似平衡的二叉查找树，它能够确保任何一个节点的左右子树的高度差不会超过二者中较低那个的一陪。
        具体来说，红黑树是满足如下条件的二叉查找树（binary search tree）：
         (1.1)每个节点要么是红色，要么是黑色。
         (1.2)根节点必须是黑色
         (1.3)红色节点不能连续（也即是，红色节点的孩子和父亲都不能是红色）。
         (1.4)对于每个节点，从该点至null（树尾端）的任何路径，都含有相同个数的黑色节点。
     (2)总结红黑树的定义：红不连续，黑相等
   - 插入和删除操作可以参考二三树
     ![红黑树的插入与二三树的插入比较](https://images2017.cnblogs.com/blog/1292434/201712/1292434-20171210003319982-1924815972.png)
   - 为什么hashMap采用红黑树结构
     (1)首先红黑树是近似的平衡二叉树，查询效率是O(log(n)),比链表高.
     (2)红黑树相比avl树，在检索的时候效率其实差不多，都是通过平衡来二分查找。
        但对于插入删除等操作效率提高很多。红黑树不像avl树一样追求绝对的平衡，省去了很多没有必要的调平衡操作，效率提高。

3.ConcurrentHashMap怎么保证线程安全
   - HashMap为什么是线程不安全的
     [参考博客](https://blog.csdn.net/loveliness_peri/article/details/81092360)
     HashMap在put的时候，插入的元素超过了容量（由负载因子决定）的范围就会触发扩容操作，
     就是rehash，这个会重新将原数组的内容重新hash到新的扩容数组中，在多线程的环境下，
     存在同时其他的元素也在进行put操作，如果hash值相同，可能出现同时在同一数组下用链表表示，
     造成闭环，导致在get时会出现死循环，所以HashMap是线程不安全的。
   - ConcurrentHashMap怎么保证线程安全
     [参考博客](https://blog.csdn.net/l_bestcoder/article/details/79316408)
     (1)ConcurrentHashMap把整个容器很为许多个Segment（段）,段数就是并发度，
       每个Segment类似于一个Hashtable，管理一个HashEntry数组，
       每个HashEntry是一个链表头，或者红黑树的根
       ![ConcurrentHashMap结构](https://blog.csdn.net/l_bestcoder/article/details/79316408)
     (2)对于像size()操作需要全局加锁的操作，CurrentHashMap处理方式
         (2.1)在不对任何Segment加锁的情况下，遍历每个Segment,并且求和，如果三次中有相邻两次求得的
         每个Segment都相等，那么说明在这两次期间，每个Segment都没有被更新，那么可以直接返回size。
         (2.2)若三次都不相等，那么就全部加锁，求和。
     (3)总结就是将整个容器分为许多Segment，由原来的对整个HashMap上锁改为对Segment上锁。
4.HashMap和HashTable的区别
    (1)HashMap是线程不安全的，HashTable是线程安全的
        [参考博客](https://blog.csdn.net/wobushixiaobailian/article/details/84074885)
        (1.1)为什么Hashtable是线程安全的，因为它的remove,put，get做成了同步方法，保证了Hashtable的线程安全性。
        (1.2)每个操作数据的方法都进行同步控制之后，由此带来的问题任何一个时刻只能有一个线程可以操纵Hashtable，所以其效率比较低。
    (2)继承的父类不同
       [参考博客](https://www.cnblogs.com/williamjie/p/9099141.html)
    (3)内部实现使用的数组初始化和扩容方式不同
    (4)hash值不同:哈希值的使用不同，HashTable直接使用对象的hashCode。而HashMap重新计算hash值。
3.HashSet的底层实现，是不是线程安全的
   - [参考博客](https://www.cnblogs.com/skillking/p/7250606.html)
   - 底层使用HashMap来保存HashSet中所有元素。 
   - 不是线程安全的
4.ArrayList和LinkedList的区别，是不是线程安全的
   - [参考博客](https://www.cnblogs.com/sierrajuan/p/3639353.html)
   - ArrayList和Vector使用了数组的实现，可以认为ArrayList或者Vector封装了对内部数组的操作
   - LinkedList使用了循环双向链表数据结构。
   - ArrayList和LinkedList都不是线程安全的
   - CopyOnWriteArrayList，ConcurrentLinkedQueue是线程安全的