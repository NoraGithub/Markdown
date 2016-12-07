---
title: STAR of Projects
date: 2016-11-15 20:57:53
tags:
type: about
---
# Summary
偏好数据技术&后端技术技术栈积累，希望充当一个能做产品需求分析、设计&项目管理的Program Manager。
本文档的主要是对每个经手项目做详细的S（Situation）T（Task）A（Action）R（Result）分析
<!-- more -->
# 舜飞科技-DSP产品经理
先后向CEO/产品负责人（创始人之一）汇报。
偏后端项目管理，偏Scrum Master。数据驱动衡量需求价值并协调前端、后端和测试资源保证项目顺利上线。协助流量变现，移动端广告投放占比增长到50%以上。
1. 构建产品部文档和项目管理制度；
2. 负责渠道/媒介对接，衡量渠道价值，安排优先级；
3. 重点客户/项目跟进（技术/英文/复杂等）；
2016年，舜飞的战略方向有两个，一个是大的品牌广告主（展示类广告是广告市场大头，效果类广告欠积累），一个是海外的资源拓展（国内增量市场）。
其中，品牌广告主主要来自4A公司。这类需求有几个特点：
- 需求不确定/变更频繁
- 对技术需求没有概念且复杂、个性化
- 预算大，时间长
- 对概念（ 一般由第三方技术对接）的要求高。

其中，“高通”是舜飞2016年最大的品牌广告主，同时高通的成功投放也是舜飞“品牌广告主”战略至关重要的一步。

## “计划”一期设计&测试
## BES_AMS服务对接
### Situation
主流的人群标签以及第三方数据主要是基于访客的浏览数据进行建模，对于toB类广告主，**这类人群标签显然不够精准**。BES是百度最大的ADX。AMS（Audience Matching Service）服务是百度关键词服务，通过API返回固定数量关键词对应的搜索人群（cookie），用于RTB过程中的(re)targeting。
另一方面，这个功能对于品牌广告主市场而言有强烈的吸引性，属于稀缺资源。渠道协议& 文档已经到位，客户有强烈需求。

### Task
按时上线，保证服务正常。

### Action
甄别背景信息&设计产品、跟进进度&撰写文档、保证测试
- 探索&明确产品需求和商务紧急性
    - 显性需求：
        - 对接
    - 隐性需求：
        - 跨渠道支持关键词人群（二期，与技术成本相关）
        - 运营理解并容易使用
    - 需要考虑的问题：
        - 广告主/DSP无法验证
        - 可能无法投出量
        - 可能无效果
- 和渠道沟通，确认
    - 确认后端数据交换流程、协议
    - 接口测试（联调）
    - 确认前端界面开发方案
- 测试
    - 对比日志中请求数据与出价数据保证出价比例
- 控制商务期望
- 确认数据口径，撰写使用文档，简单明了阐述清楚原理和使用方法，降低使用成本。包括使用规则和cookie周期等可能会遇到的问题。

遭遇两个方案：
- 实时请求方案（基于BiddingX、BES、三方ADX共同成功mapping的cookies）
理想模型：

```seq
BiddingX->BES: words
BES->BiddingX: tag_id
```
```seq
All ADXs->BiddingX: Bid Request
BiddingX->BES: AMS Request
BES->BiddingX:tag_id(s)
BiddingX->All ADXs: Bid Response
```

- 离线方案
实际模型（考虑服务器资源和120ms内的返回竞价回复的实际情况）：

```seq
BiddingX->BES: words
BES->BiddingX: tag_id
```
**曝光cookie mapping+cookie mapping 托管**
```seq
Other ADXs->BiddingX: Bid Request
BiddingX->BiddingX:self-query
BiddingX->Other ADXs: Bid Response
```

### Result
按时上线，服务SAP、IBM广告主。

##  The_Media_Trust对接
### Situation
进行海外投放过程中，像Imobi/Google/Mopub/Pubmatic渠道，有严格的创意&政策要求。The Media Trust是保证创意、落地页无恶意代码以及政策违反的第三方服务机构，通过定时轮询，**保证投放免受政策惩罚**。
由于代理商广告创意包含恶意代码，BiddingX被Google惩罚，禁止投放。台湾、香港以及东南亚、国外广告主受严重影响，需要紧急上线该服务保证创意质量。

