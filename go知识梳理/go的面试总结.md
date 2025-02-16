### 一、Golang相关面试题
1、golang 中new 和 make的区别?

    new的作用是初始化一个指向类型的指针
	make的作用是为切片，map或channel初始化并返回引用

2、数组和切片的区别

	数组是在内存中开辟一段长度固定且连续的序列,需要指定大小的空间，长度不可变。
	切片是数组的抽象又叫做动态数组，长度是不固定的，可以追加长度使其容量变大
	注: 使用make([]int,len,cap) 声明数组 len:表示数组长度 cap:表示切片长度最大可以达到多少

3、说说关于chan的了解

	channel是golang语言中一个数据类型，用来解决go协程的同步问题以及数据共享问题，数据类型是以队列的形式进行存储，遵循先进先出的规则。
	（1）、什么情况下会造成阻塞
		在无缓冲区的情况下： 无缓冲通道一次只能传输一个数据，如果只有读端没有写端则读端阻塞,反之亦然
		在有缓冲区的情况下:  当缓冲区满时写入端阻塞,当缓冲区为空时读取端阻塞

4、golang中的锁有那些？

	互斥锁和读写锁,互斥锁又称同步锁
	互斥锁: 当一个goroutine获取到Mutex后，其它goroutine都进入等待状态直到这个锁释放掉
	读写锁: 在读锁情况下，会阻止其它goroutine写入,但不阻止读取
		在写锁情况下，会阻止其它goroutine进行读写操作

5、golang中有那些关键字

    var const type struct int uint string interface defer range  
    if switch else for default continue break return byte rune 
    make new chan go func select case 

6、关于main包

      main函数不能带参数
      main函数所在的包必须为main包
      main函数不能定义返回值
      main函数中可以使用flag包来获取和解析命令行参数

7、如何优雅的退出协程

        (1)使用context包来管理上下文，通过调用context.WithCancel()方法创建一个可以取消的上下文
    ，在协程中使用select语言来监听context.Done()信号
        (2)使用channel 加上select语句来通知协程退出的信号
        (3)使用sync.waitGroup 来等待协程的执行完成

8、了解Golang的内存管理吗？

        Golang使用了自动垃圾回收机制来管理内存，会自动检测和回收不再使用的内存。采用标记-清除算法，
    来进行标记和回收内存对象。可以减少内存泄漏和提高性能


9、调用函数传入结构体时，应该传值还是指针?说出你的理由?

    结构体传值，是会创建一个对象副本。而传指针则是对源结构体直接操作。
    
10、线程有几种模型？Goroutine的实现和优势说下？

    线程模型有：用户线程模型、内核系统模型还有轻量级线程模型。
    gorotine的实现：goroutine是由Golang运行时系统管理的，它使用了M:N的调度技术，将多个goroutine映射到少数
    的系统线程上运行，当一个Goruntine发生阻塞时，运行时系统会自动将其它可执行的goroutine进行调用，从而实现高效的并发
        优势：轻量级(创建和销毁的非常小，占用的内存也只有几k)
                并发能力强(可以创建成千上万个goroutine)
                内存占用低，
                通信机制:提供了channel来实现goroutine之间的通信

11、Goroutine什么时候会发生阻塞?

    网路IO阻塞：当Goroutine执行网路相关的读写操作时，如果没有数据可读或者暂时无法写入时，此时就会进入阻塞状态。
    channel阻塞：当Goroutine执行通道的读写操作时，如果通道为空或者已满是，也会导致goroutine阻塞
    系统阻塞：当Goroutine执行系统调度(文件IO),如果系统调用文件需要等待一段时间也会导致阻塞
    手动阻塞：手动添加互斥锁，条件变量、信号量来主动阻塞goroutine,直到满足特定条件
    
12、什么是PMG模型？有几种状态？

    PMG模型是Golang运行时系统中的一种抽象，用于描述Goroutine的调度和状态转换.
    P状态：表示协程正在执行中，
    M状态：表示协程正在等待可用的处理器，处于休眠状态。
    G状态：表示协程正处于阻塞状态。等待某个事件发生。

