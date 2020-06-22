# volatile

https://baijiahao.baidu.com/s?id=1595669808533077617&wfr=spider&for=pc

用以声明变量的值可能随时会别的线程修改，使用volatile修饰的变量会强制将修改的值立即写入主存，主存中值的更新会使缓存中的值失效(非volatile变量不具备这样的特性，非volatile变量的值会被缓存，线程A更新了这个值，线程B读取这个变量的值时可能读到的并不是是线程A更新后的值)。

volatile具有**可见性、有序性，不具备原子性**。

可见性：当多个线程访问同一个变量x时，线程1修改了变量x的值，线程1、线程2...线程n能够立即读取到线程1修改后的值。

即程序执行时按照代码书写的先后顺序执行。

有序性：即程序执行时按照代码书写的先后顺序执行。

**volatile适用场景**

适用于**读多写少**的场景。

JDK中volatie应用：JDK中**ConcurrentHashMap**的Entry的value和next被声明为volatile，**AtomicLong**中的value被声明为volatile。AtomicLong通过CAS原理(也可以理解为乐观锁)保证了原子性。

**volatile VS synchronized**

volatile不会让线程阻塞，响应速度比synchronized高，这是它的优点
