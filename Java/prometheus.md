# prometheus 简介
## 监控目标
### 黑盒监控
主要关注的现象，一般都是正在发生的东西，例如出现一个告警，某文件系统不可写入，那么这种监控就是站在用户的角度能看到的监控，重点在于能对正在发生的故障进行告警。
### 白盒监控
主要关注的是原因，也就是系统内部暴露的一些指标，例如redis的info中显示redis slave down，这个就是redis info显示的一个内部的指标，重点在于原因，可能是在黑盒监控中看到redis down，而查看内部信息的时候，显示redis port is refused connection。
### 长尾效应 The Long Tail Effect
长尾效应则是指在一个分布的尾部，大量的小众项目加起来可以形成一个不容忽视的市场份额，即“多数占少数”的现象
由于个别请求的响应时间需要1秒或者更久，传统的响应时间的平均值就体现不出响应时间中的尖刺了
p95/p99就是长尾效应的分割线，如表示99%的请求在XXX范围内，或者是1%的请求在XXX范围之外。99%是一个范围，意思是99%的请求在某一延迟内，剩下的1%就在延迟之外了。只是正推与逆推而已，是一种概念的两种不同描述。
### 目标
- 长期趋势分析
- 对照分析
- 告警
- 故障分析与定位
- 数据可视化

## 优势
- 易于管理
- 监控服务的内部运行状态
Node Exporter
Prometheus Server并不直接服务监控特定的目标，其主要任务负责数据的收集，存储并且对外提供数据查询支持。因此为了能够能够监控到某些东西，如主机的CPU使用率，我们需要使用到Exporter。Prometheus周期性的从Exporter暴露的HTTP服务地址（通常是/metrics）拉取监控样本数据。
- 强大的数据模型
- 强大的查询语言PromQL
通过PromQL我们可以非常方便的对数据进行查询，过滤，以及聚合，计算等操作。通过这些丰富的表达书语句，监控指标不再是一个单独存在的个体，而是一个个能够表达出正式业务含义的语言。
- 高效
- 可扩展
- 易于集成
- 可视化
- 开放性

## 组件
### Prometheus Server
时间序列数据库
### Exporters
Exporter将监控数据采集的端点通过HTTP服务的形式暴露给Prometheus Server。可分为直接采集和间接采集。
### AlertManager
在Prometheus Server中支持基于PromQL创建告警规则，如果满足PromQL定义的规则，则会产生一条告警，而告警的后续处理流程则由AlertManager进行管理。
### PushGateway
利用PushGateway来进行中转，网络环境隔离的服务。

# 探索PromQL
## Metric类型
### Counter：只增不减的计数器
Counter是一个简单但有强大的工具，例如我们可以在应用程序中记录某些事件发生的次数，通过以时序的形式存储这些数据，我们可以轻松的了解该事件产生速率的变化。常见的监控指标，如http_requests_total，node_cpu都是Counter类型的监控指标。 一般在定义Counter类型指标的名称时推荐使用_total作为后缀。
通过rate()函数获取HTTP请求量的增长率
```
rate(http_requests_total[5m])
```
访问量前10的HTTP地址
```
topk(10, http_requests_total)
```
### Gauge：可增可减的仪表盘
Gauge类型的指标侧重于反应系统的当前状态。因此这类指标的样本数据可增可减。常见指标如：node_memory_MemFree（主机当前空闲的内容大小）、node_memory_MemAvailable（可用内存大小）都是Gauge类型的监控指标。
通过PromQL内置函数delta()可以获取样本在一段时间返回内的变化情况。例如，计算CPU温度在两个小时内的差异
```
delta(cpu_temp_celsius{host="zeus"}[2h])
```
还可以使用deriv()计算样本的线性回归模型，甚至是直接使用predict_linear()对数据的变化趋势进行预测。例如，预测系统磁盘空间在4个小时之后的剩余情况：
```
predict_linear(node_filesystem_free{job="node"}[1h], 4 * 3600)
```
### Histogram和Summary分析数据分布情况
在大多数情况下人们都倾向于使用某些量化指标的平均值，例如CPU的平均使用率、页面的平均响应时间。这种方式的问题很明显，以系统API调用的平均响应时间为例：如果大多数API请求都维持在100ms的响应时间范围内，而个别请求的响应时间需要5s，那么就会导致某些WEB页面的响应时间落到中位数的情况，而这种现象被称为长尾问题。