### Task
- 通过对接第三方服务、和Google沟通等恢复海外资源投放
- 确认Google投放要求，避免类似情况出现

### Action
- 和Google Tech Team&Business Team沟通确认specific issue（创意/落地页）
    - 确认清楚被惩罚的具体原因（严重沟通漏斗）
    - 结合实际考虑Google的对接建议
- 对接保证创意安全
    - 确认最保守的轮询策略（Google有最后通牒：永久禁投）
    - 合适的提醒方案（不打扰目前创意送审流程同时出问题可以及时发现、修改和沟通：送审轮询得到恶意代码后提醒）
- 测试
    - review出问题创意是否及时阻止
    - case by case 跟进所有The Media Trust警告邮件
    - 确认被默认停止投放的广告具体原因

### Result
Google恢复正常投放，顺利保证The Media Trust服务。

##  Peer_39/ADbug_brandsafety（品牌安全）对接

### Situation
品牌类广告主对于品牌美誉度有要求，要求广告位所在的页面不影响品牌调性。brandsafety指的正是这一类ad verification服务。Peer 39/ADbug是国内外该服务提供商中最出名的。机房竞价前请求（国内/海外）服务器page级别的属性数据，判断是否危害广告主品牌安全。**用于保护品牌类广告主广告调性**。
主要用于高通广告主第三波投放。

### Task
- 对接brandsafety服务
- 调研brandsafety原理、关键指标和边界并分享

### Action
- 控制商务期望（最重要）
- 探索&明确产品需求和商务紧急性
    - 需求&里程碑确认
    - 技术&产品文档确认
    - 确认前端界面开发方案和后端出价、获取数据逻辑
- 保证使用
    - 测试（合理预计过滤比例）
    - 数据报告（验证有效性）
    - 使用方案
- 基于行业内brandsafety的adserving调研并产出[PPT](https://noragithub.github.io/PRD_deployment/%E5%93%81%E7%89%8C%E5%AE%A2%E6%88%B7%E5%85%B3%E5%BF%83%E7%9A%84brandsafety%E5%92%8C%E5%8F%8D%E4%BD%9C%E5%BC%8A/index.html)

### Result
顺利对接brandsafety服务，服务高通广告主并形成文档、ppt，完成分享

##  Sizmek_ad_serving对接
### Situation
Sizmek为4A提供广告创意& 监测服务，包括动态创意和brandsafety （品牌安全-post bid）／viewability（可视率）监测。**通过对接，保证创意、落地页和竞价服务（DSP端、渠道端）相互兼容**。

主要用于高通广告主投放。

### Task
- 对接第三方动态创意服务（通过HTML投放js）
- 保证video in banner正常投放（在banner广告位做视频投放）

### Action
- 控制商务期望（最重要）
- 探索&明确产品需求
    - 确认“动态创意”包含什么服务（通过HTML投放js）
    - 确认video in banner投放具体细节
        - 特殊广告形式下的成本（CDN）
        - 非expandable
        - 确认具体尺寸& 分别尺寸可竟得的impression数据
        - 具体创意尺寸& 格式、压缩方式、加载时长等（第几帧开始第几帧结束）
    - 广告主方/渠道方是否有特殊需求
        - 渠道/广告位粒度的协议确认
- 测试
    - 保证统计、监测服务正常
    - 分终端、分ADX的adserving测试

### Result
正常投放，并在投放过程中解决渠道对类Mraid协议、创意兼容问题（设备级别）

##  高通DSP投放
### Situation
高通（甲方）的需求来自奥美（乙方）和sizmek（丙方），舜飞在商务投放中处于丁方的地位，需要向上把握需求。且商务决策中每一方的需求点都不一样，执行的细节有很大需要把握的空间和需求的正确性。

- 需求来源于邮件，而且同一个需求在不同的时间点由不同的人提出，继续确认哪些需求是同一个需求，那些不是，确认范围和时间点。



主要用于高通广告主第二波投放。

高通是2016年最重要的品牌类广告主，品牌类广告主的需求变化多且个性化。**解决业务层面为当前产品/技术架构带来的挑战**。

### Task/Result 
- 保证campaign按时上线
- 满足商务“平分轮显”需求

### Action
- 控制商务期望（最重要）
    - 明确cookie保存周期最多半年（频次控制by cookie&by time）
- 明确商务需求，包括媒介需求& 产品功能需求，确认需求边界于关键时间点
    - 确认“平分轮显”具体要求（by day）
        - 跨渠道跨创意的频次控制(by adx&by creative&by cookie)    
        - 无法预测的累积时间段的频次控制（by cookie&by time)
        - 创意轮流显示且平分显示次数(by creative&by cookie)
    - 保证优酷/腾讯/爱奇艺PDB 市场对接完成（OTV）
