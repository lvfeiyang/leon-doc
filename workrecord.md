## 工作内容

7.15
完成米读预估的适配
在tulong代码的基础上，完成米读粗排预估的开发
完成米读读取hdfs文件获取模型nn信息的功能，完成k8s的预估迁移

3.16：调assemble的tulong预估，输入embed大多为0，排查中
3.17：解决预估取fs失败的问题：去掉etcd的发现，修改请求的url。现可正常取特征，预估值正常范围。

3.22: 正式ctr环境搭建，代码库整理
3.23: 引擎联调

2021.2
米读：
1、协助整理打点逻辑
2、推动reqid落地事宜
3、推动全场景样本落地
4、特征补全 context相关特征 session类特征
5、assemble的调试维护



2020下半年
一、萌推
1、粗排、用户向量（dssm模型）服务的预估支持，p99耗时5ms，保证服务稳定性。
2、流失率、品牌、类目、店铺等模型预估支持，各小流量场景实现k8s集群混部，节约资源。
3、session特征的推理端开发，9-10完成go版本保证上线。9-30完成cpp版本相比go版本使用资源节约一倍。
4、12-30前完成预估dump样本的功能开发上线，减少不一致的问题。
5、9~10月支持旧版预估下线节约成本10w左右；11月替换16台32c128g机器，节约成本6k/月。

二、米读
1、11-25前完成已有数据流问题排查：
midu_book_exposure推出的底表缺失问题，导致训练样本缺失30%以上；
新流label表拼接的时间段错误问题；
协助解决parquet schema变化带来的训练问题；
2、12-15前完成24h的阅读深度样本数据流，保证时长模型的正常训练。

三、实惠喵、键多多
1、08-06支持完成键多多的第一版模型预估上线。
2、10-21支持完成实惠喵的ctr、cvr等模型上线。
3、协助排查实惠喵的一致性问题，修复指数分桶等问题。



11.26
米读
1、修复新数据流样本少的问题
2、开发24小时阅读深度的样本表
3、协助修复新数据流的样本读取问题
萌推
1、开发支持粗排的ctr、cvr多分值返回
2、开发支持cgc的模型预估上线

学习：tf、flink
推理端，基于 TF LIB 开发 DNN 推理，初步支持至少 2个精排模型上线，RT 控制在 X，服务稳定性达到 Y，取得 Z 的线上效果；

1、搜索算子一致性check对比
2、新特征的开发
3、

parquet格式 与tfserver的源码

7、8月
预估的C++迁移
对特征的fs、de的负责。收口
对引擎的了解

对de特征的规划：
需求：
静态、动态、离线t+1特征

静态收口到自建的MySQL表：1、方便只保留需要的特征 2、方便添加自定义的特征（如切词、聚类等）

离线特征的多hive表同步到一个de表。不同特征分散在不同的表中，更新的时间也有差异：1、可以提高部分特征的实时性 2、减少了统一宽表的维护成本


5、6月okr计划

研究下 tfserver的源码
rust对predict的重构
模型训练 aibox的研究

旧的v3b适配新框架
fs与de对齐
增加from特征

启动时的耗时原因
统一的hash 可行性 研究
hadoop parquet格式研究

各位写自评的时候，注意以下几点：
1、本次是半年度绩效评估，所以请回溯过去半年工作。
2、peer review请认真对待。
3、自评请体现以下几个方面内容：业绩（工作中产出），技术能力（个人/团队的技术积累和提升），价值观（公司价值观体现案例），团队协作性（个人与他人的协作）。


1、开发完成加载模型的预估服务。完成正常预估、监控、check验证等功能；k8s的快速部署，模型部署上线由1天缩减到半小时；保证了dnn版本预估的正常上线，在多数场景的uv价值有10%的提升。
2、预估服务的性能优化。性能由50qps逐步提升至300qps，p99耗时保证100ms以下，可用性保证两个9；节约了机器成本，由原来ftrl集群200台左右，降低到k8s预估集群70台；
3、特征获取由feature_server替换为de。提升获取特征性能，p99耗时减少10ms左右。
4、相比之前，个人在golang性能优化上略有提升，对go的内存管理、gc过程、协程调度等都有了进一步的理解。


S 在预估服务加载多交叉复杂模型的时候，性能衰减的较为严重时，为了提升性能；自驱的去了解golang相关的性能优化，对gc过程、内存管理做了更多的了解后，重构了相关的代码；完成了性能的提升，可支持300多的qps
T
A
R