为了区分是平均的慢还是长尾的慢，最简单的方式就是按照请求延迟的范围进行分组。例如，统计延迟在0~10ms之间的请求数有多少而10~20ms之间的请求数又有多少。通过这种方式可以快速分析系统慢的原因。Histogram和Summary都是为了能够解决这样问题的存在，通过Histogram和Summary类型的监控指标，我们可以快速了解监控样本的分布情况。

例如，指标prometheus_tsdb_wal_fsync_duration_seconds的指标类型为Summary。 它记录了Prometheus Server中wal_fsync处理的处理时间，通过访问Prometheus Server的/metrics地址，可以获取到以下监控样本数据
```
# HELP prometheus_tsdb_wal_fsync_duration_seconds Duration of WAL fsync.
# TYPE prometheus_tsdb_wal_fsync_duration_seconds summary
prometheus_tsdb_wal_fsync_duration_seconds{quantile="0.5"} 0.012352463
prometheus_tsdb_wal_fsync_duration_seconds{quantile="0.9"} 0.014458005
prometheus_tsdb_wal_fsync_duration_seconds{quantile="0.99"} 0.017316173
prometheus_tsdb_wal_fsync_duration_seconds_sum 2.888716127000002
prometheus_tsdb_wal_fsync_duration_seconds_count 216
```

从上面的样本中可以得知当前Prometheus Server进行wal_fsync操作的总次数为216次，耗时2.888716127000002s。其中中位数（quantile=0.5）的耗时为0.012352463，9分位数（quantile=0.9）的耗时为0.014458005s。

与Summary类型的指标相似之处在于Histogram类型的样本同样会反应当前指标的记录的总数(以_count作为后缀)以及其值的总量（以_sum作为后缀）。不同在于Histogram指标直接反应了在不同区间内样本的个数，区间通过标签len进行定义。

同时对于Histogram的指标，我们还可以通过histogram_quantile()函数计算出其值的分位数。不同在于Histogram通过histogram_quantile函数是在服务器端计算的分位数。 而Sumamry的分位数则是直接在客户端计算完成。因此对于分位数的计算而言，Summary在通过PromQL进行查询时有更好的性能表现，而Histogram则会消耗更多的资源。反之对于客户端而言Histogram消耗的资源更少。在选择这两种方式时用户应该按照自己的实际场景进行选择。
## 初识PromQL
### 查询时间序列
PromQL可以支持使用正则表达式作为匹配条件，多个表达式之间使用|进行分离：
- 使用label=~regx表示选择那些标签符合正则表达式定义的时间序列；
- 反之使用label!~regx进行排除；

```
http_requests_total{environment=~"staging|testing|development",method!="GET"}
```
### 范围查询
直接通过类似于PromQL表达式httprequeststotal查询时间序列时，返回值中只会包含该时间序列中的最新的一个样本值，这样的返回结果我们称之为瞬时向量。而相应的这样的表达式称之为__瞬时向量表达式。
而如果我们想过去一段时间范围内的样本数据时，我们则需要使用区间向量表达式。区间向量表达式和瞬时向量表达式之间的差异在于在区间向量表达式中我们需要定义时间选择的范围，时间范围通过时间范围选择器[]进行定义。例如，通过以下表达式可以选择最近5分钟内的所有样本数据：
```
http_request_total{}[5m]
```
### 时间位移操作
如果我们想查询，5分钟前的瞬时样本数据，或昨天一天的区间内的样本数据呢? 这个时候我们就可以使用位移操作，位移操作的关键字为offset
```
http_request_total{} offset 5m
http_request_total{}[1d] offset 1d
```
### 使用聚合操作
sum avg 使用 by 来 分组
### 标量和字符串
#### 标量
当使用表达式count(http_requests_total)，返回的数据类型，依然是瞬时向量。用户可以通过内置函数scalar()将单个瞬时向量转换为标量。
## PromQL操作符
### 数学运算
例如，我们可以通过指标node_memory_free_bytes_total获取当前主机可用的内存空间大小，其样本单位为Bytes。这是如果客户端要求使用MB作为单位响应数据，那只需要将查询到的时间序列的样本值进行单位换算即可：
```
node_memory_free_bytes_total / (1024 * 1024)
```
node_memory_free_bytes_total表达式会查询出所有满足表达式条件的时间序列，在上一小节中我们称该表达式为瞬时向量表达式，而返回的结果成为瞬时向量。
当瞬时向量与标量之间进行数学运算时，数学运算符会依次作用域瞬时向量中的每一个样本值，从而得到一组新的时间序列。

