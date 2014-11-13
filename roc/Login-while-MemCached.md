# MemCached缓存
陈鹏-2014年11月12日 13:48:43

---
##MemCached简介
`Memcached` 是一个高性能的分布式内存对象缓存系统，用于动态Web应用以减轻数据库负载。它通过在内存中缓存数据和对象来减少读取数据库的次数，从而提高动态、数据库驱动网站的速度。`Memcached`基于一个存储键/值对的`HashMap`。

##原理示意图
许多Web应用都将数据保存到RDBMS中，应用服务器从中读取数据并在浏览器中显示。但随着数据量的增大、访问的集中，就会出现RDBMS的负担加重、数据库响应恶化、网站显示延迟等重大影响。
![MemCached原理示意图][1]

##内部主要实现原理
通过在内存里维护一个统一的巨大的hash表，memcached能够用来存储各种格式的数据，包括`图像`、`视频`、`文件`以及`数据库检索`的结果。

##特点
**基于C/S架构，协议简单。
基于liberent的事件处理。
自主内存存储处理。
基于客户端的memcached分布式**

对于其中一些特点的理解：
[libevent][2]是一套跨平台的事件处理接口的封装，能够兼容包括这些操作系统：Windows/Linux/BSD/Solaris 等操作系统的的事件处理。

##存储处理：
###数据存储方式：Slab Allocation
![slabclass][3]

该存储方式的特点是：Slab Allocation的原理相当简单。将分配的内存分割成各种尺寸的块（chunk），并把尺寸相同的块，但是分成组由于分配的是特定长度的内存，因此无法有效利用分配的内存。例如，将100字节的数据缓存到128字节的chunk中，剩余的28字节就浪费了（如下图）。针对这个问题的解决方案是：
![byteschunk][4]

The most efficient way to reduce the waste is to use a list of size classes that closely matches (if that's at all possible) common sizes of objects that the clients of this particular installation of memcached are likely to store.
就是说，如果预先知道客户端发送的数据的公用大小，或者仅缓存大小相同的数据的情况下，只要使用适合数据大小的组的列表，就可以减少浪费。
![100in128][5]

##数据过期方式：Lazy Expiration + LRU
###Lazy Expiration
**MemCached**内部不会监视记录是否过期，而是在get时查看记录的时间戳，检查记录是否过期。这种技术被称为`lazy`（惰性）expiration。假如我们所存储的数据项相当多的时候，在这时候进行监控的话，花费的代价是相当大的，所以memcached不会在过期监视上耗费过多的CPU时间，从而在性能方面也起到一定的优化作用。
###LRU
**MemCached**会优先使用已超时的记录的空间，但即使如此，也会发生追加新记录时空间不足的情况，此时就要使用名为 Least Recently Used（LRU）机制来分配空间。顾名思义，这是删除“**最近最少使用**”的记录的机制。因此，当memcached的内存空间不足时（无法从slab class 获取到新的空间时），就从最近未被使用的记录中搜索，并将其空间分配给新的记录。从缓存的实用角度来看，该模型十分理想。

---
##Sequence Diagram简介
`时序图`，也成为`序列图`或`循序图`，是一种UML行为图。它通过描述对象之间发送`消息`的**时间**顺序显示多个对象之间的动态协作。它可以表示用例的行为顺序，当执行一个用例行为时，时序图中的每条消息对应了一个类操作或状态机中引起转换的触发事件。

##时序图元素
时序图包括如下元素：**对象（Actor），生命线（Life line），控制焦点（Focus of control）和消息（Message）**。

**消息**
消息一般分为同步消息（Synchronous Message），异步消息（Asynchronous Message）和返回消息（Return Message）

##我做的一个例子

![登陆的时序图，带有缓存MemCached][6]


  [1]: http://cshijiel.github.io/roc/images/MemCached%E5%8E%9F%E7%90%86%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg
  [2]: http://baike.baidu.com/view/1590523.htm
  [3]: http://cshijiel.github.io/roc/images/slabclass.jpg
  [4]: http://cshijiel.github.io/roc/images/byteschunk.jpg
  [5]: http://cshijiel.github.io/roc/images/100in128.jpg
  [6]: http://cshijiel.github.io/roc/images/login-Sequence.png