亮点：在性能优化上，对go的内部逻辑进一步做了了解
提升点：对项目的整体进度缺少把控，在de迁移的任务中，部分特征的迁移还是存在delay


- 6.30
排查模型切换时的程序卡顿问题
de版本的相关报错问题
chan导致的de获取耗时等高问题

1、chan的开销：本身是指针 内容是memmove 会lock g
2、goroutine独占：无方法 oslock只是绑定m 不会独占。可尝试1.14，calculate耗时减少,de_mod的超时减少更准？
3、calc的pod耗时不均：不是g、m的数量问题
4、mod的切换耗时：排除 hadoop的带宽占用（200MB，机器8Gb左右）。map的内存消耗，mapclear内存置零不清理空间


- 6.8
1、适配de的上下文特征格式开发

- 5.25
1、验证item的一致性

- 5.19
1、整理对de的需求文档
2、替换de：开发测试

- 5.18
1、替换de：批量调度的处理开发

- 5.15
1、替换de：单特征的处理、测试完成。交叉与批量的开发测试

- 5.14
1、替换de：go版本开发完成80% 进行测试
2、预估掉用feature：增加熔断机制

- 5.13
1、替换de的方案，先实现纯go版本的适配踩坑

- 5.12
1、替换de。batch接口的对接调试
2、用v3c的c版本的错误率更低，可能是item的缓存时间造成的影响？进行验证

- 5.11
1、查v3b的不一致问题，测试等方案已都试过，不能排除。后续再追查只能线上记录更加详细的日志。
2、替换de方案，调研go的protoreflect看能否满足需求，决定与de的接口定义。

- 5.9 周报
1、查v3b的不一致问题，排除
	a、模型加载切换机制导致的错误：应该与cvr_v1a相仿；8号停更也未恢复
	b、模型计算或特征的不一致：离线验证万分千分位错误；泰杨帮忙模拟延迟后错误率依然很低
	c、不同实验的影响：过滤不同实验的结果都差不多
	d、engine内部：有一处对pscore的修改，但无人掉用
2、替换de：整理第一期模型替换需要使用的特征；研究pb协议，确认和de间的接口

- 5.8
1、查v3b的不一致 泰杨延迟 2% 下掉4台 停止更新 均未恢复
2、研究pb协议 3月新出的reflect包 研究
3、parquet的go版解析

- 5.7
1、推全实验 推迟观察一天
2、查v3b 16点模型18点还有打分 可能和缓存保护相关；排除a、模型加载慢 b、模型切换和cvr_v1a相仿 c、离线验证万分千分位错误
3、定pb结构 确定de需要的字段，具体格式研究pb是否可以根据key来获取value
Q:搜索的流量会导致取特征耗时变高

- 5.6
1、研究parquet格式


DNN预估总结
1、提升机器使用效率：由原来200多台的预估机器降低到60多台，迁移dnn累计回收115台机器
2、线上部署改用k8s，节约人效：训练完成的模型，5分钟内可以完成线上的部署
3、性能提升：对简单模型的p99耗时在60~80ms。复杂模型的p99在100~120ms；预估错误率降低至0.3%~0.5%

4、技术总结：对pprof的性能分析。
io问题：
对item的缓存处理sync.map（不进行编辑吗）。
限流器未启动理想效果，使程序内的协程数变多。卡http的连接数

gc问题：
使用pool对hash结果的碎片处理。
对加载的id embed处理 只记录列表的所在位置，不使用slice结构，减少hash
对protobuf结构 使用gogofaster模式

每个样本的特征重点关注：（因循环次数过大）
对用户的特征样本仅处理一次，包括用到的hit特征。
对murmur3升级，减少逃逸的变量


3~4月的okr review
1、稳定性与性能的优化：简单模型80~100qps，复杂模型40qps的耗时在100ms以下
2、功能完善：分不同模型的监控告警；在离线的一致性校验5分钟内的快速check等
3、hellow项目的功能完善：附近的人使用s2算法进行区域的划分
4、增加特征：上下文中场景等特征的完善；de特征的接入
5、实验及效果



- 4.21
今日计划：
1、开发check hash落盘功能

- 4.20
今日计划：
1、完善check hash 对比泰杨的表

- 4.16
昨日进展：
1、dump流，引擎调用de接口。开发完成
2、掉预估接口直传de的user特征，对de的数据适配统一在引擎完成
今日计划：
1、预估的代码整理
2、完善check hash 对比泰杨的表

- 4.15
昨日进展：
1、dnn预估：对接de接口。测试，调试断链心跳等流程。
1、dump流，引擎调用de接口。开发中
今日计划：
1、dump流，引擎调用de接口。开发测试

