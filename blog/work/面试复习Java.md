# 面试复习

## 1 Java

## 1.1 面向对象

### 1.1.1 Java为啥是单继承？

因为要避免多继承的缺点，假如说可以多继承，那么可能会出现以下问题。
如果两个父类定义的相同的方法或者是(成员或者静态)变量，会冲突。
所以Java就是单继承。但是单继承会损失语言的灵活性，所以Java还有接口，一个类可以实现多个接口。
  
### 1.1.2 类

正常的类
内部类
静态内部类->Builder模式(：2.如果现在不能确定参数的个数，最好一开始就使用构建器即)。
匿名内部类-> 点击监听，在Koltin里面也叫Single Abstract Method。这里可以直接用Lambda来写

### 1.1.3 对象

#### 深入理解Class对象

##### 什么是Class对象

+ Class类也是类的一种，与class关键字是不一样的。

+ 手动编写的类被编译后会产生一个Class对象，其表示的是创建的类的类型信息，而且这个Class对象保存在同名.class的文件中(字节码文件)，比如创建一个Shapes类，编译Shapes类后就会创建其包含Shapes类相关类型信息的Class对象，并保存在Shapes.class字节码文件中。

+ 每个通过关键字class标识的类，在内存中有且只有一个与之对应的Class对象来描述其类型信息，无论创建多少个实例对象，其依据的都是用一个Class对象。

+ Class类只存私有构造函数，因此对应Class对象只能有JVM创建和加载

+ Class类的对象作用是运行时提供或获得某个对象的类型信息，这点对于反射技术很重要。

##### Class对象的加载

对象的加载时机：所有的类都是在对其第一次使用时动态加载到JVM中的：创建第一个对类的静态成员的引用、new操作符创建对象，这样就会加载这个被使用的类，实际上加载的就是这个类的字节码文件。

+ 获取Class对象引用的方式3种，通过继承自Object类的getClass方法，Class类的静态方法forName以及字面常量的方式”.class”。

+ 其中实例类的getClass方法和Class类的静态方法forName都将会触发类的初始化阶段，而字面常量获取Class对象的方式则不会触发初始化。

+ 初始化是类加载的最后一个阶段，也就是说完成这个阶段后类也就加载到内存中(Class对象在加载阶段已被创建)，此时可以对类进行各种必要的操作了（如new对象，调用静态成员等），注意在这个阶段，才真正开始执行类中定义的Java程序代码或者字节码

## 1.2 反射

### 应用：

+ Object-relational mapping 数据库：Hibernate，ROOM,Litepal
+ 其他的各种框架 Spring

## 1.3 泛型 Java SE5引入

### 应用

泛型类、泛型方法、泛型接口、通配符

### 好处：

代码上减少强制转换，提升类型转换的安全性。

### 类型擦除

```java
    List<String> l1 = new ArrayList<String>();
    List<Integer> l2 = new ArrayList<Integer>();
    System.out.println(l1.getClass() == l2.getClass());
```

上面的输出结果是什么呢？答案是true
Java泛型是JavaSE 1.5才出来的，所以为了兼容之前的代码，我们写的泛型代码在JVM中和之前的代码并没有任何区别；泛型信息只存在于代码编译阶段，在进入 JVM 之前，与泛型相关的信息会被擦除掉，专业术语叫做**类型擦除**。

打印的结果为 true 是因为 List<String>和 List<Integer>在 jvm 中的 Class 都是 List.class。
泛型信息被擦除了。

## 1.4 static相关问题

+ “static”关键字表明一个成员变量或者是成员方法可以在没有所属的类的实例变量的情况下被访问。
+ static 的属性和方法跟实例变量的无关，不能被重写/覆盖(overwrite)

### 静态变量、实例变量、构造方法的初始化顺序

