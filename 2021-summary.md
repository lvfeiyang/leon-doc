1、总结：CASE 和 IF的区别:
·在高级语言中，CASE的可以用IF来替代，但是在SQL中不行。
·CASE是SQL标准定义的，IF是数据库系统的扩展。
·CASE可以用于SQL语句和SQL存储过程、触发器，IF只能用于存储过程和触发器。
·在SQL过程和触发器中，用IF替代CASE代价都相当的高，相当的麻烦，难以实现。
通过上面几组实例可以看出，应用CASE语句可以让SQL变得简洁高效，从而大大提高了执行效率。
而且，CASE的使用一般不会引起性能（相比没有用CASE的语句）低下，反而增加了操作的灵活性

IF(source="趣投稿", 0, 1) as is_audit
CASE source WHEN "趣投稿" THEN 0 ELSE 1 END as is_audit

CASE   <单值表达式>
        WHEN <表达式值> THEN <SQL语句或者返回值>
        WHEN <表达式值> THEN <SQL语句或者返回值>
        ...
        WHEN <表达式值> THEN <SQL语句或者返回值>
        ELSE <SQL语句或者返回值>
 END

2、shell执行awk命令 定义两个字符串 awk "$hh" "$tt"

3、注意from等宏(go的iota 类枚举)的定义，是顺序定义的，注意从后插入

4、kafka客户端自动平衡研究，是golang的库不支持，见下文
To consume messages, use the Consumer. Note that Sarama's Consumer implementation does not currently support automatic consumer-group rebalancing and offset tracking.
 For Zookeeper-based tracking (Kafka 0.8.2 and earlier), the https://github.com/wvanbergen/kafka library builds on Sarama to add this support. 
For Kafka-based tracking (Kafka 0.9 and later), the https://github.com/bsm/sarama-cluster library builds on Sarama to add this support. 

推荐使用go库 https://github.com/confluentinc/confluent-kafka-go

kafka的链接使用tls/tcp

5、grpc建立微服务
https://segmentfault.com/a/1190000008672912   LB的实现方案

6、php 厉害的框架 drupal

7、邮件标题注意，群组里有其他部门的人，写清楚什么上线，“算法策略上线”

8、tcpdump参数 https://www.cnblogs.com/ggjucheng/archive/2012/01/14/2322659.html

9、矩阵向量 图形 v = xp + yq + zr 可以用[p,q,r]表示。所以夹角就可理解了

11、利用反射时，即使不是指针也可改变其值与排序

12、统计学相关：后验概率就是条件概率；范数先简单理解为距离，维数（模型参数越多）范数越大；

13、go的矩阵运算gonum

14、机器学习视频教程 https://developers.google.cn/machine-learning/crash-course/

15、golang的slice可以理解为指针，子函数内修改生效，但扩展append不生效，因为会改变slice地址，这时需要指针

16、git revert -m 说明：http://www.cnblogs.com/520yang/articles/6732687.html
在你合并两个分支并试图撤销时，Git 并不知道你到底需要保留哪一个分支上所做的修改。从 Git 的角度来看，master 分支和 dev 在地位上是完全平等的，只是在 workflow 中，master 被人为约定成了「主分支」。
于是 Git 需要你通过 m 或 mainline 参数来指定「主线」。merge commit 的 parents 一定是在两个不同的线索上，因此可以通过 parent 来表示「主线」。m 参数的值可以是 1 或者 2，对应着 parent 在 merge commit 信息中的顺序。

17、bin小流量测试，不好进行ab平台测试的，进行分小流量bin测试。绝大多数的修改都要进行ab。

18、传interface，传递的是一个interface对象，这个对象占用16字节长度，包含一个指向原数据的指针，和一个指向运行时类型信息的指针

19、推荐的内容库 alter add 耗时11min

20、召回模型。正负样本选择。特征向量的选择与计算。梯度计算。doc的建树与剪
fm模型。两两特征之间的关系。

21、go testing的使用 https://studygolang.com/articles/5499

22、tcpdump tcpreplay（指定回放）

23、cmd设置代理 http_proxy https_proxy no_proxy
set http_proxy=http://127.0.0.1:1080
set no_proxy=git.qutoutiao.net
https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2013/hh272656(v=vs.120)

24、碰到有损失推荐的情况，无论多少流量，一查到底！！ 遇到不确认的问题及时联系

25、golang的 append 并发会导致数据少

26、sidecar工具的推荐 servicemesh架构 微服务框架推荐

27、centos的开机启动 新建supervisord.service 拷贝到 /usr/lib/systemd/system/ 启动服务 systemctl enable supervisord 验证 systemctl is-enabled supervisord

28、均值 方差计算 https://blog.csdn.net/u014485485/article/details/77679669
En=En-1 + (xn-En-1)/n
Fn=Fn-1 + (xn-En-1)(xn-En)

29、模型相关知识 https://blog.csdn.net/u014595019/article/details/52989301

30、数据压缩parquet

31、rust 不同消息CommMsg结构的定义 不同于golang的接口方式 可以使用 enum枚举的方式

32、包管理工具bazel

33、auc的计算 https://blog.csdn.net/zhaohang_1/article/details/92794489

34、go的逃逸问题与函数定义。指针类型不要作为返回值，而作为输入型的输出

35、对一般的同学，定好上线流程与检查内容，一步步尽量详细

36、标准要求编译器在实例化模板时必须在上下文中可以查看到其定义实体；而反过来，在看到实例化模板之前，编译器对模板的定义体是不处理的——原因很简单，编译器怎么会预先知道typename 实参是什么呢？因此模板的实例化与定义体必须放到同一翻译单元中

37、https://www.jianshu.com/p/f5920842b27b 对supervisor不能coredump的处理