- 从商务需求转化为技术需求，拆分子任务和关键验证点（创意按by cookie/by 频次 随机）
- 测试
    - 设置合适的campaign（按活动/按创意，对比频次数据，预期数据无差异）
    - 保证统计discrepancy
- 广告主方/渠道方是否有特殊需求
    - 渠道/广告位粒度的协议确认
    - 确认RTB/PDB市场完成对接
- 考虑可扩展性
    - 跨渠道跨创意的频次控制(by adx&by creative&by cookie)
    - campaign、产品、创意包粒度的“平分轮显”需求
    - 全局轮显& 用户轮显
    - 3+reach最多

##  移动RTB/全渠道PMP/PDB市场对接
### Situation/Result
14年后，互联网进入移动时代。同时广告市场也根据商务需求和技术发展进入了RTB/PMP/PDB的精细化运营的时代，RTB/PMP/PDB市场业务有区别，需要根据协议和商务业务调整竞价服务。从产品层面考虑兼容性，**兼容跨渠道差异以及现有产品功能**。
该项目由很多小项目组成，根据不同项目case by case 解决问题，兼产品经理和项目经理，根据协议进行产品需求设计，协调资源，推动进度，保证上线。
例如：
- 腾讯渠道RTB和PMP/PDB通过两套（相似）协议和分离环境实现两部分流量的分发。流程有不一样的地方，为了高度的拓展性，牺牲了一定的的便利性：如何关联广告主，如何放量。为了可理解性，希望一定的开发资源。
- 资质的扩展、adx的合并与分离等（新旧数据）
- 通过算法保证PDB按比例返量并考虑极端情况（提前和媒体&广告主沟通）
- PDB竞价暂停情况

##  SEM品牌推广
SEM做广告投放
### Situation
原MKT SEM离职，老板希望重新整理BiddingX品牌的SEM&DSP投放，通过数据化运营优化后吸引客户注册

### Task
负责BiddingX品牌的SEM& DSP营销，调整关键词和campaign，优化后吸引客户注册

### Action
- 整合Site Tracking和PDMP重定向目标人群
- 利用数据可视化展现工作成果

