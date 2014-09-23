---
layout:post
title:beanstalkd相关
category:experience
---
#beanstalkd的核心要素#
* job
一个需要异步处理的任务，是队列的基本单元。需要存放在一个tube中（tube我也不知道是什么东西，就当成对应的管道或者队列窗口吧，比如售票大厅里面，你要买票就去买票的窗口，你要退票就去退票的窗口）

* tube
一个有名字的任务队列，用来存储统一类型的job，是producer和consumer操作的对象。

* producer生产者
job的生产者，将数据或job put到对应的tube里面。

* consumer消费者
Job的消费者，通过reserve（预定任务）/release（释放任务让别人处理）/bury（埋葬、保留任务）/delete（删除任务）命令来获取job或改变job的状态。

beanstalkd有五种状态：

![](http://writehappy.qiniudn.com/img/beanstalkd.jpg)

	READY(*) - 需要立即处理的任务，当延时 (DELAYED) 任务到期后会自动成为当前任务；
	DELAYED(*) - 延迟执行的任务, 当消费者处理任务后, 可以用将消息再次放回 DELAYED 队列延迟执行；
	RESERVED - 已经被消费者获取, 正在执行的任务。Beanstalkd 负责检查任务是否在 TTR(time-to-run) 内完成；
	BURIED - 保留的任务: 任务不会被执行，也不会消失，除非有人把它 "踢" 回队列；
	DELETED - 消息被彻底删除。Beanstalkd 不再维持这些消息

```生产者 - use / put```

生产者用 use 选择一个管道 (tube), 然后用 put 命令向管道发布任务 (job).以下是曾经定义过的job格式

	etype "业务类型"
	aid 数字
	mac 字符串
	idfa 字符串
	client  数字
	crypt true/false
	idfv 字符串
	t 时间戳 	

```消费类 - watch / reserve / delete / release / bury / touch```

消费者用 watch 选择多个管道 (tube), 然后用 reserve 命令获取待执行的任务，这个命令是阻塞的。客户端直到有任务可执行才返回。当任务处理完毕后, 消费者可以彻底删除任务 (DELETE), 释放任务让别人处理 (RELEASE), 或者保留 (BURY) 任务。

Beanstalkd 不足:

Beanstalkd 没有提供主备同步 + 故障切换机制, 在应用中有成为单点的风险。实际应用中，可以用数据库为任务 (job) 提供持久化存储。

![](http://writehappy.qiniudn.com/img/beanstalkd_progress.jpg)

另外, 和 memcached 类似, Beanstalkd 依赖 libevent 的单线程事件分发机制, 不能有效利用多核 cpu 的性能。这一点可以通过单机部署多个实例克服。