13、channel使用中需要注意的地方？

    避免在未初始化的channel上进行操作，不然会导致程序阻塞或panic
    避免在已关闭的channel上发送数据，如果一个channel已经被关闭，再向其发送数据会导致panic。
    重复关闭一个channel会导致panic


### 二、Http相关面试题
1、简单讲解什么是http

      Http是客户端和服务器之间数据传输的格式规范，表示超文本传输协议
      Http是超文本传输协议，用于服务器传输超文本到本地浏览器的传送协议
      Http的消息结构由请求行，请求头和请求体组成
  
2、http和https的区别

      http协议是明文传输的，不提供数据加密，所以数据不安全
      而https是在http的基础上利用SSL/TLS对数据进行加密，来保障数据的安全性
      2.1 说说Https是怎么保证数据安全的
        客户端验证服务器数字证书，这个证书从CA(数字证书认证机构)获取
        DH算法协商对称加密算法的密钥，hash算法的密钥
        SSL安全加密隧道协商完成
        网页以加密的方式传输，用协商的对称加密算法和密钥加密，用协商的hash算法进行数据完整性保护，
        保证数据不被篡改

3、http的常见的状态码有那些

      100-199 表示信息响应
      2 开头的状态码表示服务器成功响应
      3 开头的 表示重定向消息
      4 表示客户端错误响应
      5 表示服务器端出现问题

4、什么是http缓存

      Http缓存存储与请求关联的响应，并将存储的响应复用与后续请求

5、网络模型有那几层

      现在的网络模型有五层分别是: 物理层，数据链路层，网络层，传输层，应用层

6、Get 和 Post的区别

    Get和Post是Http常用的两种方法
    Get方法的请求参数是存放在Url中而get的url有长度大小，所以可携带的数据较少
    Post方法的请求参数放在Body当中长度没有限制，Get方法的请求参数是以明文
    存放在url中的，对于一些敏感数据，起不到保护作用
    Get请求的响应数据会被浏览器缓存起来，而post不会

7、什么是无状态协议,http是无状态的吗？

      无状态协议是指浏览器对于事务的处理没有记忆能力，Http是无状态的协议,
      cookie 能够让浏览器具有记忆功能
      JWT 也可以 

8、TCP的三次握手和四次挥手

      三次挥手是用来建立连接通道
      客户端发送一次SYN包用于建立与服务器的链接
      服务器接收到SYN包，则会发送SYN+ACK来做应答
      客户端收到服务的回应之后，也会发送ACK来回应服务器，此时服务器收到ACK之后双方建立链接
  
      四次挥手是用来断开连接
      客户端发送FIN包请求断开连接,并且进入FIN-wait-1等待状态,等待服务器响应
      服务器在收到断开连接请求后，立刻发送ACK包来做响应
      客户端收到服务器的ACK响应之后进入FIN-Wait-2等待状态，等待服务端的FIN包
      服务器在发送ACK确认消息之后一段时间，会发送FIN包给客户端告知客户端可以断开连接
      客户端收到FIN包后从FIN-wait-2状态改为time-wait状态,处于time-wait状态下客户端会
      发送ACK进行最后的确认，防止信息丢失。在等待一段时间后，连接关闭，客户端的所以资源都被
      释放

9、Http有那些请求方式?

      GET:  请求访问已经被URI(统一资源标识符)识别的资源，可以通过URL给服务器传递参数数据
      POST: 传输信息给服务器，与GET相似
      PUT:  传输文件，报文主体中包含文件内容，保存到对应的URI位置
      HEAD: 获取报文首部，与GET方法类似，一般用于验证URI是否有效
      DELETE:删除文件,与PUT方法相反
      OPTIONS: 查询对应URI支持的HTTP方法

10、TCP和UDP的区别

      TCP: 是基于连接的协议，在收发数据前必须建立稳定可靠的连接。
      UDP: 他是面向非链接的协议，不与对方建立可靠连接，而是直接把数据包发送过去，
    适用于只传送少量数据。对可靠性要求不高的应用环境

11、反向代理

      是指代理服务器来接收互联网上的连接请求，然后将请求数据转发给内部网络上的
    服务器，并把服务器上得到的结果返回给客户端。Nginx