```java
package work;

/**
 * <a href="https://blog.csdn.net/zhangxichao100/article/details/49300317"></>
 * 如果有子类的话，子类的静态初始顺序在父类的静态初始顺序之前，静态代码和变量的初始化是按照书写顺序
 * 来的
 */
public class InitialOrderTest {
    //    变量
    public String field = "变量";

    //编译期静态常量  这种变量不会触发初始化，因为
    /**
     * 变量时也没有触发初始化，这是因为staticFinal属于编译期静态常量，在编译阶段通过常量传播优化
     * 的方式将Initable类的常量staticFinal存储到了一个称为NotInitialization类的常量池中，
     * 在以后对Initable类常量staticFinal的引用实际都转化为对NotInitialization类对自身常量池的引用，
     * 所以在编译期后，对编译期常量的引用都将在NotInitialization类的常量池获取，这也就是引用编
     * 译期静态常量不会触发Initable类初始化的重要原因。
     * ---------------------
     * 作者：zejian_
     * 来源：CSDN
     * 原文：https://blog.csdn.net/javazejian/article/details/70768369
     * 版权声明：本文为博主原创文章，转载请附上博文链接！
     */
    static final int staticFinal = 47;

    //  静态变量
    public static String staticField = "静态变量";

    //  静态初始代码块
    static {
        System.out.println("静态初始化块");
        System.out.println("staticField:" + staticField);
    }

    //  静态变量
    public static String staticField2 = "静态变量2";

    //  静态初始代码块
    static {
        System.out.println("静态初始化块2");
        System.out.println("staticField:" + staticField);
        System.out.println("staticField2:" + staticField2);
    }


        //父类静态方法
    public static void Order() {
        System.out.print("父类静态方法");
        System.out.println("staticField:" + staticField);
    }



    //   初始化代码块
    {
        System.out.println("初始化代码块");
        System.out.println("field:" + field);
    }

    //   构造函数
    public InitialOrderTest() {
        System.out.println("构造器");

    }


    //所以执行顺序为： static代码按照顺序来-> 初始化代码块、实例变量也是按照代码顺序来->构造方法
    public static void main(String[] args) {
//        System.out.println("-----[[-------");
//        System.out.println(InitialOrderTest.staticField);
        InitialOrderTest.Order();

//        System.out.println("------]]------");
//        InitialOrderTest i = new InitialOrderTest();
//        System.out.println("-----[[-------");
//        System.out.println(InitialOrderTest.staticField);
//        InitialOrderTest.Order();
//        System.out.println("------]]------");

    }
}
```

## 1.5 集合

### 1.5.1 ArrayList和LinkdedList的区别

本质上是数组和双向链表的区别：数组查询的时候是O(1),插入的时候因为要移动位置是O(n)，链表找到位置后插入也是O(1)，查询的话需要O(n)。
当然插入的时候还是要看插入的位置来决定。

+ ArrayList无论在多大的数据量的情况下，获取元素的效率都相差不大。随机获取的效率总体上比LinkedList高

+ LinkedList和ArrayList在尾部添加数据的效率相差不大，头部添加则LinkedList明显优于ArrayList

+ ArrayList元素随机替换效率和查询效率相同，都比较高。LinkedList效率较低。

+ **随机插入的效率ArrayList反而比LinkedList高**。

### 1.5.2 【强制】不要在foreach循环里进行元素的remove/add操作。remove元素请使用Iterator方式，如果并发操作，需要对Iterator对象加锁。

因为foreach其实本质上是iterator的语法糖，iterator再遍历的时候有计数器，如果使用非iterator的方式移除元素就会造成计算混乱，然后就会报错java.util.ConcurrentModificationException

### 1.5.3 HashMap 1.7到1.8的变化 红黑树

+ HashMap在JDK1.7之前是由数组+链表组成的，1.8之后加入了红黑树；链表长度<8的时候，发生Hash冲突后会增加链表的长度，当链表长度大于8的时候，会先判断数组的容量，如果容量小于64会先扩容(原因是数组容量越小，越容易发生碰撞，因此当容量过小的时候，首先要考虑的是扩容)，如果容量大于等于64，则会将链表转化成红黑树以提升效率。

+ HashMap的容量是2的n次幂，无论在初始化的时候传入的初始容量是多少，最后都会转化为2的n次幂(或| 、无符号右移>>>)，这样做是为了在取模运算的时候使用&运算符，而不是%取余，可以极大的提升效率，同时也降低hash冲突。

+ HashMap是非线程安全的，在多线程的操作下会存在异常情况(如形成闭环(1.7),1.8已修复，但仍不安全)，可以使用HashTable或者ConcurrentHashMap进行代替。

+ 重写equals()后必须要重写hashCode()：不然会HashMap里面会出现2个或者是多个我们自己认为相同的key，导致key重复的问题。 因为HashMap的key值是根据hashCode来计算的，所以如果不重写hashCode()，只重写euqals()的话，就会出现在if判断语句里面两个对象相等，但是在HashMap里面又不相等的情况。

参考文章  

