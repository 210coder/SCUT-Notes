## 集合大体框架图
![在这里插入图片描述](https://img-blog.csdnimg.cn/6aadb04079ba4f5c9de69f034fddd877.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5Y2X5bCP5ZOl,size_20,color_FFFFFF,t_70,g_se,x_16)
## 集合的数据结构底层实现


#### List

- `Arraylist`： `Object[]` 数组
- `Vector`：`Object[]` 数组
- `LinkedList`： 双向链表(JDK1.6 之前为循环链表，JDK1.7 取消了循环)

#### Set

- `HashSet`(无序，唯一): 基于 `HashMap` 实现的，底层采用 `HashMap` 来保存元素
- `LinkedHashSet`: `LinkedHashSet` 是 `HashSet` 的子类，并且其内部是通过 `LinkedHashMap` 来实现的。有点类似于我们之前说的 `LinkedHashMap` 其内部是基于 `HashMap` 实现一样，不过还是有一点点区别的
- `TreeSet`(有序，唯一): 红黑树(自平衡的排序二叉树)

#### Queue

- `PriorityQueue`: `Object[]` 数组来实现二叉堆
- `ArrayQueue`: `Object[]` 数组 + 双指针

再来看看 `Map` 接口下面的集合。

#### Map
一般就是数组+链表  HashMap的话链表长度大于8  就会转化为红黑树
有Tree的比如TreeMap就是红黑树
- `HashMap`： **JDK1.8 之前 `HashMap` 由数组+链表组成的，数组是 `HashMap` 的主体，链表则是主要为了解决哈希冲突而存在的（“拉链法”解决冲突）。JDK1.8 以后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间**
- `LinkedHashMap`： `LinkedHashMap` 继承自 `HashMap`，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外，`LinkedHashMap` 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。
![在这里插入图片描述](https://img-blog.csdnimg.cn/eee42043e7b14fa8b4d2fdc7ce92a3b0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5Y2X5bCP5ZOl,size_19,color_FFFFFF,t_70,g_se,x_16)
LinkHashMap如何实现有序的：
在HashMap的基础上增加了一个双向链表，LinkedHashMap对Entry进行了扩展，增加了指针before和after。
![在这里插入图片描述](https://img-blog.csdnimg.cn/4224e499d4164beea119756aa0ae8198.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5Y2X5bCP5ZOl,size_20,color_FFFFFF,t_70,g_se,x_16)
- `Hashtable`： 数组+链表组成的，数组是 `Hashtable` 的主体，链表则是主要为了解决哈希冲突而存在的
- `TreeMap`： 红黑树（自平衡的排序二叉树）

## Arraylist 与 LinkedList 区别
- **1. 是否保证线程安全：**`ArrayList`和`LinkedList`都是不同步的，也就是不保证线程安全；

- **2. 底层数据结构：**`Arraylist`底层使用的是**`Object`数组**；`LinkedList`底层使用的是**双向链表**数据结构（JDK1.6之前为循环链表，JDK1.7取消了循环。）

- **3.插入和删除是否受元素位置的影响**：①`ArrayList`采用数组存储，所以插入和删除元素的时间复杂度受元素位置的影响。比如：执行`add(E e)`方法的时候，`ArrayList`会默认在将指定的元素追加到此列表的末尾，这种情况时间复杂度就是O(1)。但是如果要在指定位置 i 插入和删除元素的话（`add(int index, E element)`）时间复杂度就为 O(n-i)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作。 ②`LinkedList`采用链表存储，所以插入，删除元素时间复杂度不受元素位置的影响，都是近似 O（1）而数组为近似 O（n）。

- **4. 是否支持快速随机访问：**`LinkedList`不支持高效的随机元素访问，而`ArrayList`支持。快速随机访问就是通过元素的序号快速获取元素对象(对应于`get(int index)`方法)。

- **5. 内存空间占用**:ArrayList的空间浪费主要体现在在list列表的结尾会预留一定的容量空间，而LinkedList的空间花费则体现在它的每一个元素都需要消耗比ArrayList更多的空间（因为要存放直接后继和直接前驱以及数据）。

## ArrayList的扩容机制
ArrayList默认数组容量为10
ArrayList初建数组是空数组
当第一次add元素时，数组的容量设置为默认数组长度

- 之后再add元素时，计算数组的实际需要的最小容量（数组的实际长度+1）
判断数组需要实际的最小容量和数组的容量（数组的长度）若数组实际需要的最小容量比数组的容量大
调用grow(int mincapacity) 进行扩容
- 将数组的容量 * 1.5 和实际需要的最小容量比较 取最大值作为扩容的容量
- 若扩容的容量 和 数组默认的数组最大长度（Integer.MAX_VALUE - 8） 相比 扩容容量大
		- 如果实际需要的最小容量 比  Integer.MAX_VALUE还要大 扩容的容量设为Integer.MAX_VALUE
		- 如果实际需要的最小容量 比  Integer.MAX_VALUE还要小 扩容的容量设为实际需要的最小容量

## HashTable和HashMap区别

- **1.继承的父类不同**

Hashtable继承自Dictionary类，而HashMap继承自AbstractMap类。但二者都实现了Map接口。

- **2.线程安全性不同**

HashMap 是非线程安全的，HashTable 是线程安全的。HashTable 内部的方法基本都经过`synchronized`修饰。（如果你要保证线程安全的话就使用 ConcurrentHashMap ）。

Hashtable不建议在新代码中使用，不需要线程安全的场合可以用HashMap替换，需要线程安全的场合可以用ConcurrentHashMap替换。

**ConcurrentHashMap取消了Segment分段锁，采用CAS和synchronized来保证并发安全。**

- **3.对Null key 和Null value的支持不同**

HashMap 中，null 可以作为键，这样的键只有一个，可以有一个或多个键所对应的值为 null。但是在 HashTable 中 put 进的键值只要有一个 null，直接抛出 NullPointerException。

- **4.初始容量大小和每次扩充容量大小的不同**

①创建时如果不指定容量初始值，Hashtable 默认的初始大小为11，之后每次扩充，容量变为原来的2n+1。HashMap 默认的初始化大小为16。之后每次扩充，容量变为原来的2倍。②创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小，而 HashMap 会将其扩充为2的幂次方大小。

- **5.底层数据结构**

都是基于哈希表实现的，就是数组+链表。哈希表，也称为哈希数组，数组的每个元素都是一个单链表的头节点，链表是用来解决冲突的，如果不同的key映射到了数组的同一位置处，就将其放入单链表中。

JDK1.8 以后的 HashMap 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间。Hashtable 没有这样的机制。


## 红黑树
1）  红黑树节点要么是红节点、要么是黑节点；

2）  根节点为黑节点；

3）  叶节点（空节点）为黑节点；

4）  每个红节点的两个子节点是黑节点；

5）  对于每个节点，从该节点到其叶子点的所有路径上包含相同数目的黑节点。

6）最长路径不超过最短路径的两倍

左旋：根节点变为左节点 右节点变为根节点


## HashMap 和 HashSet区别
**往set 和map两种数据结构去比较**

如果你看过`HashSet`源码的话就应该知道：HashSet 底层就是基于 HashMap 实现的。

| HashMap                   | HashSet                                                                |
|:-------------------------:|:----------------------------------------------------------------------:|
| 实现了Map接口                  | 实现Set接口                                                                |
| 存储键值对                     | 仅存储对象                                                                  |
| 调用`put（）`向map中添加元素        | 调用`add（）`方法向Set中添加元素                                                   |
| HashMap使用键（Key）计算Hashcode | HashSet使用成员对象来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性， |

**TreeMap、TreeSet以及JDK1.8之后的HashMap底层都用到了红黑树。红黑树就是为了解决二叉查找树的缺陷，因为二叉查找树在某些情况下会退化成一个线性结构。**

## Map的一些介绍

Java为数据结构中的映射定义了一个接口java.util.Map，此接口主要有四个常用的实现类，分别是HashMap、Hashtable、LinkedHashMap和TreeMap，类继承关系如下图所示：

下面针对各个实现类的特点做一些说明：

- **(1) HashMap**：它根据键的hashCode值存储数据，大多数情况下可以直接定位到它的值，因而具有很快的访问速度，但遍历顺序却是不确定的。 HashMap继承AbstractMap，最多只允许一条记录的键为null，允许多条记录的值为null。HashMap非线程安全，即任一时刻可以有多个线程同时写HashMap，可能会导致数据的不一致。如果需要满足线程安全，可以用 Collections的synchronized Map方法使HashMap具有线程安全的能力，或者使用ConcurrentHashMap。

- **(2) Hashtable**：Hashtable是遗留类，很多映射的常用功能与HashMap类似，不同的是它承自Dictionary类，不允许键值中一个为null，并且是线程安全的，任一时间只有一个线程能写Hashtable，并发性不如ConcurrentHashMap，因为ConcurrentHashMap引入了分段锁。Hashtable不建议在新代码中使用，不需要线程安全的场合可以用HashMap替换，需要线程安全的场合可以用ConcurrentHashMap替换。

- **(3) LinkedHashMap**：LinkedHashMap是HashMap的一个子类，保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的，也可以在构造时带参数，按照访问次序排序。

- **(4) TreeMap**：TreeMap实现SortedMap接口，能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator遍历TreeMap时，得到的记录是排过序的。如果使用排序的映射，建议使用TreeMap。在使用TreeMap时，key必须实现Comparable接口或者在构造TreeMap传入自定义的Comparator，否则会在运行时抛出java.lang.ClassCastException类型的异常。


## hashCode()与 equals() 的相关规定：
- 如果两个对象相等，则 hashcode 一定也是相同的
- 两个对象相等,对两个 equals() 方法返回 true
- 两个对象有相同的 hashcode 值，它们也不一定是相等的
- 综上，equals() 方法被覆盖过，则 hashCode() 方法也必须被覆盖
hashCode()的默认行为是对堆上的对象产生独特值。如果没有重写 hashCode()，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）。

## ==与 equals 的区别
- 对于基本类型来说，== 比较的是值是否相等；
- 对于引用类型来说，== 比较的是两个引用是否指向同一个对象地址（两者在内存中存放的地址（堆内存地址）是否指向同一个地方）；
- 对于引用类型（包括包装类型）来说，equals 如果没有被重写，对比它们的地址是否相等；如果 equals()方法被重写（例如 String），则比较的是地址里的内容。

## ArrayDeque 与 LinkedList 的区别
ArrayDeque 和 LinkedList 都实现了 Deque 接口，两者都具有队列的功能，但两者有什么区别呢？

- ArrayDeque 是基于可变长的数组和双指针来实现，而 LinkedList 则通过链表来实现。

- ArrayDeque 不支持存储 NULL 数据，但 LinkedList 支持。

- ArrayDeque 是在 JDK1.6 才被引入的，而LinkedList 早在 JDK1.2 时就已经存在。

- ArrayDeque 插入时可能存在扩容过程, 不过均摊后的插入操作依然为 O(1)。虽然 LinkedList 不需要扩容，但是每次插入数据时均需要申请新的堆空间，均摊性能相比更慢。

从性能的角度上，选用 ArrayDeque 来实现队列要比 LinkedList 更好。此外，ArrayDeque 也可以用于实现栈。

## 讲讲PriorityQueue
总是优先级最高的元素先出队。
- PriorityQueue 利用了二叉堆的数据结构来实现的，底层使用可变长的数组来存储数据
- PriorityQueue 通过堆元素的上浮和下沉，实现了在 O(logn) 的时间复杂度内插入元素和删除堆顶元素。
- PriorityQueue 是非线程安全的，且不支持存储 NULL 和 non-comparable 的对象。
- PriorityQueue 默认是小顶堆，但可以接收一个 Comparator 作为构造参数，从而来自定义元素优先级的先后。

```java
// 传入比较构造器 排序对象 最大堆 最小堆
PriorityQueue<Person> p = new PriorityQueue<Person>(new Comparator<Person>(
	@OVerride
	public int compare(Person o1, Person o2){
		return 1;
	}
));
```