三、Mysql相关面试题
1、MySQL的索引有那些

      (1)从物理结构上分为聚集索引和非聚集索引
        聚集索引:指索引的键值的逻辑顺序与表中对应行的物理顺序一致,每张表只能有一个
     聚集索引,也就是主键索引
        非聚集索引的逻辑顺序与数据行,的物理顺序不一致
      (2)从应用上可以划分为以下几类
        普通索引：Mysql的基本索引类型，没有限制，允许在定义索引的列中插入重复值和空值
    纯粹为了提高查询效率
        唯一索引：索引的值必须是唯一的，但是允许为空 (unique)
        主键索引：特殊的唯一索引，也称聚集索引，不允许有空值，由数据库帮我们自动创建
        组合索引：组合表中多个字段创建的索引，遵守最左前缀匹配规则
        全文索引：只有MylSAM引擎上才能使用，同时只支持CHAR、VARCHAR、TEXT 类型的字段使用

2、有哪些存储引擎

     InnoDB 和 MyISAM
      (1)谈谈InnoDB和MyISAM的区别
        InnoDB支持事务，而MyISAM不支持
        InnoDB支持外键，而MYISAM不支持
        这两均支持B+树数据结构的索引，但InnoDB是聚集索引，MyISAM是非聚集索引
        InnoDB必须由唯一索引(主键),如果没有指定，会自动寻找或生产一个隐藏ROW_ID
      来充当默认主键，MyISAM可以没有主键
      (2)InnoDB的优点:
        InnoDB支持事务，MyISAM不支持事务
        InnoDB支持外键，MyISAM不支持
        InnoDB是聚集索引，聚集索引的文件存放在主键索引的叶子节点上，因此InnoDB必须要有
      主键，通过主键查询加快查询速度和效率
        InnoDB恢复所花费的时间更短


3、mysql组合索引的最左前缀匹配原则

    最左前缀原则是发生在复合索引上的，只有复合索引才会有所谓的左右之分
    在检索数据时从联合索引的最左边开始匹配
    例：INDEX(a,b,c)
    where a = 1 and b =2 能用上a、b
    where b = 1 and a =2 能用上a、b （有mysql查询优化器）
    where a = 1 and c =2 能用上a 
    where b = 1 and c =2 不能

4、mysql如何优化

    设计合理的数据库结构,减少重复数据和冗余字段
    选择合适的数据类型可以减少存储空间和提高查询效率
    创建适当的索引: 提高查询效率和性能
        优化mysql查询语言，避免使用 select * 和 like % ，需要哪些字段就查哪些字段就行了，like % 可能会导致全表扫描
    占用过多的开销
    使用缓存:减少数据库的访问次数，提高性能如redis


5、mysql事务的隔离级别

    (1) 读未提交
    (2) 读已提交
    (3) 可重复读
    (4) 串行化

6、Mysql的索引主要包括以下几种类型

    BTree索引：是mysql默认的索引类型，基于b数数据结果实现，他可以加速等值查询，范围查询和排序查询等操作
    哈希索引:基于哈希表实现，可以快速进行等值查询，但是不支持范围查询和排序等操作
    全文索引: 基于全文索引技术实现，可以快速进行文本匹配查询，但是不支持排序等操作
    空间索引: 基于空间数据结构实现，可以快速进行空间范围查询，但是不支持排序等发操作，适用于空间数据 查询较多的场景

四、Redis面试题
    
1、redis的数据类型有哪些？

    字符串型，哈希，列表，集合，有序集合和位图

2、redis持久化怎么做的？

    redis持久化有两种方式：RDB和AOF
    RDB:是一种快照的持久化方式，会将数据以二进制格式保存到磁盘中,可以设置定期或手动触发保存来进行数据持久化
    AOF:是一种追加日志方式的持久化方法，将redis的写操作以文本格式追加到AOF文件中。

五、其它问题 
1、遇到bug该如何解决？

        首先要定位问题，通过查看错误日志，看看是那个接口哪条代码的出了问题，在本地代码通过dehug的模式下
    看看能不能复现刚才的hug,定位到问题之后，就可以着手开始解决


