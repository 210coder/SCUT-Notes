## 基于Kaffa的日志流收集项目

技术选型：参考ELK模型  es Logstash Kabana



why what how理论

why 背景

实验室有很多项目 图书搜索 音乐搜索

需要收集项目的访问日志 检查项目系统访问情况



what 是什么

日志收集项目包括三个模块，agent模块、transfer模块

how 怎么做的

用Kafka消息队列暂存消息日志，建立好不同的日志topic类型，系统访问成功200，失败404.

系统上有日志文件产生时，将日志文件路径写到etcd里面。

agent通过监控etcd数据的变化，有日志路径变化时，分配一个任务到线程池中，通过线程池分配一个线程读取日志文件，生产日志发送到Kafka不同的主题消息队列里面

tranfer模块按照主题不断地从Kafka里面消费消息，按照不同的日志写到Mysql进行持久化。



- **agent模块**：

每天产生一个日志文件，项目会把文件路径名更新到etcd里面，

agent 用一个线程去监听etcd 日志文件路径名的变化 有变化的时候  添加任务(new runnable)到线程池里面。

使用的线程池是固定大小（100) 的线程池

newFixedThreadExecutor(100)

创建一个核心线程个数和最大线程个数都为100的线程池，阻塞队列长度为Integer.max_value, keepAliveTime = 0,表示线程个数比核心线程个数多并且当前空闲则回收, 阻塞队列为LinkedBlockingQueue。



线程池new 开一个线程去生产消息 线程需要判断这个文件有没有存在 没有的话分配错误  读取日志里面的变化日志 这个是通过什么去检测变化的 



加入读取日志的线程崩了 通过心跳检测重新分配线程 从上一次的offset去读



线程每次在读日志文件的时候定期保持日志文件的offset（这里有个问题 假如读取日志的时候没到时间记录offset  这个时候线程崩了 下一次线程启动的时候读取的时候从上一次offset读取 这样会有重复 这种不可避免 性能权衡的问题）



- **transfer模块 **

  通过线程池异步去消费消息，创建一个线程池
  
  ④newScheduleThreadExecutor（n）
  
  创建一个核心线程个数为100, 最大线程数为Integer.max_value, keepAliveTime = 0,  阻塞队列为DelayQueue
  
  
  
  如果kafka集群里面topic有消息到达，则向线程池提交周期性的任务，每个任务时间间隔10s, 
  
   `scheduleWithFixedDelay()`(固定的间隔时间)



- etcd

 项目每产生一个日志文件，项目会把文件的路径名更新到etcd里面 监控管理日志收集任务的配置信息

etcd日志路径只有一个key value 每个key相当于一个项目系统ip  value 是这个项目下的日志收集 假设为json格式 里面包含非常多的日志路径



为什么用ETCD？

实现配置动态管理的功能





ssm框架中配置log4j环境



https://www.cnblogs.com/fawaikuangtu123/articles/10360264.html

配置Nginx日志格式  一条日志的日志格式

```
log_format  access_log_json ``'{"host":"$http_x_real_ip","client_ip":"$remote_addr","log_time":"$time_iso8601","request":"$request","status":"$status","body_bytes_sent":"$body_bytes_sent","req_time":"$request_time","AgentVersion":"$http_user_agent"}'``;
```

状态码、客户端浏览器版本



https://blog.csdn.net/weixin_34281537/article/details/89869842

数据库的数据表

MariaDB [elk]> create table nginx_log(host varchar(128),client_ip varchar(128),status int(4),req_time float(8,3),AgentVersion varchar(512), time TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin;

表结构

![image-20220324093437640](基于Kafka的日志收集项目/image-20220324093437640.png)

有一个主键id 递增的



项目的日志文件格式是否为json格式



数据库的日志格式

host    client_ip   status  req_time    AgentVersion    time

1. \-   192.168.20.191  304 0.023   Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36 2017-12-18 11:33:39
2. \-   192.168.20.191  200 0.042   Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36 2017-12-18 11:33:39
3. \-   192.168.20.191  200 0.030   Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36 2017-12-18 11:33:39

前面的主机host -





- 生产者和消费者的数量关系

kafka使用分区将topic的消息打散到多个分区分布保存在不同的broker上

Kafka的producer和consumer都可以多线程地并行操作，而每个线程处理的是一个分区的数据。

对于producer而言，它实际上是用多个线程并发地向不同分区所在的broker发起Socket连接同时给这些分区发送消息；而consumer，同一个消费组内的所有consumer线程都被指定topic的某一个分区进行消费。



- Consumer个数与分区数有什么关系？

topic下的一个分区只能被同一个consumer group下的一个consumer线程来消费，但反之并不成立，即一个consumer线程可以消费多个分区的数据，比如Kafka提供的ConsoleConsumer，默认就只是一个线程来消费所有分区的数据。即分区数决定了同组消费者个数的上限

[link](https://www.jianshu.com/p/dbbca800f607)



Etcd作为 KV 存储，会为每个 key 都保留历史版本，比如用于发布回滚、配置历史等。

观察 key的变化:

```nginx
etcdctl watch  foo --rev=0

```







## 你的项目是如何保证Redis的数据和Mysql数据一致性的问题的

1. 设置定期的过期时间

2. 手动删 delete key

3. mysql更新时把更新的部分写到redis里面



## 如何读取日志文件

1. 项目日志文件按照每日更新的变化 保存在每天的日志文件里
2. 通过开启线程不断读取每天的日志 不断循环从最后一次读的行read 当有新的日志行写入时 就会检测到变化
3. Kafka创建不断的日志消息主题 按照社交信息的类别 不断写入kafka 记录用户的系统行为 信息传播变化 后续用于日志的分析行为



## 高并发情况上如何解决

1. 部署多台服务器 集群管理 分流
2. 设置缓存 多级缓存
3. 用布隆过滤器查数据是否存在
4. 限制 访问权限 过滤掉一部分无效的访问
5. 设置分布式锁 



系统的用户级别