而如果是瞬时向量与瞬时向量之间进行数学运算时，过程会相对复杂一点。 例如，如果我们想根据node_disk_bytes_written和node_disk_bytes_read获取主机磁盘IO的总量，可以使用如下表达式：
```
node_disk_bytes_written + node_disk_bytes_read
```
那这个表达式是如何工作的呢？依次找到与左边向量元素匹配（标签完全一致）的右边向量元素进行运算，如果没找到匹配元素，则直接丢弃。同时新的时间序列将不会包含指标名称。
### 使用布尔运算过滤时间序列
系统管理员在排查问题的时候可能只想知道当前内存使用率超过95%的主机呢？通过使用布尔运算符可以方便的获取到该结果：
```
(node_memory_bytes_total - node_memory_free_bytes_total) / node_memory_bytes_total > 0.95
```
### 使用bool修饰符改变布尔运算符的行为
只需要知道当前模块的HTTP请求量是否>=1000，如果大于等于1000则返回1（true）否则返回0（false）。这时可以使用bool修饰符改变布尔运算的默认行为。 例如：
```
http_requests_total > bool 1000
```
如果是在两个标量之间使用布尔运算，则必须使用bool修饰符
```
2 == bool 2 # 结果为1
```
### 使用集合运算符
and or unless(排除)
### 操作符优先级
- ^
- *, /, %
- +, -
- ==, !=, <=, <, >=, >
- and, unless
- or

