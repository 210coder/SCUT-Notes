## 什么是线程和进程
1. 进程是系统进行资源分配的基本单位，而线程是CPU资源调度的最小单位。
因为cpu资源比较特殊，它是分配到线程的，占用cpu运行的是线程。
2. 一个进程中有多个线程，多个线程共享进程中的堆和方法区资源，每个线程有自己的**程序计数器、虚拟机栈和本地方法栈**。
3. 在 Java 中，当我们启动 main 函数时其实就是启动了一个 JVM 的进程，而 main 函数所在的线程就是这个进程中的一个线程，也称主线程。一个 Java 程序的运行是 main 线程和多个其他线程同时运行。



## Java中创建线程的四种方法

[link](https://www.cnblogs.com/zhou-test/p/9811771.html)

https://cloud.tencent.com/developer/article/1038547

 在JDK1.5之前，创建线程就只有两种方式，即继承java.lang.Thread类和实现java.lang.Runnable接口；而在JDK1.5以后，增加了两个创建线程的方式，即实现java.util.concurrent.Callable接口和线程池

1. 定义一个类继承Thread类并重写run方法（Java不支持多继承）

2.定义一个类实现Runnable接口，并重写run方法

3.实现Callable接口，并重写call方法

3.使用线程池去创建



## Runnable接口和Callable接口的区别：
Runnable自 Java 1.0 以来一直存在，但Callable仅在 Java 1.5 中引入,目的就是为了来处理Runnable不支持的用例。Runnable 接口 不会返回结果或抛出检查异常，但是 Callable 接口 可以。所以，如果任务不需要返回结果或抛出异常推荐使用 Runnable 接口 ，这样代码看起来会更加简洁。
Runnable.java

```java
@FunctionalInterface
public interface Runnable {
   /**
    * 被线程执行，没有返回值也无法抛出异常
    */
    public abstract void run();
}
```
Callable
```java
@FunctionalInterface
public interface Callable<V> {
    /**
     * 计算结果，或在无法这样做时抛出异常。
     * @return 计算得出的结果
     * @throws 如果无法计算结果，则抛出异常
     */
    V call() throws Exception;
}
```


## 并发与并行的区别
并发： 同一时间段，多个任务都在执行 (单位时间内不一定同时执行)；
并行： 单位时间内，多个任务同时执行。

## 线程的生命周期和状态

![image-20220728110252956](java并发面经/image-20220728110252956.png)

![image-20220728110328288](java并发面经/image-20220728110328288.png)
由上图可以看出：线程创建之后它将处于 NEW（新建） 状态，调用 start() 方法后开始运行，线程这时候处于 READY（就绪） 状态。可运行状态的线程获得了 CPU 时间片（timeslice）后就处于 RUNNING（运行） 状态。
在操作系统中层面线程有 READY 和 RUNNING 状态，而在 JVM 层面只能看到 RUNNABLE 状态，所以 Java 系统一般将这两个状态统称为 **RUNNABLE**（运行中） 状态 。

当线程执行 wait()方法之后，线程进入 **WAITING**（等待） 状态。进入等待状态的线程需要依靠其他线程的通知才能够返回到运行状态，而 **TIMED_WAITING**(超时等待) 状态相当于在等待状态的基础上增加了超时限制，比如通过 sleep（long millis）方法或 wait（long millis）方法可以将 Java 线程置于 **TIMED_WAITING** 状态。当超时时间到达后 Java 线程将会返回到 **RUNNABLE** 状态。当线程调用同步方法时，在没有获取到锁的情况下，线程将会进入到 **BLOCKED**（阻塞） 状态。线程在执行 Runnable 的run()方法之后将会进入到 **TERMINATED**（终止） 状态。



注意：调用wait()方法 sleep方法线程进入等待状态的！不是阻塞状态

## 线程上下文切换
线程在执行过程中会有自己的运行条件和状态（也称上下文），比如上文所说到过的程序计数器，栈信息等。当出现如下情况的时候，线程会从占用 CPU 状态中退出。

1. 主动让出 CPU，比如调用了 sleep(), wait() 等。
2. 时间片用完，因为操作系统要防止一个线程或者进程长时间占用CPU导致其他线程或者进程饿死。
3. 调用了阻塞类型的系统中断，比如请求 IO，线程被阻塞。
4. 被终止或结束运行
这其中前三种都会发生线程切换，线程切换意味着需要保存当前线程的上下文，留待线程下次占用 CPU 的时候恢复现场。并加载下一个将要占用 CPU 的线程上下文。这就是所谓的 上下文切换。

上下文切换是现代操作系统的基本功能，因其每次需要保存信息恢复信息，这将会占用 CPU，内存等系统资源进行处理，也就意味着效率会有一定损耗，如果频繁切换就会造成整体效率低下。

## 什么是线程死锁
死锁是指两个或两个以上的线程在执行过程中，因争夺资源而造成的相互等待的现象，多个线程同时被阻塞。
如线程 A 持有资源 2，线程 B 持有资源 1，他们同时都想申请对方的资源，所以这两个线程就会互相等待而进入死锁状态。

操作系统产生死锁必须具备以下四个条件：

- 互斥条件：该资源任意一个时刻只由一个线程占用。
- 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
- 不剥夺条件:线程已获得的资源在未使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才释放资源。
- 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。
## 如何预防和避免线程死锁?
破坏死锁的产生的必要条件即可：

-  破坏请求与保持条件 ：一次性申请所有的资源。
- 破坏不剥夺条件 ：占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源。
- 破坏循环等待条件 ：靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件。

## 如何避免死锁？

避免死锁就是在资源分配时，借助于算法（比如银行家算法）对资源分配进行计算评估，使其进入安全状态。
安全状态 指的是系统能够按照某种进程推进顺序（P1、P2、P3.....Pn）来为每个进程分配所需资源，直到满足每个进程对资源的最大需求，使每个进程都可顺利完成。称<P1、P2、P3.....Pn>序列为安全序列。

## sleep() 方法和 wait() 方法区别和共同点
- 两者最主要的区别在于：sleep()方法没有释放锁，而 wait() 方法释放了锁 。
- 两者都可以暂停线程的执行。
- wait() 通常被用于线程间交互/通信，sleep() 通常被用于暂停执行。
- wait() 方法被调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的 notify() 或者 notifyAll() 方法。sleep() 方法执行完成后，线程会自动苏醒。或者可以使用 wait(long timeout) 超时后线程会自动苏醒。



## 为什么我们调用 start() 方法时会执行 run() 方法，为什么我们不能直接调用 run() 方法？
这是另一个非常经典的 Java 多线程面试问题，而且在面试中会经常被问到。很简单，但是很多人都会答不上来！

new 一个 Thread，线程进入了新建状态。调用 start()方法，会启动一个线程并使线程进入了就绪状态，当分配到时间片后就可以开始运行了。 start() 会执行线程的相应准备工作，然后自动执行 run() 方法的内容，这是真正的多线程工作。 但是，直接执行 run() 方法，会把 run() 方法当成一个 main 线程下的普通方法去执行，并不会在某个线程中执行它，所以这并不是多线程工作。

**总结： 调用 start() 方法方可启动线程并使线程进入就绪状态，直接执行 run() 方法的话不会以多线程的方式执行。**


## 并发编程的三个重要特性

1. **原子性** : 一个的操作或者多次操作，要么所有的操作全部都得到执行并且不会受到任何因素的干扰而中断，要么所有的操作都执行，要么都不执行。`synchronized` 可以保证代码片段的原子性。
2. **可见性** ：当一个线程对共享变量进行了修改，那么另外的线程都是立即可以看到修改后的最新值。`volatile` 关键字可以保证共享变量的可见性。
3. **有序性** ：代码在执行的过程中的先后顺序，Java 在编译器以及运行期间的优化，代码的执行顺序未必就是编写代码时候的顺序。`volatile` 关键字可以禁止指令进行重排序优化。



## 1. 说说synchronized 关键字
synchronized是java提供的一个原子性内置锁，线程的执行代码在进入synchronized代码块时会自动获取到内部锁（java内置的使用者看不到的锁），也叫监视器锁，其他线程访问该同步代码块时会被阻塞挂起。synchronized关键字可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。



## synchronized 关键字主要的三种使用方式
**1.修饰实例方法:** 作用于当前对象实例加锁，进入同步代码前要获得 **当前对象实例的锁**

```java
synchronized void method() {
    //业务代码
}
```

**2.修饰静态方法:** 也就是给当前类加锁，会作用于类的所有对象实例 ，进入同步代码前要获得 **当前 class 的锁**。因为静态成员不属于任何一个实例对象，是类成员（ _static 表明这是该类的一个静态资源，不管 new 了多少个对象，只有一份_）。所以，如果一个线程 A 调用一个实例对象的非静态 `synchronized` 方法，而线程 B 需要调用这个实例对象所属类的静态 `synchronized` 方法，是允许的，不会发生互斥现象，**因为访问静态 `synchronized` 方法占用的锁是当前类的锁，而访问非静态 `synchronized` 方法占用的锁是当前实例对象锁**。

```java
synchronized static void method() {
    //业务代码
}
```

**3.修饰代码块** ：指定加锁对象，对给定对象/类加锁。`synchronized(this|object)` 表示进入同步代码库前要获得**给定对象的锁**。`synchronized(类.class)` 表示进入同步代码前要获得 **当前 class 的锁**

```java
synchronized(this) {
    //业务代码
}
```

**总结：**

- `synchronized` 关键字加到 `static` 静态方法和 `synchronized(class)` 代码块上都是给 Class 类上锁。
- `synchronized` 关键字加到实例方法上是给对象实例上锁。
- 尽量不要使用 `synchronized(String a)` 因为 JVM 中，字符串常量池具有缓存功能！

synchronized除了可以解决共享变量的内存可见性问题外，还可以用来实现原子性操作，synchronized关键字会引起线程上下文切换并带来线程调度开销。

## 面试题
面试中面试官经常会说：“单例模式了解吗？来给我手写一下！给我解释一下双重检验锁方式实现单例模式的原理呗！”

**双重校验锁实现对象单例（线程安全）**

```java
public class Singleton {

    private volatile static Singleton uniqueInstance;

    private Singleton() {
    }

    public  static Singleton getUniqueInstance() {
       //先判断对象是否已经实例过，没有实例化过才进入加锁代码
        if (uniqueInstance == null) {
            //类对象加锁
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```

1. 由于**构造函数私有**，我们获取Singleton类的实例化对象只能通过Singleton.getUniqueInstance()的方式，而无法在外界通过new或者其他方法创建此类的实例，并且由于此成员变量被static修饰，使得实例对象属于类本身且只有唯一一个。
2. volatile关键字可以防止jvm指令重排
> 因为 uniqueInstance = new Singleton() 这句话可以分为三步：
1.为 singleton 分配内存空间；
2.初始化 singleton；
3.将 singleton 指向分配的内存空间。
但是由于JVM具有指令重排的特性，执行顺序有可能变成 1-3-2。 指令重排在单线程下不会出现问题，但是在多线程下会导致一个线程获得一个未初始化的实例。例如：线程T1执行了1和3，此时T2调用 getInstance() 后发现 singleton 不为空，因此返回 singleton， 但是此时的 singleton 还没有被初始化。
使用 volatile 会禁止JVM指令重排，从而保证在多线程下也能正常执行。

3. synchronized加在getUniqueInstance方法上这样虽然能保证只有一个线程执行getUniqueInstance()方法，但是缺点在于锁的粒度太大了，多个线程同时调用getUniqueInstance()时，其他线程都会被阻塞，毕竟大多数情况下对象已经存在。
4. 第二次判断是为了防止多个线程同时完成第一次判断为null，A线程获得锁后new一个对象释放锁，此时B线程也获得锁如果没有第二次判断B线程也会new一个对象。



## 2. 讲一下 synchronized 关键字的底层原理
synchronized 关键字底层原理属于 JVM 层面。
- synchronized 同步语句块的实现使用的是 monitorenter 和 monitorexit 指令，其中 monitorenter 指令指向同步代码块的开始位置，monitorexit 指令则指明同步代码块的结束位置。
>1. 在执行monitorenter时，会尝试获取对象的锁，如果锁的计数器为 0 则表示锁可以被获取，获取后将锁计数器设为 1 也就是加 1。
>2. 在执行 monitorexit 指令后，将锁计数器设为 0，表明锁被释放。如果获取对象锁失败，那当前线程就要阻塞等待，直到锁被另外一个线程释放为止。
- synchronized 修饰的方法并没有 monitorenter 指令和 monitorexit 指令，取得代之的确实是 ACC_SYNCHRONIZED 标识，该标识指明了该方法是一个同步方法。JVM 通过该 ACC_SYNCHRONIZED 访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。

不过两者的本质都是对对象监视器 monitor 的获取。

synchronized通过Monitor来实现线程同步，Monitor是依赖于底层的操作系统的Mutex Lock（互斥锁）来实现的线程同步。

总结：

同步代码块

- **通过`monitorenter`与`monitorexit`指令获取和释放对象的监控器锁( monitor )**

同步方法

- **当线程执行到方法后会检查是否有ACC_SYNCHRONIZED这个标志，有的话隐式去调用`monitorenter`与`monitorexit`指令，也是对监控器锁的获取和释放**



## JDK1.6后 synchronized 增加了锁的升级

 ![img](java并发面经/34612537_1637132349942_552B7C2C7D899D6C4FD48728415A6077)

synchronized为重量级锁, 线程的阻塞和唤醒需要操作系统用户态和内核态的切换，耗费时间，所以锁的升级是为了尽量避免这种时间消耗。

不得已才使用重量级锁，也就是最终

**Synchronized锁升级的过程。 主要步骤是：**

- 先是通过偏向锁来获取锁，解决了虽然有同步但无竞争的场景下锁的消耗。
- 再是**通过对象头的Mark Word**来实现的轻量级锁，通过轻量级锁如果还有竞争，那么继续升级。
- 升级为自旋锁，如果达到最大自旋次数了，那么就直接升级为重量级锁，所有未获取锁的线程都阻塞等待。



## 说说 JDK1.6 之后的 synchronized 关键字底层做了哪些优化，可以详细介绍一下这些优化吗

[link](https://www.nowcoder.com/discuss/839862?type=all&order=recall&pos=&page=1&ncTraceId=&channel=-1&source_id=search_all_nctrack&gio_id=0B4AEC2B6C443237D83E9B6E4B928204-1645864140400)

JDK1.6 对锁的实现引入了大量的优化，锁粗化、锁消除、锁升级如偏向锁、轻量级锁、自旋锁、适应性自旋锁等技术来减少锁操作的开销。

锁主要存在四种状态，依次是：无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态，他们会随着竞争的激烈而逐渐升级。注意锁可以升级不可降级，这种策略是为了提高获得锁和释放锁的效率。

**锁粗化**就是将「**多个连续的加锁、解锁操作连接在一起**」，扩展成一个范围更大的锁，避免频繁的加锁解锁操作。

**锁消除**是指去除不可能存在共享资源竞争的锁，通过这种方式消除没有必要的锁，可以节省毫无意义的时间消耗。



### 锁升级

`Synchronized` 锁的升级顺序是 「**无锁-->偏向锁-->轻量级锁-->自旋锁-->重量级锁，只会升级不会降级**」

`Java`中每个对象都拥有**对象头**，对象头由`Mark World` 、指向类的指针`Klass Point`、以及数组长度三部分组成，本文，我们只需要关心`Mark World` 即可，

**Mark World: **记录了对象的`HashCode`、分代年龄和**锁标志位信息**。

**Klass Point**：对象指向它的类元数据的指针，虚拟机通过这个指针来确定这个对象是哪个类的实例。

锁的升级变化，体现在锁对象的对象头`Mark World`部分，也就是说`Mark World`的内容会随着锁升级而改变。

![image-20220407114911625](java并发面经/image-20220407114911625.png)



1. **偏向锁**

同一个线程执行同步资源时自动获取锁, 降低获取锁的代价。

在大多数情况下，锁总是由同一线程多次获得，不存在多线程竞争，所以出现了偏向锁，其目标就是在只有一个线程执行同步代码块时，降低获取锁带来的消耗，提高性能

轻量级锁的获取及释放依赖多次CAS原子指令，而偏向锁只需要在置换ThreadID的时候依赖一次CAS原子指令即可



2. **轻量级锁**

多个线程竞争同步资源时，**没有获取资源的线程自旋等待锁释放，不会阻塞**

轻量级锁考虑的是竞争锁对象的线程不多，持有锁时间也不长的场景。

当前线程持有的锁是偏向锁的时候，被另外的线程所访问，偏向锁就会升级为轻量级锁，其他线程会通过**自旋的形式尝试获取锁，不会阻塞**，从而提高性能。

细节：

在代码进入同步块的时候,  虚拟机首先将在当前线程的栈帧中建立一个名为锁记录, 用于存储锁对象目前的Mark Word的拷贝，然后拷贝对象头中的Mark Word复制到锁记录中。拷贝成功后，虚拟机将使用CAS操作尝试将对象的Mark Word更新为指向Lock Record的指针，并将Lock Record里的owner指针指向对象的Mark Word。如果这个更新动作成功了，那么这个线程就拥有了该对象的锁



3. **自旋锁**

未获取锁的线程“稍等一会”，但不放弃处理器执行时间，只需要让线程执行一个忙循环（自旋），这就是所谓的自旋锁。



4. **重量级锁**

多个线程竞争同步资源时，**没有获取资源的线程阻塞等待被唤醒**

当自旋超过一定的次数，或者一个线程在持有锁，一个在自旋，又有第三个来访时，轻量级锁升级为重量级锁。

轻量级锁膨胀之后，就升级为重量级锁，重量级锁是依赖操作系统的`MutexLock`（**互斥锁**）来实现的，需要从用户态转到内核态。



综上，偏向锁通过对比Mark Word解决加锁问题，避免执行CAS操作。而轻量级锁是通过用CAS操作和自旋来解决加锁问题，避免线程阻塞和唤醒而影响性能。重量级锁是将除了拥有锁的线程以外的线程都阻塞。

## Java的锁

https://mp.weixin.qq.com/s?__biz=MjM5NjQ5MTI5OA==&mid=2651749434&idx=3&sn=5ffa63ad47fe166f2f1a9f604ed10091&chksm=bd12a5778a652c61509d9e718ab086ff27ad8768586ea9b38c3dcf9e017a8e49bcae3df9bcc8&scene=38#wechat_redirect



# 乐观锁和悲观锁

线程要不要锁住同步资源

**悲观锁**

总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁（**共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程**）。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。Java中`synchronized`和`ReentrantLock`  `Lock`等独占锁就是悲观锁思想的实现。

**乐观锁**

总是假设最好的情况，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以**使用版本号机制和CAS算法**实现。**乐观锁适用于多读的应用类型，这样可以提高吞吐量**，像数据库提供的类似于**write_condition机制**，其实都是提供的乐观锁。在Java中`java.util.concurrent.atomic`包下面的**原子变量类**就是使用了乐观锁的一种实现方式**CAS**实现的。

**乐观锁适用于多读少写**

### CAS与synchronized的使用情景

> 简单的来说CAS适用**多读场景**，冲突一般较少
>
> synchronized适用**多写场景**，冲突一般较多。



## CAS原理

CAS即Compare And Swap，JDK中Unsafe类提供了一系列的compareAndSwap*方法。

即**compare and swap（比较与交换）**，是一种有名的**无锁算法**。无锁编程，即不使用锁的情况下实现多线程之间的变量同步，也就是在没有线程被阻塞的情况下实现变量的同步，所以也叫非阻塞同步（Non-blocking Synchronization）。**CAS算法**涉及到三个操作数

- 需要读写的内存值 V
- 进行比较的值 A
- 拟写入的新值 B

当且仅当 V 的值等于 A时，CAS通过原子方式用新值B来更新V的值，否则不会执行任何操作（比较和替换是一个原子操作）。一般情况下是一个**自旋操作**，即**不断的重试**。

java.util.concurrent包中的原子类就是通过CAS来实现了乐观锁。

CAS底层是通过CPU指令实现的

后续JDK通过CPU的cmpxchg指令，去比较寄存器中的 A 和 内存中的值 V。如果相等，就把要写入的新值 B 存入内存中。如果不相等，就将内存值 V 赋值给寄存器中的值 A。然后通过Java代码中的while循环再次调用cmpxchg指令进行重试，直到设置成功为止。

### 示例

下面以compareAndSwapLong(Object obj, long valueOffset, long expect, long update)方法为例进行介绍

boolean compareAndSwapLong(Object obj, long valueOffset, long expect, long update)方法：如果obj对象中内存偏移值为valueOffset的变量值为expect，则使用新的值update替换之。这是处理器提供的一个原子性指令。



1. **ABA问题**

   - CAS需要在操作值的时候检查内存值是否发生变化，没有发生变化才会更新内存值。但是如果内存值原来是A，后来变成了B，然后又变成了A，那么CAS进行检查时会发现值没有发生变化，但是实际上是有变化的。ABA问题的解决思路就是在变量前面**添加版本号**，每次变量更新的时候都把版本号加一，这样变化过程就从“A－B－A”变成了“1A－2B－3A”。

   - JDK从1.5开始提供了AtomicStampedReference类来解决ABA问题，具体操作封装在compareAndSet()中。compareAndSet()首先检查当前引用和当前标志与预期引用和预期标志是否相等（就是时间戳），如果都相等，则以原子方式将引用值和标志的值设置为给定的更新值。

2. **循环时间长开销大**。CAS操作如果长时间不成功，会导致其一直自旋，给CPU带来非常大的开销。

3. **只能保证一个共享变量的原子操作**
- 对一个共享变量执行操作时，CAS能够保证原子操作，但是对多个共享变量操作时，CAS是无法保证操作的原子性的。
- Java从1.5开始JDK提供了AtomicReference类来保证引用对象之间的原子性，可以把多个变量放在一个对象里来进行CAS操作。



## 自旋锁

线程获取同步资源的锁失败时，不放弃CPU的执行时间，通过自旋看看持有锁的线程是否很快就会释放锁

通过自旋操作减少CPU切换以及恢复现场导致的消耗



适应性自旋锁

自适应意味着自旋的时间（次数）不再固定，而是由前一次在同一个锁上的自旋时间及锁的拥有者的状态来决定。如果在同一个锁对象上，自旋等待刚刚成功获得过锁，并且持有锁的线程正在运行中，那么虚拟机就会认为这次自旋也是很有可能再次成功，进而它将允许自旋等待持续相对更长的时间。如果对于某个锁，自旋很少成功获得过，那在以后尝试获取这个锁时将可能省略掉自旋过程，直接阻塞线程，避免浪费处理器资源。



## 公平锁和非公平锁

公平锁是指多个线程按照申请锁的顺序来获取锁，线程直接进入队列中排队，队列中的第一个线程才能获得锁。

公平锁的优点是等待锁的线程不会饿死。缺点是整体吞吐效率相对非公平锁要低，等待队列中除第一个线程以外的所有线程都会阻塞，CPU唤醒阻塞线程的开销比非公平锁大。



非公平锁是多个线程加锁时直接尝试获取锁，获取不到才会到等待队列的队尾等待。但如果此时锁刚好可用，那么这个线程可以无需阻塞直接获取到锁，所以非公平锁有可能出现后申请锁的线程先获取锁的场景。

非公平锁的优点是可以**减少唤起线程的开销，整体的吞吐效率高**，因为线程有几率不阻塞直接获得锁，CPU不必唤醒所有线程。缺点是处于等待队列中的线程可能会饿死，或者等很久才会获得锁。



通过ReentrantLock的源码来讲解公平锁和非公平锁

ReentrantLock里面有一个内部类Sync，Sync继承AQS（AbstractQueuedSynchronizer），

添加锁和释放锁的大部分操作实际上都是在Sync中实现的。它有公平锁FairSync和非公平锁NonfairSync两个子类。

ReentrantLock默认使用非公平锁，也可以通过构造器来显示的指定使用公平锁。

![image-20220410163941146](java并发面经/image-20220410163941146.png)

公平锁与非公平锁的lock()方法唯一的区别就在于公平锁在获取同步状态时多了一个限制条件：hasQueuedPredecessors()。

该方法主要是判断当前线程是否位于同步队列中的第一个。如果是则返回true，否则返回false。



### 可重入锁 VS 非可重入锁

可重入锁又名递归锁，是指同一个线程在外层方法获取锁的时候，再进入该线程的内层方法会自动获取锁（前提锁对象得是同一个对象或者class），不会因为之前已经获取过还没释放而阻塞。Java中ReentrantLock和synchronized都是可重入锁，可重入锁的一个优点是可一定程度避免死锁。

NonReetrantLock是非可重入锁，ReentrantLock和NonReentrantLock都继承父类AQS，其父类AQS中维护了一个同步状态status来计数重入次数，status初始值为0。



### 独享锁 VS 共享锁

独享锁也叫排他锁，是指该锁一次只能被一个线程所持有。如果线程T对数据A加上排它锁后，则其他线程不能再对A加任何类型的锁。获得排它锁的线程即能读数据又能修改数据。JDK中的synchronized和JUC中Lock的实现类就是互斥锁。

共享锁是指该锁可被多个线程所持有。如果线程T对数据A加上共享锁后，则其他线程只能对A再加共享锁，不能加排它锁。获得共享锁的线程只能读数据，不能修改数据。

独享锁与共享锁也是通过AQS来实现的，通过实现不同的方法，来实现独享或者共享。

ReentrantReadWriteLock有两把锁：ReadLock和WriteLock，ReadLock和WriteLock是靠内部类Sync实现的锁，Sync是AQS的一个子类，这种结构在CountDownLatch、ReentrantLock、Semaphore里面也都存在。

AQS state 字段 在独享锁中这个值通常是0或者1（如果是重入锁的话state值就是重入的次数），在ReentrantReadWriteLock整型变量state上分别描述读锁和写锁的数量，高16位表示读锁状态（读锁个数），低16位表示写锁状态（写锁个数）。

![img](java并发面经/8793e00a.png)

读写锁才能实现读读的过程共享，而读写、写读、写写的过程互斥。



ReentrantLock无论读操作还是写操作，添加的锁都是都是独享锁。



## ReentrantLock的原理

![image-20220406191947840](java并发面经/image-20220406191947840.png)



ReetrantLock有个Sync类的实例，Sync类是继承AQS的, 它的子类NoFairSync和FairSync分别实现了获取锁的非公平策略和公平策略。

AQS是使用AQS实现的可重入的独占锁，默认是非公平锁。

AQS的state状态值表示线程获取该锁的可重入次数， 使用CAS设置state的值 compareAndSetState(0, 1)

tryacquire tryRelease方法



ReetrantLock**获取锁失败**后（说明已经有线程获取锁了 可能是其他线程，也可能是本线程），会调用tryacquire（）再次判断state的值

如果此时state的值为0（获取锁的线程恰好释放了锁） 更新state值为1并设置获取锁的线程为本线程

如果此时state的值不为0  判断是否是本线程之前获得到了锁 如果是 更新state的值 表示重入次数（具体看代码）

否则添加到AQS的**阻塞队列**里面



加锁：

- 通过ReentrantLock的加锁方法Lock进行加锁操作。
- 会调用到内部类Sync的Lock方法，由于Sync#lock是抽象方法，根据ReentrantLock初始化选择的公平锁和非公平锁，执行相关内部类的Lock方法，本质上都会执行AQS的Acquire方法。 
- AQS的Acquire方法会执行tryAcquire方法，但是由于tryAcquire需要自定义同步器实现，因此执行了ReentrantLock中的tryAcquire方法，由于ReentrantLock是通过公平锁和非公平锁内部类实现的tryAcquire方法，因此会根据锁类型不同，执行不同的tryAcquire。
- tryAcquire是获取锁逻辑，获取失败后，会执行框架AQS的后续逻辑，跟ReentrantLock自定义同步器无关。



解锁：

- 通过ReentrantLock的解锁方法Unlock进行解锁。
- Unlock会调用内部类Sync的Release方法，该方法继承于AQS。
- Release中会调用tryRelease方法，tryRelease需要自定义同步器实现，tryRelease只在ReentrantLock中的Sync实现，因此可以看出，释放锁的过程，并不区分是否为公平锁。
- 释放成功后，所有处理由AQS框架完成，与自定义同步器无关。



ReentrantLock加锁解锁时API层核心方法的映射关系

![img](java并发面经/f30c631c8ebbf820d3e8fcb6eee3c0ef18748.png)





## 加锁的公平性和非公平性

ReetrantLock调用lock方法时，会调用 sync 的lock

![image-20220406221233694](java并发面经/image-20220406221233694.png)

默认state为0，所以第一个调用Lock的吸纳成会通过CAS设置状态值为1，CAS成功则表示当前线程获取到了锁，然后设置该锁持有者为当前线程。

如果此时有其他线程企图过去该锁，CAS会失败，然后会调用AQS的acquire方法。

记住这个代码！

![image-20220406221329278](java并发面经/image-20220406221329278.png)



**何时加入队列**

**何时出队列**

addWaiter方法，这个方法其实就是把对应的线程以Node的数据结构形式加入到双端队列里，返回的是一个包含该线程的Node。而这个Node会作为参数，进入到acquireQueued方法中。acquireQueued方法可以对排队中的线程进行“获锁”操作。

总的来说，一个线程获取锁失败了，被放入等待队列，acquireQueued会把放入队列中的线程不断去获取锁（**自旋获取锁操作**），直到获取成功或者不再需要获取（中断）。

处于排队等候机制中的线程一直无法获取锁，节点的状态会变成取消状态，取消状态的节点会从队列中释放。

![image-20220407113655329](java并发面经/image-20220407113655329.png)



AQS并没有提供可用的tryAcquire方法，tryAcquire方法需要子类自己定制。这里会调用ReentrantLock重写的tryAcquire方法。

会调用Sync的子类实现NonfairSync或FairSync的 tryAcquire(), 这个方法实现了非公平性和公平性。

![image-20220406221425485](java并发面经/image-20220406221425485.png)



**非公平**指的是先尝试获取锁的线程并不一定首先获取该锁。当第一个线程A被放入AQS阻塞队列等待时，当线程B执行到代码1的时候发现state为0,就成功获得了锁，而比B先请求锁的线程A还在等待



**公平性**在tryAcquire方法中在判断当前没有线程获取锁的，state = 0 的时候，增加了一个**hasQueuedPredecessors**方法来判断是否有线程在当前线程前尝试获取锁，即判断当前线程是否位于同步队列中的第一个

如果当前线程节点有前驱节点则返回true，否则如果当前AQS队列为空或者当前线程节点是AQS的第一个节点则返回false。



![image-20220406221733246](java并发面经/image-20220406221733246.png)

![image-20220406222100669](java并发面经/image-20220406222100669.png)





## 读写锁 ReentrantReadWriteLock



## 谈谈 synchronized 和 ReentrantLock 的区别
两者都是可重入锁
synchronized 依赖于 JVM 而 ReentrantLock 依赖于 API
ReentrantLock 比 synchronized 增加了一些高级功能

- 等待可中断 : ReentrantLock提供了一种能够中断等待锁的线程的机制，通过 lock.lockInterruptibly() 来实现这个机制。也就是说正在等待的线程可以选择放弃等待，改为处理其他事情。相当于Synchronized来说可以避免出现死锁的情况
- 可实现公平锁 : ReentrantLock可以指定是公平锁还是非公平锁。而synchronized只能是非公平锁。所谓的公平锁就是先等待的线程先获得锁。ReentrantLock默认情况是非公平的，可以通过 ReentrantLock类的ReentrantLock(boolean fair)构造方法来制定是否是公平的。
- 可实现选择性通知（锁可以绑定多个条件）: synchronized关键字与wait()和notify()/notifyAll()方法相结合可以实现等待/通知机制。ReentrantLock类当然也可以实现，但是需要借助于Condition接口与newCondition()方法。
- tryLock：ReentrantLock提供了一个TryLock方法，如果锁可用, 则获取锁, 并立即返回true, 否则返回false, 该方法不会引起线程阻塞，tryLock使用的是非公平策略，和非公平锁的tryAcquire代码一样。

![image-20220321104734730](java并发面经/image-20220321104734730.png)

记住这个：

ReentrantLock是依赖AQS的，提供可中断等待、超时、尝试获取锁，实现公平锁和非公平锁（默认），可选择性通知、关联多个条件队列。



## 有哪几种方式实现线程安全
volatile 

互斥 同步 
synchronized 
ReetrantLock 

无同步 
ThreadLocal 本地副本 



# 说说volatile关键字

用以声明变量的值可能随时会别的线程修改，使用volatile修饰的变量会强制将修改的值立即写入主存，主存中值的更新会使缓存中的值失效(非volatile变量不具备这样的特性，非volatile变量的值会被缓存，线程A更新了这个值，线程B读取这个变量的值时可能读到的并不是是线程A更新后的值)。
volatile具有**内存可见性、有序性，不具备原子性**。

可见性：当多个线程访问同一个变量x时，线程1修改了变量x的值，线程1、线程2...线程n能够立即读取到线程1修改后的值。

即程序执行时按照代码书写的先后顺序执行。

有序性：即程序执行时按照代码书写的先后顺序执行。

**所以volatile 关键字 除了防止 JVM 的指令重排 ，还有一个重要的作用就是保证变量的可见性。**

**volatile适用场景**

适用于**读多写少**的场景。

JDK中volatie应用：JDK中**ConcurrentHashMap**的Entry的value和next被声明为volatile，**AtomicLong**中的value被声明为volatile。AtomicLong通过CAS原理(也可以理解为乐观锁)保证了原子性。
volatile的一个重要作用就是和CAS结合，保证了原子性，详细的可以参见java.util.concurrent.atomic包下的类，比如AtomicInteger。



## 说说 synchronized 关键字和 volatile 关键字的区别
`synchronized` 关键字和 `volatile` 关键字是两个互补的存在，而不是对立的存在！

- **`volatile` 关键字**是线程同步的**轻量级实现**，所以 **`volatile `性能肯定比` synchronized `关键字要好** 。volatile不会让线程阻塞，响应速度比synchronized高，但是 `volatile` 关键字只能用于变量而 `synchronized` 关键字可以修饰方法以及代码块** 。
- **`volatile` 关键字能保证数据的可见性，但不能保证数据的原子性。`synchronized` 关键字两者都能保证。**
- **`volatile`关键字主要用于解决变量在多个线程之间的可见性，而 `synchronized` 关键字解决的是多个线程之间访问资源的同步性。**



## 讲一下ThreadLocal类

ThreadLocal是JDK包提供的，实现每一个线程都有自己的专属本地变量。
如果你创建了一个ThreadLocal变量，那么访问这个变量的每个线程都会有这个变量的一个本地副本，当多个线程操作这个变量时，实际操作的是自己本地内存里面的变量，从而避免了线程安全的问题。



原理记住这个：

每个线程都有一个threadLocals和inheritableThreadLocals变量，它是hashmap类型的变量，存放每个线程变量的本地副本，初始为null,第一次调用ThreadLocals实例工具set方法的时候，创建当前线程的threadlocals变量。key为当前线程，value就是对应变量的值了。

## ThreadLocal原理
从 Thread类源代码入手。

```java
public class Thread implements Runnable {
    //......
    //与此线程有关的ThreadLocal值。由ThreadLocal类维护
    ThreadLocal.ThreadLocalMap threadLocals = null;

    //与此线程有关的InheritableThreadLocal值。由InheritableThreadLocal类维护
    ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;
    //......
}
```
**Thread 类中有一个 threadLocals 和 一个 inheritableThreadLocals 变量，它们都是 ThreadLocalMap 类型的变量**。默认情况下这两个变量都是 null，只有当前线程调用 ThreadLocal 类的 set或get方法时才创建它们，实际上调用这两个方法的时候，我们调用的是ThreadLocalMap类对应的 get()、set()方法。

每个线程的本地变量不是存放在ThreadLoal实例里面的，而是存放在**调用线程的threadLocals变量里面的**。ThreadLocals类型的本地变量是放到具体的线程内存空间的。

ThreadLocals就是一个工具壳，它通过 set把value值放到当前线程的threadLocal变量存放，或get方从当前线程的threadLocals变量拿出来使用。（来自书本）



## InheritableThreadLocal类

**inheritableThreadLocals继承自ThreadLocal 类，能够让子线程访问在父线程设置的本地变量**。就是这个类用变量inheritableThreadLocals替换了threadLocals变量。

把本地变量值放到当前线程的inheritableThreadLocals变量里面，线程通过inheritableThreadLocals类实例的set或get方法设置变量值。

**父线程创建子线程时，构造函数会把父线程的inheritableThreadLocals变量里面的本地变量复制一份保存在子线程的inheritableThreadLocals变量里面。**



## 为什么要用线程池

1. 在执行大量异步任务时，线程池能够提供较好的性能。

   不使用线程池的话，每次执行异步任务都需要创建线程，而线程的创建和销毁是需要开销的，线程池的线程是可以复用的

2. 线程池提供了一种限制和管理资源的手段，比如限制线程的个数，动态新增线程。

3. 线程池也提供了许多可调的参数和可扩展的接口。例如好几种创建线程池的方法。



## 线程池生命周期：

`ctl`这个AtomicInteger类型，是对线程池的运行状态(五种线程状态)和线程池中有效线程的数量进行控制的一个字段， 它同时包含两部分的信息：线程池的运行状态 (runState) 和线程池内有效线程的数量 (workerCount)，高3位保存runState，低29位保存workerCount，两个变量之间互不干扰。



## 线程池的五个状态如下：

- RUNNING：接受新任务并处理阻塞队列里的任务。
- SHUTDOWN：拒绝新任务但是处理阻塞队列里面的任务。
- STOP：拒绝新任务并且抛弃阻塞队列里的任务，同时会中断正在处理的任务。
- TIDYING：所有任务都执行完后当前线程池活动线程数为0，将要调用terminated方法（相当于一个过渡状态）。
- TERMINATED： 终止状态，terminated方法调用完成后的状态。

生命周期转换流程

![图3 线程池生命周期](java并发面经/582d1606d57ff99aa0e5f8fc59c7819329028.png)****



参考博客

https://tech.meituan.com/2020/04/02/java-pooling-pratice-in-meituan.html

## 任务调度

**线程池的工作流程要从它的执行方法 execute() 说起**

​		所有任务的调度都是由execute方法完成的，这部分完成的工作是：检查现在线程池的运行状态、运行线程数、运行策略，决定接下来执行的流程，是直接申请线程执行，或是缓冲到队列中执行，亦或是直接拒绝该任务。其执行过程如下：

1. 首先检测线程池运行状态，如果不是RUNNING，则直接拒绝，线程池要保证在RUNNING的状态下执行任务。
2. 如果workerCount < corePoolSize，则创建并启动一个线程来执行新提交的任务（ 执行这一步骤需要获取全局锁 )。
3. 如果workerCount >= corePoolSize，且线程池内的阻塞队列未满，则将任务添加到该阻塞队列中。
4. 如果workerCount >= corePoolSize && workerCount < maximumPoolSize，且线程池内的阻塞队列已满，则创建并启动一个线程来执行新提交的任务（ 执行这一步骤需要获取全局锁 )。
5. 如果workerCount >= maximumPoolSize，并且线程池内的阻塞队列已满, 则根据拒绝策略来处理该任务, 默认的处理方式是拒绝并抛出异常（四种拒绝）