- 4.15
昨日进展：
1、dnn预估：对接de接口，接口已通
今日计划：
1、dump流，引擎调用de接口。增加de中用户行为

- 4.14
昨日进展：
1、dnn预估：对接de接口
1、dnn预估：适配c_3_relative_pos特征，上线v2c模型
今日计划：
1、dnn预估：对接de接口


1、hit特征的开发上线
2、go版本hash的开发

- 4.1
1、增加modname的透传上报逻辑；增加o_score打分
2、hellow项目：区分付费用户的类型；修改附近的人排序规则

- 3.30
1、dnn预估：go版的hash、交叉特征开发。上线

- 3.25
昨日进展：
1、dnn预估：调度的cgo调用优化
今日计划：
1、dnn预估：交叉特征的功能支持

- 3.24
昨日进展：
1、dnn预估：合并各功能的上线实验
今日计划：
1、dnn预估：调度的优化测试

- 3.23
昨日进展：
1、dnn预估：合并变长embed、user特征分离优化、配置热加载。并压测
今日计划：
1、dnn预估：合并各功能的上线实验

- 3.20
昨日进展：
1、dnn预估：优惠卷场景在离线仍然不一致的异常情况，确认为新开cor实验进行了itemid的转换
2、hellow项目：排查活跃时间问题，为上报时间错误，已修改加入本机时间对比
今日计划：
1、dnn预估：合并变长embed、user特征分离优化、配置热加载。并压测

- 3.19
昨日进展：
1、dump搜索时的特征
2、dnn预估：排查在离线不一致的异常情况。修复cache未定期清理的问题
今日计划：
1、dnn预估：优惠卷场景在离线仍然不一致的异常情况

- 3.18
昨日进展：
1、推动后端传递pos_id的相关信息：与卡缪确认，可以由前端透传。与青峰确定了后端请求的透传字段
2、dnn预估支持变长embed：开发，联调
3、hellow项目：开发多种付费用户类型的区分
今日计划：
1、dnn预估：cvr模型扩容。排查同一node俩pod的异常情况
2、dump搜索时的特征
3、dnn预估：排查cvr的异常情况

- 3.17
昨日进展：
1、推动后端传递pos_id的相关信息。有难度，对方案中。备选方案：用现有的actid、tagid生成
2、dnn预估支持变长embed，开发中
3、dnn预估增加context设置，防止模型切换前后不一致的情况
今日计划：
1、dnn预估支持变长embed开发

- 3.16
昨日进展：
1、梳理下context特征
今日计划：
1、增加新的context特征，位置与场景信息
2、预估服务优化，增加context超时等配置

- 3.13
昨日进展：
1、hellow项目：开发用户行为消费服务 （完成）
今日计划：
1、梳理下context特征，新用户特征

- 3.12
昨日进展：
1、排查dump流问题:生成context特征提前，导致rectype变量没有获取到
2、hellow项目：开发用户行为消费服务（开发中）
今日计划：
1、hellow项目：开发用户行为消费服务
2、梳理下context特征

- 3.11
昨日进展：
1、dump用户特征的逻辑优化：修改在engine侧进行dump。（测试上线完成）
2、集团k8s环境：完成dnn部署相关文档
3、修复搜索的上下文特征错误的问题
今日计划：
1、排查dump流修复问题
2、hellow项目：开发用户行为消费服务

- 3.10
昨日进展：
1、dump用户特征的逻辑优化：修改在engine侧进行dump。（开发完成）
2、hellow项目：增加mcn用户强插。展示付费用户去重逻辑。（完成）
今日计划：
1、dump用户特征的逻辑优化：修改在engine侧进行dump。（测试）
2、集团k8s环境：完成dnn部署相关文档

- 3.9
昨日进展：
1、集团k8s环境：基本可用。遗留问题：1、编译参数的传递。2、work用户的hadoop拉取
今日计划：
1、dump用户特征的逻辑优化：修改在engine侧进行dump。
2、hellow项目：增加mcn用户强插。展示付费用户去重逻辑。

- 3.7
1、集团k8s环境：新dnn预估镜像对接
完成 遗留问题：1、编译参数的传递。2、work用户的hadoop拉取。

- 3.6
昨日进展：
1、dnn预估:原预估dump环境恢复。完成
2、集团k8s环境：新dnn预估镜像对接。镜像生成gomod拉取会超时，改为govendor模式
今日计划：
1、集团k8s环境：新dnn预估镜像对接

