# 更多，更快，更好，更省，怎么设计一个好用的A/B测试系统？
相信很多人听说过A/B测试，但是受限于各种条件真正实践过的可能并不多，A/B测试目前还局限于小范围尝试。最近核心工作就是设计一款好用、易用的A/B测试产品，通过产品化推动A/B测试大规模应用。基于这个主题，接下来我将介绍我们A/B测试系统设计的出发点，思考以及产品设计。

## 经典实践
简单来说，A/B测试就是为同一个目标制定两个方案（比如两个页面），让一部分用户使用 A 方案，另一部分用户使用 B 方案，记录下用户的使用情况，根据不同方案的结果数据（例如：下单）来确定方案的优劣。我举两个大家都听说的例子作为开篇。

案例一，奥巴马的竞选网站，实验的主要内容通过A/B测试优化注册页面，提高注册转化率：

![Obama捐款网站](../myReplay/obama.png)
希望通过测试，确定最优的背景图片和注册文案，实验方案总共16种组合方式，最终选出右侧版本:

1. 家庭图片好于个人图片
2. LEARNMORE(了解更多）好于SIGNUP（注册）

该方案相对原始版本提升了40.6%的注册转化率，直接经济价值几千万美元。特别需要说明的是2008年参与奥巴马竞选的团队后续创建了optimzely网站，专业提供A/B测试服务，并且在2015年实现C轮5800万美元融资。后续2012年奥巴马再次竞选时，竞选团队就通过optimizely进行网站优化，据统计竞选期间共进行了240次A/B测试。

案例二，Facebook改版，产品希望通过新的的设计提升用户体验：

<img src="../myReplay/facebook.png" width="100%" align=center />

新版本突出图片的内容，并且将左侧内容收起，更加美观，效果也更加酷炫。但不幸的是新版本相对老版本用户访问时长降低百分之三十，最终回退到老版本。

传统数据分析设计一般是后验性的，也就是先把产品做出来投放市场，然后通过数据验证实验效果。对于体量比较大的产品，这种工作流程带来的错误成本会非常高。相对来说，A/B测试通过小流量测试，多变量组合实验等方法可以完美解决上述问题。

## 使用场景
通过上边两个案例，我们对A/B测试的作用可以归纳为：

1. 求最优解：我有很多种方案，希望确定哪个方案最优
2. 证伪：我有一个新的想法，希望通过数据验证合理性

那么问题来了，A/B测试可以做哪些东西呢？简单来说，所有东西都可以测。更恰当的问题应该是，哪些场景更适合A/B测试？

1. 成本相对可控，既包括开发成本，也包括用户使用成本
2. 流量相对充足，既包括时间上的纵向流量，也包括规模上的横向流量

可以发现符合以上条件的主要是线上业务，包括互联网产品以及传统业务的线上部分，线下的成本相对来说更加高昂。具体对于线上业务来说，这些内容是我们关注的重点：

1. 市场同学：测试推广素材，落地页，CTA文案，注册流程等等
2. 运营同学：测试标题，排版，定价，邀请流程等等
3. 产品同学：测试产品功能，页面设计，交互流程，引导操作等等
4. 开发同学：测试算法，技术方案等等

就重要程度来说，A/B测试能够快速起作用的流程如：

1. 直接跟钱相关的环节，比方说广告投放，产品落地页，购买流程
2. 涉及到经常变动的流程，典型如运营方案，内容营销等

## 产品结构
那么对于互联网服务来说，A/B测试主要包括哪些模块呢？我用下边这个图来对A/B测试系统进行介绍：

<img src="../myReplay/structure.png" width="60%" align=center />

从产品结构上来看，完成一个A/B测试至少需要有三个模块：

1. 用户分流：通过某一个机制将用户分配到不同版本中，属于A/B测试核心环节
2. 产品功能：通过工程方式将不同设计，文案，流程等在产品中实现
3. 效果评估：计算不同实验版本的优化指标，确定最终实验效果。这个过程中需要计算相关的统计变量，评估数据以及抽样的带来的实验偏差。

#### A/B测试系统需要的开发资源
从参与方来看，完成一次A/B测试至少需要以下这些角色：

1. 后端提供分流模块，将用户分配到不同版本中
2. 移动端或者前端负责开发不同版本，部分情况A/B测试需要做在后端
3. 数据分析师确定优化指标，计算逻辑
4. 数据开发负责数据采集，加工以及最后的结果计算

## 大规模实践的障碍
A/B测试是提高推广效率，优化运营策略，改进产品设计的必备利器，那么这个工具的应用情况到底怎么样呢？经过我自己的调研，发现A/B测试目前并没有大规模体系化应用，目前还仅限于小规模测试阶段。

#### 问题1，开发资源不足 
虽然结构看起来相对简单，但是一个稳定并且可用性很高的A/B测试系统仍然需要很多工作。在开发排期相对紧张的团队，产品提出A/B测试时要做好被拍死的准备（高危职业啊）。最简单的方式是尽量避免A/B测试，除非改动会对营收或者UV有很大影响。这也是A/B测试大规模普及的主要阻力，也是我们想要解决的核心问题。真实业务场景中，很多A/B测试系统会把部分功能进行简化

1. 移动应用开发过程中，很多产品将新版单独打包发到某一个渠道进行小流量测试，这种A/B测试方法只需要移动端开发参与就好。但是问题非常明显：
	1. 这种方式仅限于安卓端测试，局限性非常明显。同时，这种工作方式显然不是通用的解决方案，会造成版本混乱
	2. 最重要的问题在于实验的科学性上，横向来看不同渠道差异明显，纵向来看不同时段的用户差异明显，所以数据不具有普适性。
	3. 还有一个隐藏问题，安装包发出去之后无法回撤，给用户体验造成的影响是不可逆的
2. 很多业务方通过CRM或者其他后台做用户流量分配，再通过分析师做效果评估。这种方式灵活性相对较高，但是仍然有很大问题：
	1. 最关键的是这些后台不是做这些事情的，所以肯定不会为A/B测试专门设计。举例如比例选择，流量放大，实验停止等等
	2. A/B测试需要相同实验的用户属性尽可能相似，而这种分流方式在科学性上有很大问题，也不利于后续数据分析

#### 问题2，效果评估阶段数据整合
每一个实验都有个确定的实验目的，需要有相应的数据指标来评价实验效果，一般来说数据主要来源于以下这两个部分：

1. 按钮点击，页面浏览，下单等埋点信息
2. 销售额，支付金额，注册用户数等等业务信息 

埋点数据的来源有可能是iOS、Android、JS、服务端等等各种来源，业务端数据来源于业务数据库。将这些数据进行处理、整合、计算需要专业的数据开发深度参与。

#### 问题3，A/B测试灵活度不足
假设A/B测试系统已经正式开启，仍然会有很多难题需要我们解决。其中最主要矛盾在于无限的实验想法受限于固定的实验开发周期，尤其是App的开发流程：

1. 在产品已经发布的的情况下，如何在调整测试内容时不需要二次开发
2. 在确定实验效果已经达到预期的情况下，如何实时调节流量
3. 在实验想法很多的情况下，会不会存在流量不足

#### 总结
要解决这些难题，需要在系统的架构与底层设计上对产品进行改进。但是产品方资源集中在业务发展，对于A/B测试这种底层平台的价值没有达成共识，或者说开发成本过高。再有，我们现在产品的思维是迭代思维，对于测试这种事情没有充分认识到它的价值。以上介绍的内容是我们希望解决的问题：通过搭建底层平台，降低业务方使用A/B测试的成本；通过推广A/B测试平台，推广新的实验思维。

## 我们的探索与实践
还需要说明的是，目前零散的A/B测试系统还有个特别大的弊端，零散式测试无法形成产品的公共知识库，前期测试无法形成公共经验，也不利于后续产品的持续优化。基于以上场景以及问题，我们对现有问题进行梳理汇总之后，设计出现有A/B测试产品：

<img src="../myReplay/expStructure.png" width="60%" align=center />

#### 通过产品化降低开发成本
我们将公共服务抽象之后进行产品化，然后将这些产品模块整合成统一的产品。接入这个系统后各个产品方实验时，仅需要关注业务开发。具体来说:

1. 分流服务模块：提供基本的分流服务与运行控制功能。为了有效利用流量和精准测试，引入分层，定向，黑白名单等高级功能
2. 产品功能模块：产品方需要开发相应的实验版本，并且通过SDK与我们后台进行交互。为了减少产品方开发成本，我们正在探索可视化实验编辑
3. 效果评估模块：将常用的分析模型整合进产品分析模块，用户仅需要确定数据指标。为了实验的科学性，我们会提供各种统计指标以计算抽样带来的偏差
4. 其他工具，帮助用户预览实验效果，测试数据准确性等等

通过将效果评估，数据采集，分流服务，调试工具等模块进行抽象并且产品化，我们可以做到将A/B测试成本降为原来的十分之一：

1. 产品进行A/B测试时仅需要专注于具体业务的工程实现，而不需要关注其他功能
2. 我们将常用数据分析模型进行抽象，数据团队可以将注意力从数据采集加工转移到业务机会发现上
3. 通过调试等第三方工具的开放，用户A/B测试的效率可以进一步提升

#### 打通埋点数据，简化数据处理流程
比较复杂的问题是数据采集、加工与计算，我们目前采用的方案是将埋点数据与实验数据打通，通过埋点的行为分析模型结合实验版本数据对A/B测试进行分析。这种方案的优点是A/B测试与行为分析共用同一套数据采集与处理平台，极大降低接入成本与开发成本。但是我们还有一些问题没有解决：

1. 部分A/B测试的目的是希望提高销售额等业务指标，这些的指标只存在业务系统。另外部分数据指标我们产品模型暂不支持
2. 服务端与前端数据打通，毕竟我们很多业务逻辑需要在服务端实现

当然这个问题相对复杂，展开可以单独写一篇文章，所以我仅介绍我们的思路以及面对问题。

#### 引入最新的产品架构实现A/B测试的灵活性
产品的灵活性主要体现在两个层面：实验内容的灵活调整以及实验流量的实时调节。实验流量的实时调节，涉及到比较多的技术问题，此处略过。实验内容的灵活调整由简单到复杂可以分为两个层面：

1. 实验主题已经确定，比方说obama竞选的示例，后续主要是具体内容的调整（比方说文案内容由`注册`改为`加入我们``了解更多``免费注册`），这种场景主要是A/B测试经过需求评审，并且也已经开发上线，调整仅涉及具体内容
2. 实验主题未定，比方说临时决定调整一个详情页的商品图片。这种场景主要是A/B测试没有经过需求评审，属于版本中间临时需求。

我们引入变量的概念解决实验灵活性的问题，每一个实验对应若干变量。每一个实验版本对应不同变量的组合值，这些值由用户输入。如果主题已经确定，后续用户只需要对变量值进行调整，不需要二次开发。

<img src="../myReplay/variable.jpg" width="60%" align=center />

进一步思考如果不需要产品方开发参与，我本身就可以拿到产品的全部变量，是不是意味着不需要提前定义，仅在需要的时候插入代码对变量进行实验。这也就是上边提到过的可视化实验，但是篇幅有限我不会做详细说明，感兴趣的出门左转可以看我们前端（汪洋）的文章：[web在线页面编辑实现-abtest可视化实验](http://ks.netease.com/blog?id=11516)

#### 引入分层概念，帮助产品方最优化流量分配
我们引入分层概念，帮助产品方解决流量不足的难题。用户分层首先由谷歌提出并实现，后续渐渐被大家采纳，我们在产品设计之初也引入分层的概念。简单来说分层可以认为将用户复制，从而扩大流量规模以支撑更多实验。

<img src="../myReplay/Layer1.png" width="60%" align=center />

通过分层的设计，我们在流量的分配中可以达到最大的灵活性：

1. 如果实验之间互相影响，将不同实验放在同一分层，从而保证实验的科学性
2. 如果实验之间互相独立，将不同实验放在不同分层，从而保证实验的流量充足

## 写在最后
我前边仅仅介绍了我们大致的框架与设计，不同主题我后续还将深入分析，我们的探索跟实践还在继续。开发一个好用并且易用的A/B测试系统是我们的初衷，希望通过这个产品帮助所有产品实现数据驱动、实验驱动。那么问题来了:

1. 这么好用的系统，我现在可以接入吗？
2. 如果使用的话，你们有技术支持吗?

答案必须是肯定的，同时欢迎大家跟我们做技术以及产品交流。如果大家想了解的话也可以访问[我们的官网：HubbleData](https://hubble.netease.com/sl/aaaas7)