[面试必问的HashMap，你真的了解吗？](https://mp.weixin.qq.com/s/SHJzWpZ0MscuJhPLRwWQxg)

[重写equals方法的时候为什么需要重写hashcode](https://www.jianshu.com/p/75d9c2c3d0c1)

## 1.6 异常

Java异常分类：

+ Checked异常，编译的时候如果没有try-catch就会提示出错，必须捕获，

+ unchecked 异常->RuntimeException运行时异常，不用强行加try-catch，常见的运行时异常:NullPointerException,NumberFormatException,ArithmeticException,IndexOutOfBoundsException

Error：一般是JVM出现了一些错误，比如类加载失败呀什么的，会导致程序退出。

+ 当程序发生不可控的错误时，通常做法是通知用户并中止程序的执行。与异常不同的是Error及其子类的对象不应被抛出。

+ Error是throwable的子类，代表编译时间和系统错误，用于指示合理的应用程序不应该试图捕获的严重问题。

+ Error由**Java虚拟机**生成并抛出，包括动态链接失败，虚拟机错误等。程序对其不做处理。

## 1.7 多线程

### 1.7.1 线程基础

### 1.7.2 进程与线程的区别

|指标 |进程|线程|
|----|----|---|
|任务大小 |大|小 |
|地址空间 |不同的进程不能共享地址空间|在同一进程中的线程可以共享地址空间，比如读写相同的数据结构和变量|
|实质|线程和进程实际上都是一个时间段的描述，即CPU工作时间段的描述|线程和进程实际上都是一个时间段的描述，即CPU工作时间段的描述|
|定义|进程就是程序的实体、是**受操作系统管理的基本运行单元**|线程是**受操作系统调度**的最小单元|

#### 线程的状态

新创建 new

运行状态 runnanle

等待状态 waiting

超时等待状态 timed waiting

阻塞状态 blokced

终止状态 terminated

![线程的状态](/blog/res/2.png)

<center>状态之间的转换图与相关的方法调用 </center>

#### 理解终端方法和安全的终止线程

interrupted()会在阻塞方法调用处抛出InterruptedException

### 1.7.3 同步

#### synchronize同步方法

#### synchronize同步代码块

#### ReentrantLock

+ 可以指定是公平锁还是非公平锁

#### volatile (adj. [化学] 挥发性的；不稳定的；爆炸性的；反复无常的)

+ Java的内存模型：
![Java的内存模型](/blog/res/3.png)
<center>Java Memory Model </center>

+ 原子性、可见性和有序性。

    原子性即某个操作是不能被中断的，比如变量的取值和赋值。

    可见性：用volatile修饰一个共享变量后，当这个变量的值改变后，另外的线程能马上看到这个变量的值发生了改变。

    有序性：编译器和处理器会对指令进行重排序，在单线程的情况下是没有问题的，但是在并发的时候就会出现问题。所以需要保证代码的有序性。比如使用volatile、synchronize和Lock，这样就可以保证每个时刻只有一个线程在**同步代码**执行代码，从而保证有序性。

+ volatile关键字：使用volatize修饰的共享变量在修改后对其他线程是立即可见的。

    volatile不保证原子性：比如有两个线程A、B在修改一个用volatile修饰的共享变量x(x=1)的时候，可能A、B都同时拿到了x的版本1的值，然后A先对x进行了修改(x++,现在x为2)，这个时候B是可以看到修改后的值了(x=2)，但是因为B已经拿到了版本1的值,所以B会在x版本1的基础上进行修改(x++)，最后的结果为2。这个现象说明了原子性和volatile是没啥关系的, volatile只能保证可见性：即在某个线程更新了用volatile修饰的共享变量之后，其他线程在之后获取到的变量都是最新的值，而不会是旧的值。

    volatile保证有序性：volatile会禁止重排序，什么是重排序。重排序就是编译器和处理器对指令进行优化。如果变量加了volatile那么，那么对变量写入的这条指令就不会和其他指令进行重排。而且还会在这条指令之后加入一条Write-Barrier写入屏障的指令，来清空缓冲区，然后在读取变量之前插入Read-Barrier指令，来保证CPU上的所有线程都能读到最新的数据。

参考资料：
[全面理解Java内存模型](https://blog.csdn.net/suifeng3051/article/details/52611310)

### 1.7.4 阻塞队列

主要用于消费者生产者模型

### 1.7.5 线程池

常见线程池：固定大小、单线程、定时任务

### 1.7.6 AsyncTask

## 1.8 gc

### 内存模型：

堆、方法区(类信息、常量、静态变量等)、栈(虚拟机栈、本地方法栈)、程序计数器、native之间内存

+ 堆：分为新生代和老年代；新生代又分为：Eden，和两个Survivor区

### 什么时候(触发)进行垃圾回收？

内存区域快满了就会触发

### 哪些内存需要回收？

引用计数法、可达性分析法

### 怎么回收？

标记-清除算法(慢、碎片)，

复制算法(8:1,90%)  ->新生代

标记-整理算法（没有碎片、慢）->老年代

分代收集算法。

新生代GC(Minor GC)：把Eden对象转移到Survivor中去，担保机制会把对象转移到老年代去。

老年代GC: Major GC/Full GC

## 1.9 类加载机制

### 1.9.1 类加载的过程

loading -> [vertification-> preparation-> resolution] -> initialization->using->unloading

### 1.9.2 ClassLoader

#### 双亲委派模型。

+ 优点：Java类随着类加载器有了一种优先级的关系。

+ 工作过程：当子加载器需要对类进行加载的时候，它会首先委托给父加载器进行加载，如果加载失败子加载器才会尝试自己去加载。

#### 破坏双亲委派模型

+ 第一次破坏：为了向前兼容。解决：loadClass()->findClass() JDK1.2

+ 第二次破坏：基础类要调用用户的代码，调用独立产商提供的代码比如，JDBC,JNDI等。解决：线程上下文切换器。

+ 第三次破坏：用户对程序的动态性追求：热部署、热替换等。就是希望程序能像计算机外设那样，随时替换和插拔，不用重启。

## 2 设计模式

阿松大