- 3.5
昨日进展：
1、集团k8s环境：新dnn预估镜像dockerfile等文件的编写
2、dnn预估:优惠卷上线cvr环境
3、dnn预估:原预估dump环境恢复
今日计划：
1、dnn预估:原预估dump环境恢复
2、集团k8s环境：新dnn预估镜像对接

- 3.4
昨日进展：
1、集团k8s环境：原来的sml部署验证完成。新dnn预估镜像打包验证中
1、dnn预估miss率的调试分析:增加特征明文落盘验证
今日计划：
1、dnn预估上k8s环境部署
2、dnn预估pscore的auc偏低排查

- 3.3
昨日进展：
1、dnn预估miss率的调试分析:去除miss严重的特征验证
今日计划：
1、dnn预估上k8s环境部署

- 3.2
昨日进展：
1、dnn预估miss率的调试分析
2、海外：强插功能的实现
今日计划：
1、dnn预估性能优化: 统一user处理一次
2、dnn预估测试环境的miss率长期监控

- 2.28
昨日进展：
1、dnn预估性能优化:
统一user处理一次 （开发中）
优化gc到80%（开发中）
今日计划：
1、监控完善：embed的miss率 log时间 版本信息 feature的miss率

- 2.27
昨日进展：
1、dnn预估优惠卷场景小流量上线
2、性能优化，继续找地方加缓存池
今日计划：
1、dnn预估性能优化

- 2.26
昨日进展：
1、上线文件、环境、脚本等的准备，申请4台32c机器已部署
2、调整缓存区大小、gc间隔等，优化耗时10ms左右
今日计划：
1、dnn预估性能优化

- 2.25
昨日进展：
1、模型的md5文件检测功能实现
2、dnn预估性能优化：内存使用分析
今日计划：
1、上线文件、环境、脚本等的准备
2、dnn预估性能优化

- 2.24
1、模型的md5文件检测功能实现
2、dnn预估性能优化：内存使用分析

- 2.21
1、切换使用bigcache
2、模型的动态更新加载功能实现

- 2.20
昨日进展：
1、调试解决压测中的问题，初步判断为freecache中的锁有影响
今日计划：
1、对比 freecache bigcache fastcache go-cache 三个库
2、模型的更新功能实现

- 2.19
昨日进展：
1、预估压测各模块耗时统计报表
2、预估切分batch
今日计划：
1、预估压测换机器压测

- 2.18
昨日进展：
1、预估hash的缓存，hash模块的平均耗时降低到20ms。
今日计划：
1、预估压测各模块耗时统计报表
2、预估的文件更新

- 2.17
1、预估的联调性能优化
周计划
1、预估的性能优化(
	1、并行优化处理（hash部分已处理 done 效果不大 增加了goroutine的调度开销和cache、cgo本身并发效率不高，小batch并行 done）
	2、减少hash的计算，hash的缓存，用户部分只计算一次 done
	3、item的特征缓存预加载
	4、context的超时设置
	5、tf请求数量拆分为小batch 对比ftrl拆分10个队列，每个处理50个 done
	6、手动release ReleaseTensor
	7、内存池优化
	8、逃逸分析
	9、embed的map有gc问题 改为数组
) 整体看下prof的监控
2、预估的文件更新，根据文件时间动态变更加载模型等文件；会有个服务接口，轮询是否更新
3、预估的k8s部署

- 2.14
昨日进展：
1、海外项目的部署上线。
今日计划：
1、预估的联调性能优化
2、预估的文件更新
3、预估的k8s部署

- 2.13
昨日进展：
1、预估的联调压测，解决异常特征的处理问题，压测平均耗时400ms左右待优化。
今日计划：
1、预估的联调压测：检查embed结果
2、海外项目相关bug解决

- 2.12
昨日进展：
1、预估的联调、压测：新的模型文件载入完成；线上流量copy压测工具对接完成；
今日计划：
1、完成预估的联调压测
2、海外项目相关bug解决

- 2.11
昨日进展：
1、预估模型完善配置区分不同的特征处理方法
2、分模块压测（开发中）
今日计划：
1、完成预估的联调压测
2、海外项目相关bug解决

- 2.10
昨日进展：
1、预估结合feature_process的库，开发完成
2、海外项目增加部分筛选逻辑
今日计划：
1、预估模型结合给到的模型文件 分模块压测（开发中）
2、海外项目增加垂直频道

- 2.7
本周：
1、补全预估模型的异常流程与测试用例
2、集成feature的统一处理库(50%)
3、预估模型的压测qps，落checkpoint
4、海外新项目的支持，搭建完成测试环境
下周：
1、完善预估参数更新、k8s部署等功能，小流量上线
2、海外新项目的支持，部分场景的逻辑实现