### Result
- 日均消费提高400%（700-->3000)
- 点击率提升100%(0.3%-->0.7%)
- 展示量提升100%
![SEM](https://noragithub.github.io/PRD_deployment/SEM/SEM.png)

##  有赞商城DSP推广
### Situation
电商APP推广，为了开拓电商客户，有赞是2015年重点客户之一。通过使用包括Banner创意和视频创意在内，尝试引导网页流量转化为APP激活，用于探索PC往APP转化路径。

### Task
- 通过投放优化，满足客户CPA需求
- 尝试引导网页流量转化为APP激活 

### Action
- 通过业务分析，制定投放策略
    - 受众分析
    - 选择媒体
    - 不断建立测试campaign& 调整优化创意、落地页、定向条件
    - 整合第三方DMP数据到DSP进行目标人群重定向
- 获取激活数据用于优化
    - 对接第三方监测平台，通过S2S scheme获取激活数据
    ![S2S scheme](https://noragithub.github.io/PRD_deployment/S2S_scheme/S2S_scheme.jpg)
    - 在PC端缺乏数据回传机制后，改方向投放
- 使用项目管理平台（tower.im）协调优化师与开发资源

### Result
优化后，Android端正常投放并达到客户CPA要求。

##  移动SSP_iOS_SDK第一版
### Situation
DSP发展到一个阶段，意识到媒体是广告市场的竞争力所在，需要拓展自有的核心视频& 信息流资源。商务团队已经有几个目标客户拓展中，有其紧急性，但与此同时，SSP开发团队项目由于需求持续变更，无法迭代。老板需要两周内完成完成第一版，后续迭代。

### Task/Result
保证移动SSP iOS SDK第一版两周内上线（老板deadline）

### Action
- 通过SSP PM不进行次要需求的开发（只保留关键指标：展示量& 收入 的统计和测试）
- 保证BD团队和开发团队沟通，确保上线日期

---

# 唯品会-数据产品经理
向BI(一级部门)/数据产品（二级部门）高级经理汇报。
数据分析产品化。涉及数据产品的主要流程，从PRD文档、到产品的UI设计，以及跨系统的产品整合，甚至部分ETL，黑白盒数据测试和项目管理工作。
1. 负责面向管理层的APP数据产品, 产品化常规的运营分析流程，支持决策；
2. 接手旧产品，并根据管理层反馈做快速迭代；
3. 参与创新APP项目，如， 我是妈咪 （ 母婴闪购电商）和 hey!购物 （基于库存的个性化推荐APP）;

## 总裁看板
### Situation
利用Oracle BIEE实现销售、流量业务的数据报表化，管理层通过每日邮件方式了解业务运营。原“总裁看板”产品经理离职，作为“总裁看板”第二任产品经理接手该项目。也是作为“数据产品经理”后，第一个接手负责的产品/项目。由于ETL开发人员紧张，也参与部分ETL开发工作（熟悉业务&具备独立分析能力）。

### Task
- 保证产品数据正确& 稳定运行，及时处理脏数据& fix bug
- 结合新业务进行产品迭代，通过数据向管理层传递最新业务的现状
- 对管理层提出的运营问题快速反馈（具备分析师经验）& 迅速响应新需求

### Action
- 了解产品开发/上线流程
- 了解产品指标体系&统计口径，对比常规分析报告与差异，预判可能出现的问题

### Result
顺利接手旧系统，并对管理层需求快速反馈。（5天内）
+ 针对数据中的问题提供分析报告
+ 根据业务变化进行ETL工作
+ 协调资源保证产品按时上线

## 创新APP项目
### Situation/Result
唯品会执行多APP战略，要求基于目前的业务系统孵化新业务APP。BI部门也需要参与到业务运营，承担KPI，真正做到数据驱动业务（业绩）。包括“我是妈咪”（母婴特卖垂直领域电商APP）和“hey!购物”（基于库存的个性化推荐APP）。
由于对接品牌部门和市场部门业务分析，对推广业务熟悉。
- 负责整合旧系统的推广、渠道、账户业务逻辑到新app。
- 负责H5推广活动页产品实现，通过微博、微信渠道吸引新用户
- 功能测试和数据埋点

### Task
- 对接优惠系统、渠道推广系统到“我是妈咪”（母婴特卖垂直领域电商APP）
- “我是妈咪”（母婴特卖垂直领域电商APP）部分购物车相关、用户中心（profile）相关设计和系统对接
- 对微信HTML5推广需求出设计方案&数据追踪方案
- 接手& 跟进 “hey!购物”（基于库存的个性化推荐APP）的 数据埋点&负责用户中心（profile）相关功能测试用例

### Action
- 了解旧系统业务逻辑，出产品设计&跟进项目进度
- 了解需求细节

## 移动司南
[PRD](https://noragithub.github.io/PRD_deployment/移动司南/index.html)
### Situation/Result
1. 面向对象是CXO level的管理层，数据需求主要是对经营profile的了解。
2. 当时部门要求pitch老板资源的，因此，对高保真要求高于详细文档要求。当时目标是成为独立事业部，开发和设计资源都很少，能够用口头沟通代替部分文字表达。
3. 这是第一份偏设计的PRD，而且要求（尽量）高保真，有问题，但不影响展示（主要根据手机屏幕大小做了取舍）
全面负责面向管理层的移动端分析产品，包括产品设计，数据探索和数据可视化。

### Task
基于PC端产品，根据移动端特性取舍，做移动端数据产品设计

### Action
- 阅读iOS&Android设计指南
- 数据探索设计是否合理（指标范围，实际数据展现，分析角度，计算精度）
- 尽快提交设计获取反馈进行迭代


[Android中文](http://wiki.jikexueyuan.com/project/material-design/)
[Material Design](http://www.google.com/design/spec/material-design/)
[iOS8人机界面指南](https://isux.tencent.com/ios8-human-interface-guidelines.html)
[Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/overview/design-principles/)

## 唯品司南
### Situation/Result
作为功能性产品经理，参与到品牌、档期、品类维度的分析产品中去
为品牌销售和区域销售提供数据决策的可视化产品。对常规化的分析进行ETL后，利用high-chart，e-chart框架展现，包括档期分析，退拒分析等。获得管理层高度认可。一期参与到后期数据验证，负责黑白盒数据测试&验证，确保数据正确性。二期作为功能性产品经理，参与到品类分析、品牌分析的产品规划。并对数据可视化提供UE建议。

### Task
- 了解产品指标体系&统计口径，对数据进行黑盒测试，找出常规分析报告差异
- 作为功能性产品经理，参与到品类分析、品牌分析的产品规划。并对数据可视化提供UE建议。

### Action
- 数据探索&分析
- 和商务运营沟通
- 了解数据口径
- 分析产品，具体看什么指标，怎么看？具体如何同比，如何环比。

## 唯品经纬
### Situation/Result
“唯品经纬”属于创新项目，希望通过利用订单地址数据，挖掘客户有价值的人口统计学信息& 进行职业信息分析，并根据这些信息找到营销切入点，实现的业务可视化，为市场进行地面推广提供支持。对唯品经纬数据部分进行黑盒测试。由于ETL离职，作为ETL开发参与到白盒测试，保证数据正确性。

### Task
- 了解产品指标体系&统计口径，对数据进行黑盒测试，找出常规分析报告差异
- 作为ETL，协助指标体系口径一致

### Action
- 确认项目边界
- 白盒子口径检验
- 设计数据测试方案
- 确认项目便捷

---

# 唯品会-数据分析师
先后向BI(一级部门)/运营分析（二级部门）经理、主管汇报。
作为运营分析组一员，每周提供运营分析以及日常快捷反馈到总裁会，并根据反馈进行运营层面的分析建议。
负责市场运营方面日常分析：
1. 广告投放&渠道：为衡量渠道价值进行资源分配而构造归因模型；
2. 促销组合：不同促销形式的效果&组合分析，并形成报表系统；
3. 财务：市场费用的构成分析；

## 微信特卖
### Situation/Result
100%对微信商城的BI分析负责，形成虚拟团队向品牌副总裁汇报。利用微信公众号，在微信端进行销售。

### Task
100％support微信商城销售
- 每周提供微信数据周报以及数据分析
- 对微信常态分析产品化
- 建议微信端代金券全场使用，促进跨品牌销售；

### Action
- 了解底层仓库表&申请权限
- 提供业务报表
- 和大数据部门沟通，形成微信报表产品

## 归因模型
### Situation/Result
独立分析项目。确认定岗BI部门后，参与到BI对市场部的跨渠道归因模型项目中，探索跨投放与销售的因果关系。利用归因（助攻模型）模型衡量渠道贡献价值，尝试利用图论实现可视化，提供业务决策支持。

### Task

### Action



# 其它
## 电子商务代运营
### Situation
大二参与到电商创业团队，当时天猫（淘宝商城）刚成立，全网都有流量、运营资源倾斜，从高利润品类切入（化妆品），做到50w/m销售业绩
### Task
确认推广目标，参与淘店运营
### Action
- 微博运营活动策划，目标设定，拉去资源，淘店引流
- 校园活动策划与执行

### Result
50w/m销售业绩



PS：
工作中，探索的部分比较多，受限于资源和信息不对称，自己发起的商业化产品较少
但尝试发起并运营内部的项目管理产品 - wiki & jira & Gitbook


由于舜飞是starup，以及作为agent的特殊性，这边的工作比较少考虑完整的测试用例覆盖，工作中缺乏完整的联调环境和测试环境，广告投放正式环境和联调环境本质还是有差距的。这是由于：
- 自身无法控制对方环境的完整性
- 广告市场的极度长尾化与信息不对称的资源割据
- 更多通过线上测试以及快速回归（出价和统计都比较难覆盖）

舜飞有明确的规章制度把产品技术和商务区隔开，我无法得知整个投放涉及的预算，但后续第二第三波投放都主要由我对接，投放没有出现大问题，基本按进度上线