### 匹配模式详解
#### 一对一匹配
一对一匹配模式会从操作符两边表达式获取的瞬时向量依次比较并找到唯一匹配(标签完全一致)的样本值。
在操作符两边表达式标签不一致的情况下，可以使用on(label list)或者ignoring(label list）来修改便签的匹配行为。使用ignoreing可以在匹配时忽略某些便签。而on则用于将匹配行为限定在某些便签之内。
```
<vector expr> <bin-op> ignoring(<label list>) <vector expr>
<vector expr> <bin-op> on(<label list>) <vector expr>
```
例如当存在样本：
```
method_code:http_errors:rate5m{method="get", code="500"}  24
method_code:http_errors:rate5m{method="get", code="404"}  30
method_code:http_errors:rate5m{method="put", code="501"}  3
method_code:http_errors:rate5m{method="post", code="500"} 6
method_code:http_errors:rate5m{method="post", code="404"} 21

method:http_requests:rate5m{method="get"}  600
method:http_requests:rate5m{method="del"}  34
method:http_requests:rate5m{method="post"} 120
```
使用PromQL表达式：
```
method_code:http_errors:rate5m{code="500"} / ignoring(code) method:http_requests:rate5m
``` 
该表达式会返回在过去5分钟内，HTTP请求状态码为500的在所有请求中的比例。如果没有使用ignoring(code)，操作符两边表达式返回的瞬时向量中将找不到任何一个标签完全相同的匹配项。
因此结果如下：
```
{method="get"}  0.04            //  24 / 600
{method="post"} 0.05            //   6 / 120
```
#### 多对一和一对多
多对一和一对多两种匹配模式指的是“一”侧的每一个向量元素可以与"多"侧的多个元素匹配的情况。在这种情况下，必须使用group修饰符：group_left或者group_right来确定哪一个向量具有更高的基数（充当“多”的角色）。
```
<vector expr> <bin-op> ignoring(<label list>) group_left(<label list>) <vector expr>
<vector expr> <bin-op> ignoring(<label list>) group_right(<label list>) <vector expr>
<vector expr> <bin-op> on(<label list>) group_left(<label list>) <vector expr>
<vector expr> <bin-op> on(<label list>) group_right(<label list>) <vector expr>
```
多对一和一对多两种模式一定是出现在操作符两侧表达式返回的向量标签不一致的情况。因此需要使用ignoring和on修饰符来排除或者限定匹配的标签列表。
例如,使用表达式
```
method_code:http_errors:rate5m / ignoring(code) group_left method:http_requests:rate5m
```

该表达式中，左向量method_code:http_errors:rate5m包含两个标签method和code。而右向量method:http_requests:rate5m中只包含一个标签method，因此匹配时需要使用ignoring限定匹配的标签为code。 在限定匹配标签后，右向量中的元素可能匹配到多个左向量中的元素 因此该表达式的匹配模式为多对一，需要使用group修饰符group_left指定左向量具有更好的基数。
最终的运算结果如下：
```
{method="get", code="500"}  0.04            //  24 / 600
{method="get", code="404"}  0.05            //  30 / 600
{method="post", code="500"} 0.05            //   6 / 120
{method="post", code="404"} 0.175           //  21 / 120
```
提醒：group修饰符只能在比较和数学运算符中使用。在逻辑运算and,unless和or才注意操作中默认与右向量中的所有元素进行匹配。
## PromQL聚合操作
- sum (求和)
- min (最小值)
- max (最大值)
- avg (平均值)
- stddev (标准差)
- stdvar (标准差异)
- count (计数)
- count_values (对value进行计数)
- bottomk (后n条时序)
- topk (前n条时序)
- quantile (分布统计)
without用于从计算结果中移除列举的标签，而保留其它标签。by则正好相反，结果向量中只保留列出的标签，其余标签则移除。通过without和by可以按照样本的问题对数据进行聚合。

## PromQL内置函数
### 计算Counter指标增长率
increase(v range-vector)函数是PromQL中提供的众多内置函数之一。其中参数v是一个区间向量，increase函数获取区间向量中的第一个后最后一个样本并返回其增长量。因此，可以通过以下表达式Counter类型指标的增长率：
```
increase(node_cpu[2m]) / 120
```
这里通过node_cpu[2m]获取时间序列最近两分钟的所有样本，increase计算出最近两分钟的增长量，最后除以时间120秒得到node_cpu样本在最近两分钟的平均增长率。并且这个值也近似于主机节点最近两分钟内的平均CPU使用率。

除了使用increase函数以外，PromQL中还直接内置了rate(v range-vector)函数，rate函数可以直接计算区间向量v在时间窗口内平均增长速率。因此，通过以下表达式可以得到与increase函数相同的结果：
```
rate(node_cpu[2m])
```
需要注意的是使用rate或者increase函数去计算样本的平均增长速率，容易陷入“长尾问题”当中，其无法反应在时间窗口内样本数据的突发变化。 
为了解决该问题，PromQL提供了另外一个灵敏度更高的函数irate(v range-vector)。irate同样用于计算区间向量的计算率，但是其反应出的是瞬时增长率。irate函数是通过区间向量中最后两个两本数据来计算区间向量的增长速率。这种方式可以避免在时间窗口范围内的“长尾问题”，并且体现出更好的灵敏度，通过irate函数绘制的图标能够更好的反应样本数据的瞬时变化状态。
```
irate(node_cpu[2m])
```
irate函数相比于rate函数提供了更高的灵敏度，不过当需要分析长期趋势或者在告警规则中，irate的这种灵敏度反而容易造成干扰。因此在长期趋势分析或者告警中更推荐使用rate函数。
### 预测Gauge指标变化趋势
PromQL中内置的predict_linear(v range-vector, t scalar) 函数可以帮助系统管理员更好的处理此类情况，predict_linear函数可以预测时间序列v在t秒后的值。它基于简单线性回归的方式，对时间窗口内的样本数据进行统计，从而可以对时间序列的变化趋势做出预测。例如，基于2小时的样本数据，来预测主机可用磁盘空间的是否在4个小时候被占满，可以使用如下表达式：
```
predict_linear(node_filesystem_free{job="node"}[2h], 4 * 3600) < 0
```
### 统计Histogram指标的分位数
而Histogram的分位数计算需要通过histogram_quantile(φ float, b instant-vector)函数进行计算。其中φ（0<φ<1）表示需要计算的分位数，如果需要计算中位数φ取值为0.5，以此类推即可。
需要注意的是通过histogram_quantile计算的分位数，并非为精确值，而是通过http_request_duration_seconds_bucket和http_request_duration_seconds_sum近似计算的结果。
### 动态标签替换
这是可视化工具渲染图标时可能根据，instance和job的值进行渲染，为了能够让客户端的图标更具有可读性，可以通过label_replace标签为时间序列添加额外的标签。label_replace的具体参数如下:
```
label_replace(v instant-vector, dst_label string, replacement string, src_label string, regex string)
```
该函数会依次对v中的每一条时间序列进行处理，通过regex匹配src_label的值，并将匹配部分relacement写入到dst_label标签中。如下所示：
```
label_replace(up, "host", "$1", "instance",  "(.*):.*")
```
函数处理后，时间序列将包含一个host标签，host标签的值为Exporter实例的IP地址：
```
up{host="localhost",instance="localhost:8080",job="cadvisor"}    1
up{host="localhost",instance="localhost:9090",job="prometheus"}    1
up{host="localhost",instance="localhost:9100",job="node"} 1
```
除了label_replace以外，Prometheus还提供了label_join函数，该函数可以将时间序列中v多个标签src_label的值，通过separator作为连接符写入到一个新的标签dst_label中:
```
label_replace和label_join函数提供了对时间序列标签的自定义能力，从而能够更好的于客户端或者可视化工具配合。
```
## 最佳实践：4个黄金指标和USE方法
### 4个黄金指标
#### 延迟：服务请求所需时间
重点是要区分成功请求的延迟时间和失败请求的延迟时间
#### 通讯量：监控当前系统的流量，用于衡量服务的容量需求
流量对于不同类型的系统而言可能代表不同的含义
#### 错误：监控当前系统所有发生的错误请求，衡量当前系统错误发生的速率
#### 饱和度：衡量当前服务的饱和度
主要强调最能影响服务状态的受限制的资源。
### RED方法
#### (请求)速率：服务每秒接收的请求数。
#### (请求)错误：每秒失败的请求数。
#### (请求)耗时：每个请求的耗时。
### USE方法
#### 使用率(Utilization)
#### 饱和度(Saturation)
#### 错误(Errors)

# 数据与可视化
## Grafana与数据可视化
### 变化趋势：Graph面板
Graph面板通过折线图或者柱状图的形式，能够展示监控样本数据在一段时间内的变化趋势，因此其天生适合Prometheus中的Counter和Gauge类型的监控指标的可视化，对于Histogram类型的指标也可以支持，不过可视化效果不如Heatmap Panel来的直观。
#### 使用Graph面板可视化Counter/Gauge
Graph面板会从时间序列中获取样本数据，并绘制到图表中。 为了让折线图有更好的可读性，我们可以通过定义Legend format为{{ instance }}控制每条线的图例名称
由于当前使用的PromQL的数据范围为0~1表示CPU的使用率，为了能够更有效的表达出度量单位的概念，我们需要对Graph图表的坐标轴显示进行优化。如下所示，在Axes选项中可以控制图标的X轴和Y轴相关的行为
##### Options中可以设置图例的显示方式以及展示位置，Values中可以设置是否显示当前时间序列的最小值，平均值等。 Decimals用于配置这些值显示时保留的小数位
除了以上设置以外，我们可能还需要对图表进行一些更高级的定制化，以便能够更直观的从可视化图表中获取信息。在Graph面板中Display选项可以帮助我们实现更多的可视化定制的能力，其中包含三个部分：Draw options、Series overrides和Thresholds
- Draw Options用于设置当前图标的展示形式、样式以及交互提示行为。其中，Draw Modes用于控制图形展示形式：Bar（柱状）、Lines（线条）、Points（点），用户可以根据自己的需求同时启用多种模式
- Threshold主要用于一些自定义一些样本的阈值，例如，定义一个Threshold规则，如果CPU超过50%的区域显示为warning状态

#### 使用Graph面板可视化Histogram
需要在Query Editor中定义查询结果的Format as为Heatmap

### 分布统计：Heatmap面板
Heatmap是是Grafana v4.3版本以后新添加的可视化面板，通过热图可以直观的查看样本的分布情况。
#### 使用Heatmap可视化Histogram样本分布情况
对于Histogram类型的监控指标来说，更好的选择是采用Heatmap Panel，如下所示，Heatmap Panel可以自动对Histogram类型的监控指标分布情况进行计划，获取到每个区间范围内的样本个数，并且以颜色的深浅来表示当前区间内样本个数的大小。而图形的高度，则反映出当前时间点，样本分布的离散程度。
在Grafana中使用Heatmap Panel也非常简单，在Dashboard页面右上角菜单中点击“add panel”按钮，并选择Heatmap Panel即可。
当使用Heatmap可视化Histogram类型的监控指标时，需要设置Format as选项为Heatmap。
我们需要在Axes选项中定义数据的Date format需要定义为Time series buckets。
#### 使用Heatmap可视化其它类型样本分布情况
对于非Histogram类型，由于其监控样本中并不包含Bucket相关信息，因此在Metrics选项中需要定义Format as为Time series，
通过Axes选项中选择Data format方式为Time series

### 当前状态：SingleStat面板
Singlem Panel侧重于展示系统的当前状态而非变化趋势。如下所示，在以下场景中特别适用于使用SingleStat：
- 当前系统中所有服务的运行状态
- 当前基础设施资源的使用量
- 当前系统中某些事件发生的次数或者资源数量等