- 2.6
昨日进展：
1、海外项目的测试环境搭建，完成
今日计划：
1、预估结合feature_process的so
2、预估压测

- 2.5
昨日进展：
1、补全预估相关单元测试
2、海外项目的测试环境搭建
今日计划：
1、海外项目的测试环境搭建
2、预估结合feature_process的so

- 2.4
昨日进展：
1、预估完善异常流程的处理
2、海外项目item同步代码（50%）
今日计划：
1、补全预估相关单元测试
2、海外项目的测试环境搭建

- 2.3 
昨日进展：
1、
今日计划：
1、补全预估模型的异常流程与测试用例

- 2.3 本周计划：
1、补全预估模型的异常流程与测试用例
2、集成feature的统一处理库
3、预估模型的压测qps，落checkpoint
4、海外新项目的支持

- 1.16~20:
1、开发新版的预估服务
2、新业务接口

- 1.15:
1、开发预估模型加载部分，模型已可load

- 1.14:
1、开发预估模型加载部分（20%）
2、开发特征获取预处理部分

- 1.13:
1、粗排服务，完成服务框架（50%）

- 1.10:
1、k8s机器跨区问题，置换E区机器
2、新版粗排服务研究、开发
3、整理升级tf编译开发环境

- 1.9:
1、pgmv切主备ps（等机器审批）
2、train_server的容器化，镜像生成，等ps机器申请
3、粗排预估开发（资料调研）

- 1.8:
1、k8s环境基线ctr对比ab实验
2、新数据流在商详相关实验

- 1.7:
1、粗排服务设计、开发（tf的go包基本可行）
2、消息中心、频道、优惠卷开启新数据流实验，晚些同步实验数据

- 1.6:
1、调整k8s的预估限流控制，16c32g支持并发80

- 1.3:
1、gauc开发

- 1.2:
1、反作弊数据的新模型上线预估实验
2、k8s预估开始灰度测试

- 12.31:
1、完成pcap流量复制工具，灰度测试k8s平台（开发完成）
2、搭建反作弊数据的新模型

- 12.30:
1、搜索分开预估和feature的配置，单独增加feature调权的配置
2、协助定位基线预估服务的性能问题，ctr为内存不足，cvr为ps的带宽不足

- 12.27:
1、开发转发压测工具

- 12.26:
1、观察调整pgmv实验
2、定位新版数据流train时的样本丢失问题
3、修改k8s预估的make文件--配置和线上一样的ctr环境，准备ab替换

- 12.25:
1、定位新版数据流feature_server缺少特征的问题
2、定位小程序的数据上报问题

- 12.24:
1、pgmv的权重参数调整，有部分参数单价、uv价值提升8%以上，继续观察
2、协助解决新版feature_server的问题，已可上报调试

- 12.23:
1、pgmv实验推全
2、排序模型实验参数检查冲突
3、c++预估服务的调研

- 12.22:
1、k8s调试
2、预估修改dump仅dump用户特征
3、讨论系统方案

- 12.21:
1、k8s环境部署联调

- 12.20：
1、特征的时戳修改64位

- 12.19：
1、engine预估函数加上下文特征参数
2、特征的时戳修改64位

- 12.18：
1. PGMV模型预估扩容；在首页开启实验
2. check预估dump的数据流丢失现象

- 12.17：
1. engine增加预估的ab实验主key，方便预估单独开层实验
1. PGMV模型在频道页，消息中心数据上线；频道页数据，目前UV价值表现正向，符合预期，持续观察中


2019.12.13(5)
1、预估的函数统一，全场景dump数据
2、新版ps调研

2019.12.6(6)
1、engine 调整首页下拉刷新逻辑
2、predicate的dump功能优化，engine的预估id不转换

2019.11.30(5)
1、mt搜索和模型的熟悉
2、特征缓存过期的问题排查
（缓存超时过期，load fea cache的select性能问题被大量filter调用）
（发现解决缓存过期问题；改变cache的实现方法）
3、引擎的流程梳理
4、萌推ab的错误的统计；predicate对样本的dump；搜索的预估分验证和落盘；

2019.11.22(6)
1、完成zerolog替换tlog的开发，协助66那替换上线，性能有所提升。
2、配合数据流完成“未有效曝光的回捞召回”下线。

2019.11.15(5)
1、grpc兼容ab信息的传递
2、grpc预发环境的联调测试
3、zerolog完全替换tlog的开发，开发中

