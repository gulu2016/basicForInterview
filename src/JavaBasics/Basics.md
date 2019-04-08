### 面试的java基础
1.String、StringBuffer、StringBuilder的区别
   - [参考博客](https://www.cnblogs.com/xudong-bupt/p/3961159.html)
   - 可变与不可变
     String 是不可变的
     StringBuffer和StringBuilder是可变的
   - 多线程安全
     String是线程安全的
     StringBuffer是线程安全的
     StringBuilder是线程不安全的
     (StringBuffer和StringBuilder是继承共同父类AbstractStringBuilder的)
2.怎么理解String不变性 
   -  String 是不可变的对象, 因此在每次对 String 类型进行改变的时候其实都等同于生成了一个
      新的 String 对象，然后将指针指向新的 String 对象
3.==和equals的区别，如果重写了equals()不重写hashCode()会发生什么
   - [参考博客](https://www.cnblogs.com/smyhvae/p/3929585.html)
   - == 的作用：
     　　基本类型：比较的就是值是否相同
     　　引用类型：比较的就是地址值是否相同
     equals 的作用:
     　　引用类型：默认情况下，比较的是地址值。(一般都会重写)
   - 字符串比较之中“==”和equals()的区别
     ==：比较的是两个字符串内存地址（堆内存）的数值是否相等，属于数值比较；
     equals()：比较的是两个字符串的内容，属于内容比较。
   ![字符串==比较示意图](https://images0.cnblogs.com/blog/641601/201408/221553240496054.png)
   - 总结：
     (1)==是值比较，equals()是内容比较
     (2)每次new 对象都会创建新对象，引用在栈中，对象实体在堆中，每次new的实体地址值都不一样
        String a = new String("abc");
        String b = new String("abc");
        a==b返回false,因为地址值不一样
        a.equals(b)返回true,因为内容一样
     (3)String a = "abc"，a在栈中，"abc"在常量池中，String b = "abc"
        b存储的地址和a存储的地址值是一样的，所以a==b返回true
        a.equals(b)返回true，因为内容一样
   - 为什么重写了equals()一定要重写hashCode()
     [参考博客](https://www.cnblogs.com/ouym/p/8963219.html)
     这是为了遵守：2个对象equals，那么 hashCode一定相同规则
     否则会出现两个对象equals返回true，但是存储在hashMap的不同位置
4.java为什么不能多继承 
   - [参考博客](https://blog.csdn.net/qq_36084640/article/details/83903252)
   - 不能多继承是因为可能出现调用歧义
   - 可以多实现是因为即使接口有相同方法，在子类中也要实现，调用的还是子类中的方法
5.讲一下java抽象类和接口 
   - [参考博客](http://www.importnew.com/12399.html)
   - 抽象类和接口的对比(抽象类vs接口)
     (1)默认的方法实现:它可以有默认的方法实现 vs 接口完全是抽象的。它根本不存在方法的实现
     (2)实现：如果子类不是抽象类的话，它需要提供抽象类中所有声明的方法的实现。 
          vs
          子类使用关键字implements来实现接口。它需要提供接口中所有声明的方法的实现
     (3)访问修饰符:public、protected和default vs 只有public
     (4)多继承:只可以继承一个类 vs 可以实现多个接口 
   - 如果你拥有一些方法并且想让它们中的一些有默认实现，那么使用抽象类吧。
     如果你想实现多重继承，那么你必须使用接口。
   - 总结：除了上边的四点对比，接口是一种行为(走，跑)，抽象类是一种类型(动物，水果)
6.java中为什么要写非static方法
   - [参考博客](https://blog.csdn.net/shengzhu1/article/details/70233057) 
   - 静态方法和非静态方法的区别
     (1)静态方法是共享代码段，静态变量是共享数据段。
        既然是“共享”就有并发（Concurrence）的问题。
        非静态方法是针对确定的一个对象的，所以不会存在线程安全的问题。
     (2)静态方法（Static Method）与静态成员变量一样，属于类本身，
        在类装载的时候被装载到内存（Memory），不自动进行销毁，会一直存在于内存中，直到JVM关闭。
        非静态方法（Non-Static Method）又叫实例化方法，属于实例对象，实例化后才会分配内存，
        必须通过类的实例来引用。不会常驻内存，当实例对象被JVM 回收之后，也跟着消失。 
   - 总结就是
     (1)static方法占用内存，不会自动销毁
     (2)静态方法是共享的代码段，可能出现线程安全的问题
7.讲一下java IO
   - [参考博客](https://www.cnblogs.com/fysola/p/6123947.html)