## 线程池的类型如下

常见线程池 **四种**

①newSingleThreadExecutor

创建一个核心线程个数和最大线程个数都为1的线程池，阻塞队列长度为Integer.max_value, keepAliveTime = 0,表示线程个数比核心线程个数多并且当前空闲则回收, 阻塞队列为LinkedBlockingQueue。

②newFixedThreadExecutor(n)

固定大小的线程池

创建一个核心线程个数和最大线程个数都为n的线程池，阻塞队列长度为Integer.max_value, keepAliveTime = 0,表示线程个数比核心线程个数多并且当前空闲则回收, 阻塞队列为LinkedBlockingQueue。

③newCacheThreadExecutor()

创建一个按需创建线程的线程池,初始线程个数为0,即核心线程数为0，最大线程数为Integer.max_value, keepAliveTime = 60s,表示只有当前线程在60s内空闲则回收，阻塞队列为同步队列, 同步队列里面最多只有一个任务，加入同步队列的任务会被马上执行。

④newScheduleThreadExecutor（n）

延时任务执行的线程池

创建一个核心线程个数为n, 最大线程数为Integer.max_value, keepAliveTime = 0,  阻塞队列为DelayQueue



**newScheduleThreadExecutor主要方法介绍：**

https://www.cnblogs.com/jinggod/p/8490223.html