2019.11.8(6)
1、engine调用召回的grpc接口改造
2、测试环境的联调测试

中间：
代码清理等

2019.10.8(5)
1、完成统一关键业务的log输出
2、整理排序等相关流程
3、和技术中心那看看有没有新的方案，改进现有的grpc、服务发现、负载均衡的方法

2019.9.16(4)
1、trace平台细化：解决相关排序error；接入后端联调；增加召回和排序的span
2、log规范 log的详设

2019.9.9(4)
1、trace的接入联调
2、日志规范整理

2019.9.2(5)
1、engine的打点log整理、接入trace平台
进行重构，缝缝补补的感觉。eg trace加入入库的消息解析，要加参数 改动多
社区的任务交接
2、社区增加作者相关的推荐等策略
3、引擎接口整理
3、萌推dnn

2019.8.27(6)
1、定位保量等策略多出的问题，确认为首刷等策略调整去除了强插字段
2、社区修改类目打散，限制3条策略
3、引擎制定方案

2019.8.19(5)
1、解决新文章冷启动问题
2、启动fm模型实验，去除红包行为的数据
3、修改文章静态特征的写入redis和dump同步
4、社区引擎使用bloom过滤器

2019.8.12(6)
1、fm模型训练，train_client、train_server环境搭建
2、新视频接口的数据上报的适配处理

2、视频接口调试
3、准备时长特征的上报
4、查James1002的出现，新用户特征

2019.8.5(5)
1、确认保量、冷启动出多条的case，判断基本为新用户策略造成
2、社区策略：先出运营配置的文章
3、word2vec过滤视频和影集的结果
4、准备graph embedding的数据
5、文章关键字的提取，文章静态特征存储
6、训练环境搭建，代码修改基本完成

2019.7.29(5)
1、社区推荐视频方案确认
2、作者保量替换新的ps实验 pv不显著 +0.32% 时长不显著 +0.17%
3、排查问题：base机的错误，未定位，做了部分优化；作者保量出2条的问题定位；

2019.7.8(6)
1、社区推荐的作者召回

2019.7.1(5)
1、ftrl minibatch的开发

2019.6.24(6)
1、qtt：engine内部的降级服务

2019.6.17 (6)
1、萌推：模型加淘宝特征
2、qtt：engine内部的降级服务
3、qtt：预测分开视频和图文

2019.6.10
1. 频道页ctr推全通报
2. 9.9+首页瀑布ctr小流量和推全
3. 以上页面转化率cvr模型小流量
建议支持工作：
1. 道明的活动页ctr+cvr模型
2. 搜索的ctr+cvr模型

2019.6.3(4)
1、萌推：频道页ftrl的ctr预估模型*历史统计cvr的策略全量上线
2、点击到购买的模型训练
3、多场景的数据合并一起训练

2019.5.27(5)
1、点击模型加特征

2019.5.20(5)
1、锁屏fill时清除缓存的逻辑全量反转
2、正排dump文件追加部分文章功能
1、排查训练数据。大约1/10的支付价格小于1。等上报增加价格字段后排除
2、仅用频道点击训练点击模型，今明实验
3、9.9使用支付模型实验：uv价值2.05 -> 2.38 转换率15.8 -> 16.38

2019.5.13(5)
1、9.9和频道页的一些策略迭代
2、模型使用的特征梳理
3、排序模型的训练与调整，频道上排序模型

2019.5.5(7)
1、修改ab接入集团的ab平台
2、增加召回和cvr排序的实验
3、增加9.9频道的接口
4、准备排序模型的代码、资源等

2019.4.28
1、萌推联调 并上线

2019.4.22(5)
1、问题分析的分享
2、萌推的engine; 实现三路召回高热、偏好召回、itemcf召回；和打散去重策略

2019.4.15(5)
1、proxy加接口
2、作者仅对低点击率的调权
3、联合预测增加配置参数控制分值计算

2019.4.10(6)
1、策略：展示未有效曝光的召回（全量）；图集打散（开发中）
2、工程：引擎向召回传递参数 extend；pvmonitor细分
3、分值统计：调权覆盖率统计；sharescore初始化为-1

2019.4.1(4)
1、图集召回实验
2、props的字段添加

2019.3.25(6)
1、图集召回优化
2、dkk支持
3、engine的gc问题

2019.3.18(5)
1、完成最近展示但没有有效展示的召回，并上线
2、原图集的召回去除与优化尝试
3、dkk支持 频控部署
4、引擎gc整理

