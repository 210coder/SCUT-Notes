tensorflow的基本操作
张量
张量是tenforflow的主要数据结构，用来操作计算图的
张量简单来说就是多维数组
a、标量：维度为0的Tensor，也就是一个实数
b、向量：维度为1的Tensor
c、矩阵：维度为2的Tensor
d、张量：维度达到及超过3的Tensor
创建张量有以下主要4种方法：
a、创建固定张量
创建常数张量：
constant_ts=tf.constant([1,2,3,4,5])

创建零张量：
zero_ts=tf.zeros([row_dim, col_dim])

创建单位张量：
ones_ts=tf.ones([row_dim,col_dim])

创建张量，并用常数填充：
filled_ts=tf.fill([row_dim,col_dim],123)

b、创建相似形状张量
创建相似的零张量：
zeros_like=tf.zeros_like(constant_ts)

创建相似的单位张量：
ones_like=tf.ones_like(constant_ts)

c、创建序列张量
指定起止范围
linear_ts=tf.linspace(start=0, stop=2, num=6)
结果返回[0.0,0.4,0.8,1.2,1.6,2.0]，注意该结果包括stop值


指定增量
seq_ts=tf.range(start=4, limit=16, delta=4)
结果返回[4,8,12]，注意该结果不包括stop值

d、随机张量
生成均匀分布的随机数
randunif_ts=tf.random_uniform([row_dim,col_dim],minval=0,maxval=1)
结果返回从minval（包含）到maxval（不包含）的均匀分布的随机数

生成正态分布的随机数
randnorm_ts=tf.random_normal([row_dim,col_dim],mean=0.0,stddev=1.0)
其中mean表示平均值，stddev表示标准差

（2）占位符和变量
占位符和变量是使用TensorFlow计算图的关键工具，两者是有区别的
a、变量：是TensorFlow算法中的参数，通过调整这些变量的状态来优化模型算法；
b、占位符：是TensorFlow对象，用于表示输入输出数据的格式，允许传入指定类型和形状的数据。

创建变量
通过tf.Variable()函数封装张量来创建变量，例如：
my_var=tf.Variable(tf.zeros([row_dim,col_dim]))

【注意】声明变量后需要进行初始化才能使用，最常使用以下函数一次性初始化所有变量，使用方式如下：
init_op=tf.global_variables_initializer()

创建占位符
占位符仅仅是声明数据位置，也即先占个位，后面在会话中通过feed_dict传入具体的数据。示例代码如下：

a=tf.placeholder(tf.float32,shape=[2])
b=tf.placeholder(tf.float32,shape=[2])
adder_node=a+b   #这里的“+”是tf.add(a,b)的简洁表达
print(sess.run(adder_node,feed_dict={a:[2,4],b:[5.2,8]}))
输出结果为[7.2 12]

（3）操作
TensorFlow张量的加、减、乘、除、取模的基本操作是：add()、sub()、mul()、div()、mod()
其中，乘法、除法有比较特殊之处，如果是要对浮点数进行整除，则使用floordiv()；如果是要计算两个张量间的点积，则使用cross()

shape维度理解
第几个维度的长度，就是左数第几个中括号组之间的元素总数量
如
[[[1,2,3],[4,5,6]]]
# 第一个维度中只有一个元素[[1,2,3][4,5,6]]，所以第一个维度长度为1
# 第二个维度中有两个元素[1,2,3][4,5,6]，所以第二个维度长度为2
# 第三个维度中有三个元素“1,2,3”或“4,5,6”，所以第三个维度长度为3
# 那么它的shape参数就是[1,2,3]
如
[1,2]
shape=(2,)说明了张量result是一个一维数组，这个数组的长度是2

在Tensorflow的程序中，所有的数据都可以通过Tensor来表示，张量可以理解为多维数组
tf.Session.run() 要求输入对象是一个Tensor但是它的输出是一个Numpy数组，绝大多数可以同时混合使用Tensorflow类型和Numpy类型


tensorflow计算图、会话（Session）

大多数会话创建方式如下 
import tensorflow as tf 
a=tf.constant([1.0,2.0], name='a') 
b=tf.constant([3.0,4.0], name='b') 
result = tf.add(a,b)
with tf.Session() as sess:
       print(sess.run(result))

例子中的计算图为默认的计算图 