- 零延时的 execute()、submit() 方法

本质上调用的还是 schedule() 方法；从下面的源码可以看出，这两个方法提交的任务都是延时为0的 “实时任务”；

- 提交一个延时任务的 schedule() 方法

**`<V> ScheduledFuture<V> schedule(Callable<V> callable, long delay, TimeUnit unit)：`**
创建并执行在给定延迟后启用的 ScheduledFuture。
**`ScheduledFuture<?> schedule(Runnable command, long delay,imeUnit unit)：`**
创建并执行在给定延迟后启用的一次性操作。

- 提交周期性的任务 

`scheduleAtFixedRate()`（固定的周期时间）

 `scheduleWithFixedDelay()`(固定的间隔时间)



通过上面的几个线程池的底层实现，我们可以发现底层都是通过 `ThreadPoolExecutor` 类来实现的，只是参数不一样

可以自己直接调用ThreadPoolExecutor的构造函数来自己创建线程池。阿里java编程建议这样创建线程池。  



## ThreadPoolExecutor 线程池的参数和含义

[使用 ThreadPoolExecutor 创建线程池时所设置的 7 个参数](https://mdnice.com/writing/3c078a2e14bc4412afdd126ed61abc86)

**这 7 个参数分别是：**

1. corePoolSize：核心线程数

   通常情况下最多添加corePoolSize个Worker，当任务过多时添加到阻塞队列里，阻塞队列满的时候，会继续创建Worker直到Worker数达到maximumPoolSize，超过maximumPoolSize实施拒绝策略，有四种拒绝策略。

2. maximumPoolSize：线程池的最大线程数。-

3. keepAliveTime：空闲线程存活时间

   如果当前线程池的线程数量比核心线程数多，且是闲置状态，则这些闲置的线程能存活的最大时间。

4. TimeUnit：存活时间的时间单位。

5. workQueue：用于保存等待执行的任务的阻塞队列、任务队列。**七种**

   如基于数组的有界队列ArrayBlockingQueue

   基于链表的有界队列LinkedBlockingQueue

   基于链表的无界队列LinkedTransferQueue

   基于链表的双向阻塞队列LinkedBlockingQueue
   
   
   
   不存储元素的同步队列SynchronousQueue
   
   支持优先级排序的无界阻塞队列 PriorityBlockingQueue
   
   优先级队列实现，实现延迟获取的延迟无界阻塞队列DelayQueue



6. ThreadFactory：创建线程的工厂

   此参数一般用的比较少，我们通常在创建线程池时不指定此参数，它会使用默认的线程创建工厂的方法来创建线程。可以通过实现 ThreadFactory 接口来自定义一个线程工厂，这样就可以自定义线程的名称或线程执行的优先级了。

7. RejectedExecutionHandler：拒绝策略

- AbortPolicy：丢弃任务并抛出异常。
- DiscardPolicy：忽略并抛弃任务。
- DiscardOldestPolicy：抛弃队列最前面的任务，然后重新提交被拒绝的任务。
- CallerRunsPolicy：由调用线程（提交任务的线程）处理该任务



### workQueue 线程池 阻塞队列

**阻塞队列：线程池存放任务的队列，用来存储线程池的所有待执行任务。** 它可以设置以下几个值：

1. ArrayBlockingQueue：基于数组实现的有界阻塞队列。
2. LinkedBlockingQueue：基于链表实现的有界阻塞队列，队列默认长度为Integer.max_value

LinkedTransferQueue：一个由链表结构组成的无界阻塞队列。

LinkedBlocking**D**eque：一个由链表结构组成的双向阻塞队列。



SynchronousQueue：一个不存储元素的阻塞队列（同步队列），即直接提交给线程不保持它们，使用场景newCachedThreadPool()就使用了不储存元素的阻塞队列。

PriorityBlockingQueue：一个支持优先级排序的无界阻塞队列。

DelayQueue：一个使用优先级队列实现，实现延迟获取的无界阻塞队列，只有在延迟期满时才能从中提取元素。



![image-20220323095629397](java并发面经/image-20220323095629397.png)



比较常用的是 LinkedBlockingQueue，线程池的排队策略和 BlockingQueue 息息相关。



###  RejectedExecutionHandler：拒绝策略

**拒绝策略：当线程池的任务超出线程池队列可以存储的最大值之后，执行的策略。** 默认的拒绝策略有以下 4 种：

- AbortPolicy：丢弃任务并抛出异常。
- DiscardPolicy：忽略并抛弃任务。
- DiscardOldestPolicy：抛弃队列最前面的任务，然后重新提交被拒绝的任务。
- CallerRunsPolicy：由调用线程（提交任务的线程）处理该任务

线程池的默认策略是 AbortPolicy 丢弃并抛出异常。



## Worker线程管理

线程池内的工作线程Worker

Worker这个工作线程，实现了Runnable接口，并持有一个线程thread，一个初始化的任务firstTask。

![image-20220323110948598](java并发面经/image-20220323110948598.png)

![image-20220323111316485](java并发面经/image-20220323111316485.png)



**Worker线程增加**

Worker是通过继承AQS，使用AQS来实现独占锁这个功能。

增加线程是通过线程池中的addWorker方法，该方法的功能就是增加一个线程，addWorker方法有两个参数：firstTask、core。firstTask参数用于指定新增的线程执行的第一个任务，该参数可以为空；core参数为true表示在新增线程时会判断当前活动线程数是否少于corePoolSize，false表示新增线程前需要判断当前活动线程数是否少于maximumPoolSize。



**addWorker 代码 流程**

1、获取线程池的控制状态，进行判断，不符合则返回false，符合则下一步
2、死循环，判断workerCount是否大于上限，或者大于corePoolSize/maximumPoolSize，没有的话则对workerCount+1操作，
3、如果不符合上述判断或+1操作失败，再次获取线程池的控制状态，获取runState与刚开始获取的runState相比，不一致则跳出内层循环继续外层循环，否则继续内层循环
4、+1操作成功后，使用重入锁ReentrantLock来保证往workers当中添加worker实例，添加成功就启动该实例。



**Worker线程回收**

线程池使用一张Hash表去持有线程的引用，这样可以通过添加引用、移除引用这样的操作来控制线程的生命周期。

线程池中线程的销毁依赖JVM自动的回收，线程池做的工作是根据当前线程池的状态维护一定数量的线程引用，防止这部分线程被JVM回收，当线程池决定哪些线程需要回收时，只需要将其引用消除即可。



**Worker线程执行任务**

在Worker类中的run方法调用了runWorker方法来执行任务。



## 并发编程的线程同步器 共享锁

### CountDownLatch

![image-20220323155027000](java并发面经/image-20220323155027000.png)

日常开发中经常遇到一个线程需要等待一些线程都结束后才能继续向下运行的场景，在CountDownLatch出现之前通常使用join方法来实现。

CountDownLatch用法：

```
CountDownLatch countDownLatch = new CountDownLatch(2);
// 阻塞直到被interrupt或计数器递减至0
countDownLatch.await();
```

**CountDownLatch相对于join方法的优点大致有两点：**

- 调用一个子线程的join方法后，该线程会一直阻塞直到子线程运行完毕，而CountDownLatch允许子线程运行完毕或在运行过程中递减计数器，也就是说await方法不一定要等到子线程运行结束才返回。
- 使用线程池来管理线程一般都是直接添加Runnable到线程池，这时就没有办法再调用线程的join方法了，而仍可在子线程中递减计数器，也就是说CountDownLatch相比join方法可以更灵活地控制线程的同步。

**CountDownLatch是基于AQS实现的**，把计数器的值赋给了AQS的状态变量state



主要用法：

#### void await()

当线程调用CountDownLatch的await方法后，当前线程会被阻塞，直到CountDownLatch的计数器值递减至0或者其他线程调用了当前线程的interrupt方法。

#### void countDown()

递减计数器，当计数器的值为0（即state=0）时会唤醒所有因调用await方法而被阻塞的线程。



## CyclicBarrier 回环屏障

![image-20220323194826436](java并发面经/image-20220323194826436.png)



CyclicBarrier是回环屏障的意思，它可以使一组线程全部达到一个状态后再全部同时执行，然后重置自身状态又可用于下一次的状态同步。

线程调用await方法后就会被阻塞，这个阻塞点叫屏障点，等所有的线程都调用了await方法，线程们就会冲破屏障，继续向下运行。

只能保证线程都执行完一个状态，不能保证这期间线程的顺序问题。



![image-20220322161323068](java并发面经/image-20220322161323068.png)



## Semaphore

Semaphore信号量也是一个同步器，与CountDownLatch和CyclicBarrier不同的是，**它内部的计数器是递增的**，并且在初始化时可以指定计数器的初始值（通常为0），但不必知道需要同步的线程个数，而是在**需要同步的地方调用acquire方法时指定需要同步的线程个数**。

Semaphore还是使用AQS实现的，并且可以选取公平性策略（默认为非公平性的）。

![image-20220323155436807](java并发面经/image-20220323155436807.png)

用法：

#### void acquire()

表示当前线程希望获取一个信号量资源，如果当前信号量大于0，则当前信号量的计数减1，然后该方法直接返回。否则如果当前信号量等于0，则被阻塞。

#### void acquire(int permits)

可获取多个信号量。

#### void release()

使信号量加1，如果当前有线程因为调用acquire方法被阻塞而被放入AQS中的话，会根据公平性策略选择一个信号量个数能被满足的线程进行激活。

#### void release(int permits)

可增加多个信号量。



## AQS 抽象队列同步器

https://tech.meituan.com/2019/12/05/aqs-theory-and-apply.html

AQS是锁阻塞和同步器的基础框架，并发包中锁的底层实现就是使用AQS实现的。

Java中的大部分同步类（Lock、CountDownLatch、Semaphore、ReentrantLock、ReentrantReadWriteLock等）都是基于AbstractQueuedSynchronizer（简称为AQS）实现的

![image-20220406191812452](java并发面经/image-20220406191812452.png)



- AQS是一个FIFO的双向阻塞队列，队列的元素类型是一个Node。

- 对于AQS来说，线程同步的关键是对状态值state进行操作，state是用Volatile修饰的，操作state的方法分为独占方式和共享模式，通过CAS方式更新state的值，compareAndSetState(int expect, int update)

- 当线程通过调用acquire(int arg) 方法或 acquireShare(int arg)获取独占资源或共享资源时，成功则返回，失败则将当前线程封装成一个类型为Node.EXCLUSIVE 或Node.SHARED的Node节点插入AQS阻塞队列的尾部。 

- 自定义同步器要么是独占方式，要么是共享方式，它们也只需实现tryAcquire-tryRelease、tryAcquireShared-tryReleaseShared中的一种即可。AQS也支持自定义同步器同时实现独占和共享两种方式，如ReentrantReadWriteLock。ReentrantLock是独占锁，所以实现了tryAcquire-tryRelease。



**自旋**

当多个线程同时调用lock.lock方法获取锁时，只有一个线程获取到锁，其他线程会被转化为Node节点插入到lock锁对应的阻塞队列里面，并做自旋CAS尝试获取锁。



## Node变量 

五个变量 不包括share exclusive  

thread 存放加入AQS队列里面的线程

**waiterStatus 当前节点在队列中的状态 有五个状态**

- 0 当一个Node被初始化的时候的默认值

- CANCELLED 表示线程获取锁的请求已经取消
- SIGNAL 线程等待被唤醒
- CONDITION 线程在条件队列里面等待
- PROPAGATE 释放共享资源时需要通知其他节点

prev 前驱指针

next 后继指针

nextWaiter 指向下一个处于CONDITION状态的节点

SHARED 表示线程以共享的模式等待锁

EXCLUSIVE 表示线程正在以独占的方式等待锁



## newCondition 的await() 和signal()

条件队列是ConditionObject 条件变量里面维护的，和AQS队列不是一回事。

如果获取到锁的线程调用了条件变量的await（）方法，创建一个类型为Node.CONDITION的node节点，然后节点插入到条件变量的阻塞队列( 条件队列 )末尾，然后释放获取到的锁，在await()方法处阻塞。

当另外一个线程调用条件变量的signal（）方法时，在内部会把条件队列头部的一个线程节点从条件队列里面移除并放入AQS的阻塞队列里面，然后激活这个线程。





某个线程获取锁失败的后续流程是什么呢？

A：存在某种排队等候机制,通过addWaiter添加到等待队列中，线程继续等待，仍然保留获取锁的可能，获取锁流程仍在继续。

Q：既然说到了排队等候机制，那么就一定会有某种队列形成，这样的队列是什么数据结构呢？

A：是CLH变体的FIFO双端队列。Craig、Landin and Hagersten队列

Q：处于排队等候机制中的线程，什么时候可以有机会获取锁呢？

A：可以详细看下2.3.1.3小节。

Q：如果处于排队等候机制中的线程一直无法获取锁，需要一直等待么？还是有别的策略来解决这一问题？

A：**线程所在节点的状态会变成取消状态**，取消状态的节点会从队列中释放，具体可见2.3.2小节。

Q：Lock函数通过Acquire方法进行加锁，但是具体是如何加锁的呢？

A：AQS的Acquire会调用tryAcquire方法，tryAcquire由各个自定义同步器实现，通过tryAcquire完成加锁过程。