2019.3.11(6)
1、引擎的优化：
相关的cf召回去除；（实验可能有效果，观察中）
旧频控所有都过滤有效展示；（无效果，实验已停）
cf根据时间加下权；（无效果，实验已停）
2、最近展示但没有有效展示的召回。（开发完成，测试中）
3、315预案支持；看多多机器（运维建ci中）
4、粗排尝试

1、引擎优化：cf根据时间加下权；旧频控所有都过滤有效展示；相关的cf召回去除（吕）
2、对接左俭的去重过滤
3、看多多推荐部署
4、最近展示但没有有效展示的召回
5、引擎优化：排序nothit的处理；高质量的高热补充（陈）
6、315预案


2019.3.4(5)
1、一线ffm召回上线与优化
2、产品：图集的多图清晰度降权；原创非原创作者等级
3、工程上的优化意见收集
4、引擎逻辑整理

2019.2.25(4)
1、完成一线城市ffm召回和ffm粗排的上线和优化
2、产品：图集的多图清晰度降权
3、更新两排序融合的函数
4、地域过滤（建新）

2019.2.18(5)
1、修改引擎和召回的feature的client模式
2、新ffm粗排，引擎召回测修改
3、一线城市ffm召回

2019.2.11(6)
1、一线召回的迁移实验数据基本一致了，准备全量
2、服务发现，测试中，修改部分问题。
3、通知栏，已上线

2019.1.28(5)
http的服务注册与发现的开发，完成自测（周五）
完成一线城市召回的迁移工作（周五）
通知栏请求

2019.1.21(5)
锁屏写频控加机器
debug predict ack add reqid
上视频、封面清晰度实验
锁屏请求增加缓存
一线城市召回全量迁移实体机服务
http的服务注册与发现的开发（本周开发完成50%）

2019.1.14(5)
召回粗排（启文章生成的定时任务）
打点合入engine recall（上一台观察，cpu、内存、gc）
锁屏条数
debug指定predicate
释放旧引擎的机器
给小视频提供视频清晰度
频控的client更新
低清封面打压

2019.1.7(6)周日
Prometheus打点压测完成，一线召回试用(leon_dev_v1分支试用，)
一线召回实体机实验开启
高清视频加权
给预测ack加requestid

2019.1.2(6)
Prometheus打点优化，开发完成60%
一线城市召回服务接口自测完成，待上线
给测试介绍部分引擎逻辑

2018.12.25(5)
nsq多协程消费的功能优化，自测
锁屏曝光写频控的kafka迁移
一线城市召回的接口修改为通用
Prometheus打点库的优化

2018.12.17 (6)
锁屏曝光写频控迁移到data_distributor部署，后续Kafka升级
视频大小限制、清晰度降权实验，发现最新数据不准确问题（余友同步解决）
向predicate发送ab参数，超时300ms，已开发完成
读取时长线的filter的ab参数。
接文章同步的部分工作

2018.12.10 (5)
全量 作者保量扩量到5刷出1、跳过低点击过滤、增加消息频控
为debug平台加召回、过滤的打印信息
一线城市限制类别
视频大小限制
视频清晰度降权

2018.12.3 (7)
业务切分 召回服务
一线城市的内容整改

2018.11.26 (5)
锁屏的召回优化。锁屏使用新内容池
保量与一线的实验开启
给预测返回ack的消息
作者等级数据同步配合调整

2018.11.19 (6)
查部分保量不成功的问题
迁移engine库
优化保量服务，增加降级服务
去除旧ab的调用

2018.11.12 (5)
配合并发召回的测试
优化作者保量，加absdk，加打点。已提测

2018.11.05 (6)
并发select
新版的监控整理，加告警

2018.10.29 (5)
加图集召回，已提测
作者等级调权的问题修复，加监控

2018.10.22（6）
select并发，开发了一版，收集大家建议，继续修改
一线城市配置提出并切docker环境
作者保量fm召回全量
修复filter多过滤的实验
优酷提权去除
带图集的一级偏好，长田帮忙修复pv的数据

2018.10.15(5)
召回docker环境的优化
一级偏好加图集
absdk联调

锁屏加version select并发 filter错误修复

2018.10.8(6)
图集的一级偏好
优质召回优化
敏感内容下移v2版

2018.9.25(6)
作者保量优化使用fm召回模型
一线城市敏感内容下移
小说的广告

2018.9.17(5)
锁屏配合提测、修改优化
一级偏好增加图集（代码修改，还未测试）
ab新版sdk接入，开发完成，提测

2018.9.10(6)
图集召回（部分路的召回）
锁屏召回（代码写完）

