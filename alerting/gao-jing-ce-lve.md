在 OpsMind，告警策略的运作是基于监控指标项的查询。本质上就是 **通过查询您配置的指标项来检查数据是否符合预期**，因此告警策略的配置非常灵活。

#### 指标项维度

在配置一个策略之前，我们一起看下指标项的维度对告警的影响：

一个指标项的查询原理非常简单，如下图所示：

![](/assets/metric_query_flow.png)

我们以 _服务器 CPU 使用率_ 这个指标项为例：

![](/assets/metric_query_sample_flow.png)

可以看到，对于一个指标项的查询来说，输入的条件维度与输出的结果维度并不一定相同，因为我们可以对第一步的查询结果进行维度聚合，这样会导致被聚合的维度在最终的结果中消失。

对于告警来说，指标项的查询结果会用来匹配符合的 **告警条件**，因此若指标项的某一维度存在于结果维度中，那么称之为告警的 **作用范围**。若某一维度在指标项查询中被聚合掉了，那么它就归类到告警的 **触发规则** 中，因为它不会影响 **告警条件** 的匹配，它只能作为该告警配置的一个约束条件（同阈值一样）。

#### 告警策略、告警条件与触发规则

一个 **告警策略** 由以下两点组成：

1. 基于的监控指标项（及其聚合方式）
2. 一个或多个 **告警条件**

一个 **告警条件** 中包含：

1. 告警作用范围（通过指标项维度指定）
2. 一个或多个 **触发规则**

同一个告警策略中，多个告警条件之间通过各自不同的作用范围来确定一个具体的告警对象应该受到哪个告警条件的约束，例如下图中的示例：

![](/assets/alert_scope.png)

这四个作用范围中，若多个作用范围之间存在重合部分（例如 ACD），那么这部分的触发条件就按照范围最小的告警条件（也就是 D）进行告警。

而一个 **触发规则** 则描述了告警具体的触发条件，其中包含：

1. 指标项的条件维度（可能没有，取决于指标项是否有维度被聚合）
2. 运算符、阈值、持续时间、告警级别等

一个相对完整的使用案例如下：

![](/assets/policy-adjust-trigger.png)


#### 最佳实践

通常一个监控指标项只需配置一个 **告警策略** 就能满足大部分告警需求。


