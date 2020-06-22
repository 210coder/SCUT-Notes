## Arraylist 与 LinkedList 区别

- **1. 是否保证线程安全：**`ArrayList`和`LinkedList`都是不同步的，也就是不保证线程安全；

- **2. 底层数据结构：**`Arraylist`底层使用的是**`Object`数组**；`LinkedList`底层使用的是**双向链表**数据结构（JDK1.6之前为循环链表，JDK1.7取消了循环。）

- **3.插入和删除是否受元素位置的影响**：①`ArrayList`采用数组存储，所以插入和删除元素的时间复杂度受元素位置的影响。比如：执行`add(E e)`方法的时候，`ArrayList`会默认在将指定的元素追加到此列表的末尾，这种情况时间复杂度就是O(1)。但是如果要在指定位置 i 插入和删除元素的话（`add(int index, E element)`）时间复杂度就为 O(n-i)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作。 ②`LinkedList`采用链表存储，所以插入，删除元素时间复杂度不受元素位置的影响，都是近似 O（1）而数组为近似 O（n）。

- **4. 是否支持快速随机访问：**`LinkedList`不支持高效的随机元素访问，而`ArrayList`支持。快速随机访问就是通过元素的序号快速获取元素对象(对应于`get(int index)`方法)。

- **5. 内存空间占用**:ArrayList的空间浪费主要体现在在list列表的结尾会预留一定的容量空间，而LinkedList的空间花费则体现在它的每一个元素都需要消耗比ArrayList更多的空间（因为要存放直接后继和直接前驱以及数据）。

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

## HashMap 和 HashSet区别

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

![](C:\Users\祥仔\AppData\Roaming\marktext\images\2019-08-31-01-55-42-image.png)

下面针对各个实现类的特点做一些说明：

- **(1) HashMap**：它根据键的hashCode值存储数据，大多数情况下可以直接定位到它的值，因而具有很快的访问速度，但遍历顺序却是不确定的。 HashMap最多只允许一条记录的键为null，允许多条记录的值为null。HashMap非线程安全，即任一时刻可以有多个线程同时写HashMap，可能会导致数据的不一致。如果需要满足线程安全，可以用 Collections的synchronizedMap方法使HashMap具有线程安全的能力，或者使用ConcurrentHashMap。

- **(2) Hashtable**：Hashtable是遗留类，很多映射的常用功能与HashMap类似，不同的是它承自Dictionary类，并且是线程安全的，任一时间只有一个线程能写Hashtable，并发性不如ConcurrentHashMap，因为ConcurrentHashMap引入了分段锁。Hashtable不建议在新代码中使用，不需要线程安全的场合可以用HashMap替换，需要线程安全的场合可以用ConcurrentHashMap替换。

- **(3) LinkedHashMap**：LinkedHashMap是HashMap的一个子类，保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的，也可以在构造时带参数，按照访问次序排序。

- **(4) TreeMap**：TreeMap实现SortedMap接口，能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator遍历TreeMap时，得到的记录是排过序的。如果使用排序的映射，建议使用TreeMap。在使用TreeMap时，key必须实现Comparable接口或者在构造TreeMap传入自定义的Comparator，否则会在运行时抛出java.lang.ClassCastException类型的异常。

## 集合框架底层数据结构总结

## Collection

1. **List**
- **Arraylist**：Object数组

- **Vector**：Object数组

- **LinkedList**：双向链表(JDK1.6之前为循环链表，JDK1.7取消了循环)
  
   2.**Set**

- **HashSet**（无序，唯一）:基于 HashMap 实现的，底层采用 HashMap 来保存元素

- **LinkedHashSet**：LinkedHashSet 继承于 HashSet，并且其内部是通过 LinkedHashMap 来实现的。有点类似于我们之前说的LinkedHashMap 其内部是基于 HashMap 实现一样，不过还是有一点点区别的

- **TreeSet**（有序，唯一）：**红黑树(自平衡的排序二叉树)
  
   3.**Map**

- **HashMap**：JDK1.8之前HashMap由数组+链表组成的，数组是HashMap的主体，链表则是主要为了解决哈希冲突而存在的（“拉链法”解决冲突）。JDK1.8以后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间

- **LinkedHashMap**：LinkedHashMap 继承自 HashMap，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外，LinkedHashMap 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。详细可以查看：[《LinkedHashMap 源码详细分析（JDK1.8）》](https://www.imooc.com/article/22931)

- **Hashtable**：数组+链表组成的，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的

- **TreeMap**：红黑树（自平衡的排序二叉树）