2018.9.3(5)
文章支持小时级别的时效
夸张标题打压

2018.8.27(6)
优化保量服务并上线
垂直频道优先高质量

2018.8.20(6)
保量服务的开发

2018.8.13(5)
服务发现模块
web页面的支持、加白名单id
保量服务

2018.8.6(5)
引擎优化，服务发现模块
优质文章（word2vec用mget减少耗时，
加地域召回）
夸张标题根据qukan提供系数打压

2018.7.30(6)
优质文章 ( 加ab参数，
打印打点记录，
补充策略加随机，）待上线（
区分from，
低点击的过滤，）

2018.7.23(5)
优质文章服务优化（加了频控过滤，word2vec）
针对优质文章的作者打散

2018.7.16 (5)
优质内容服务优化
图集等功能联调

2018.7.9 (6)
一线城市优质内容
提供图集的候选测试
全量地区补充企鹅号
频道提供5分钟视频降权

2018.7.2 (5)
新的wordvec
关键词替换es
新的fm预测
相关文章的缓存方法优化
地区频道接ab平台

2018.6.25(6)
优酷调整

2018.6.19 (5)
作者等级上线
优酷在频道补充pv
频道首页调整出人工tag的文章
频道fm排序
召回按from加权


2018.6.11 (5)
频道接口使用abtest平台
文章作者等级加权
优酷pv保量、加召回、召回去除低点击过滤
ftrl增加4个地址
type为0的去除

2018.6.4 （6）
外拉新dtu信息同步，游戏频道实验
优化：去除旧ftrl，旧fm的模型
优酷全量级调整
频道ftrl实验用新ftrl
cf实验


2018.5.28（6）
全量夸张标题
优酷调整实验，统计曝光
整理监控告警
外拉新dtu信息同步
企鹅号


2018.5.21（6）
指定长度的视频权重调整
频道文章企鹅号补量
优酷加权
召回模型对接
外拉新与夸张标题打印的持续调整

2018.5.14 （5）
多层外拉新支持，dtu方案优化、美女分类
热度值不衰减
低俗精准过滤
小程序接口支持
点击不喜欢的降权（等少鹏接口）


2018.5.7（6）
低俗精准过滤
瞭望5个分类的视频过滤实验
短视频降权实验
音乐小于17s替换
deeplink外拉新dtu489，60%流量上线
deeplink美女分类（等数据）
ab实验耗时打点（未上线，考虑其他方式）
ab策略替换优化（测试中）
fm time golang接入


2018.4.29
锁屏功能
小视频候选规则调整
修复超时context冲突bug
渠道新用户启动

放量的实验？

2018.04.23
相关文章接口接入ab（pv有跌，已回退，问题尚未定位）
06组接入关键字查找相关文章接口
瞭望视频补充实验
不能在feed直接播放的视频去除

2018.04.16
合并文件加载（回退）
新的ftrl预测
热度分计算
qupost
主feed视频播放
小视频接口

2018.04.08
视频列表页实验，头条召回
ab平台接入
加强低俗过滤
qupost信息获取

2018.04.02
控制低俗文章数量（朱）
接入召回模型（朱）
abtest接入

2018.03.26
相关文章接口无限，相关文章有效曝光写入频控
ftrl与fm时长模型联合排序上线
视频列表页实验，仅短视频实验
加入部分企鹅号文章实验

2018.03.19
站外相似文章
手动召回部分历史文章
cf时长排序
fm与ftrl联合算分

2018.03.12
abtest联调上线
企鹅号文章的推荐
美拍小视频推荐
按时长文章集合优化，加入用户偏好
文章按可出现的地方分召回等级
新的协同过滤在相关文章上测试

2018.01.14
熟悉kafka订阅系统，kafka数据调度系统
汽车频道的优化
xml配置文件划分优化，增加include标签
频控用golang进行优化

2018.01.19
短视频功能支持
趣投稿 文章是否审核认证
整理上线流程文档
娱乐频道应用新算法(用户召回功能的实现)

2018.01.29
频道的热度优化功能联调
小视频逻辑修改为hot_discuss
增加uid的匹配规则
修改predictor client导致文章集合空的bug
增加read pv条件

2018.02.05
优秀历史文章召回
测试工具：提供select文章接口；使用pcap文件发包

2018.02.11
冷启动，代码实现完成
调整协同过滤与词向量的配比等策略

2018.02.26
冷启动调试，上线
时长的文章候选集

2018.03.09
增加时长文章候选集策略
abtest引擎部分改造