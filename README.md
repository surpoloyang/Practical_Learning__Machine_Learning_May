# AI+X 实践学习-机器学习-五月-学习笔记

<div style="text-align: center;">by 1辰</div>

<div style="text-align: center;">
  <a href="https://datawhaler.feishu.cn/docx/JlC8dJkVrokjNMxibH1cJYDDnF8">本次实践学习介绍</a>
</div>

<div style="text-align: center;">
  <a href="https://datawhaler.feishu.cn/docx/GrWRdUgfKoea91xq43YcsNHGnYd">本次实践学习相关问题解答汇总</a>
</div>

# 往期竞赛：跨境电商智能算法大赛

## 竞赛概述
- **竞赛名称**：安泰杯 —— 跨境电商智能算法大赛
- **竞赛目的**：解决AliExpress在不同国家市场推广时遇到的用户行为差异问题，优化推荐算法。
- **竞赛链接**：[天池大赛-阿里云天池](https://tianchi.aliyun.com/competition/entrance/231718/information)
- **竞赛baseline**：[冠军方案分享](https://github.com/RainFung/Tianchi-AntaiCup-International-E-commerce-Artificial-Intelligence-Challenge/tree/master#%E8%B5%9B%E9%A2%98%E6%95%B0%E6%8D%AE)
- **复现**：[复现代码](https://github.com/surpoloyang/Practical_Learning__Machine_Learning_May/tree/main/Past-Competition-learning/Cross-border-E-commerce-Algorithm-Competition/code)

## 竞赛背景

- AliExpress是中国最大的出口B2C电商平台，覆盖全球230个国家和地区，支持18种语言站点。目前某些国家的用户群体比较成熟，沉淀了大量用户的行为数据。这些沉淀下来的用户数据被挖掘利用后形成推荐算法，用来更好的服务于该国用户。

### 平台面临的挑战

- 还有一些待成熟国家的用户在AliExpress上的行为比较稀疏；
- 对于这些国家用户的推荐算法如果单纯不加区分的使用全网用户的行为数据，可能会忽略这些国家用户的一些独特的特征；
- 如果只使用这些国家的用户的行为数据，由于数据过于稀疏，不具备统计意义，会难以训练出正确的模型。

### 需要解决的问题

- 如何利用成熟国家的稠密用户数据和待成熟国家的稀疏用户数据，训练出适合待成熟国家用户的正确推荐模型。

## 竞赛数据

### **初赛数据**

- 商品属性表：涉及2840536个商品，大部分数据含有类目ID、店铺ID和加密价格，其中价格的加密函数f(x)为一个单调增函数。

- 训练数据：包括xx国全部和yy国A部分用户的购买数据。

  - | 国家 |  记录数  | 买家数 |
    | :--: | :------: | :----: |
    |  xx  | 10635642 | 670631 |
    |  yy  | 2232867  | 138678 |

- 测试数据：yy国B部分用户的购买数据，去除最后一条购买记录。

  - | 国家 | 记录数 | 买家数 |
    | :--: | :----: | :----: |
    |  yy  | 166832 | 11398  |

- 商品属性表、训练数据、测试数据对应的文件列表为：item_attr, train和test。

#### **数据格式**

- | buyer_country_id | buyer_admin_id | item_id |  create_order_time  | irank |
  | :--------------: | :------------: | :-----: | :-----------------: | :---: |
  |        xx        |     817731     | 4033525 | 2018-06-12 07:12:58 |   1   |
  |        xx        |     817731     |  98120  | 2018-06-11 07:12:58 |   2   |

- buyer_country_id: 买家国家id, 只有'xx'和'yy'两种取值

  buyer_admin_id: 买家id

  item_id: 商品id

  create_order_time: 订单创建时间

  irank: 每个买家对应的所有记录按照时间顺序的逆排序

#### **数据集特点**

- 用户至少有7条购买数据。
- 测试数据中最后一条购买数据的商品在训练数据中出现过。
- 少量跨国买家记录在评测中被忽略。

#### **提交要求**

- 提交关于yy国B部分用户最后一条购买数据的预测Top30。

- 提交格式为CSV文件，包含**买家ID--buyer_admin_id**和**预测的Top30商品ID--predict 1 ,..., predict 30**，按**概率从高到低排序**，**不含表头**，如下：

- > 1233434,4354,23432,6546,...,91343
  >
  > 2132133,154,20987,34349,...,78772

### **复赛数据**

在给出若干日内来自某成熟国家xx的部分用户的点击购买数据，以及来自某待成熟国家yy和待成熟国家zz的A部分用户的点击购买数据，以及国家yy和zz的B部分用户的截止最后一条购买数据之前的所有点击购买数据，让参赛人预测B部分用户的最后一条购买数据。

- 商品属性表：涉及9136277个商品，同样大部分数据含有类目ID、店铺ID和加密价格，其中价格的加密函数f(x)为一个单调增函数。

- 训练数据：包括xx国用户的点击、购买数据，以及yy国和zz国A部分用户的点击、购买数据。

  - | country_id | 点击记录数 | 购买记录数 | 买家数 |
    | :--------: | :--------: | :--------: | :----: |
    |     zz     |  4582953   |  1773429   | 45795  |
    |     yy     |  5241393   |  1358329   | 58106  |
    |     xx     |  42046596  |  5241625   | 511059 |

- 测试数据：yy国和zz国B部分用户的最后一条购买数据之前的点击购买数据。

  - | country_id | 点击记录数 | 购买记录数 | 买家数 |
    | :--------: | :--------: | :--------: | :----: |
    |     zz     |   424506   |   164081   |  4275  |
    |     yy     |   429319   |   65887    |  5569  |

##### **数据格式**

- | buyer_country_id | buyer_admin_id | item_id |      log_time       | irank | buy_flag |
  | :--------------: | :------------: | :-----: | :-----------------: | :---: | :------: |
  |        xx        |     817731     | 4033525 | 2018-06-12 07:12:58 |   1   |    1     |
  |        xx        |     817731     |  98120  | 2018-06-11 07:12:58 |   2   |    0     |

- buyer_country_id: 买家国家id, 只有'xx','yy','zz'三种取值

  buyer_admin_id: 买家id

  item_id: 商品id

  log_time: 商品详情页访问时间

  irank: 每个买家对应的所有记录按照时间顺序的逆排序

  buy_flag: 当日是否购买

#### **数据集特点**

- 每个用户有若干条点击数据和至少1条购买数据 （但测试数据中该条购买记录可能未给出到选手）

- 每个用户的最后一条数据的buy_flag一定为1 （但测试数据中该条数据未给出到选手）

- 测试数据中每个用户的最后一条点击数据（也是购买数据）所对应的商品一定在训练数据中出现过.

- 可能存在少量跨国买家

#### **提交要求**

- 关于yy国、zz国的B部分用户每个用户的最后一条购买数据的预测Top30。

- 提交格式为CSV文件，包含**买家ID--buyer_admin_id**和**预测的Top30商品ID--predict 1 ,..., predict 30**，按**概率从高到低排序**，**不含表头**，如下：

- > 1233434,4354,23432,6546,...,91343
  >
  > 2132133,154,20987,34349,...,78772

## 评估方法

- 使用MRR（Mean Reciprocal Rank）评估模型性能。

- 首先对选手提交的表格中的每个用户计算用户得分，用户得分计算公式： 
  $$
  \text{score(buyer)} = \sum_{k=1}^{30} \frac{s(buyer,k)}{k}
  $$
  其中, 如果选手对该buyer的预测结果$predict k$命中该$buyer$的最后一条购买数据命中，则  $s(buyer,k) = 1$，未命中则$s(buyer,k) = 0$。

- 选手得分为所有用户得分$score(buyer)$的平均值。

## 结果复现
- 选手需在限定时间内，在标准软硬件环境下复现结果。
- 复现结果的性能指标与提交最优结果的差异需在允许范围内。

## 注意事项
- 参赛者只能使用组织者提供的数据进行模型训练。
- 禁止人工标注/修改评测结果数据，禁止多账号刷分。
- 违规行为将导致取消参赛资格及奖励。

## 总结
安泰杯跨境电商智能算法大赛旨在通过算法优化AliExpress的海外市场推荐系统，解决不同国家用户行为差异问题。竞赛分为初赛和复赛，提供详细的数据集和评估标准，要求选手在规定时间内复现结果，并严格遵守比赛规则。

---

# 往期竞赛：智慧海洋建设算法大赛

## 竞赛概述

- **竞赛名称**：2020数字中国创新大赛—算法赛：智慧海洋建设
- **竞赛链接**：[天池大赛-阿里云天池](https://tianchi.aliyun.com/competition/entrance/231768/information)
- **竞赛目的**：基于位置数据对海上目标进行智能识别和作业行为分析。
- **竞赛任务**: 分析渔船北斗设备位置数据，判断渔船的生产作业行为（拖网作业、围网作业、流刺网作业）。
- **竞赛baseline**：[各开源方案](https://github.com/datawhalechina/team-learning-data-mining/tree/master/wisdomOcean)
- **复现**：[黄海广教授团队baseline复现](https://github.com/surpoloyang/Practical_Learning__Machine_Learning_May/tree/main/Past-Competition-learning/Smart-Ocean-Practice-Competition)

## 竞赛数据

### **初赛数据**

 提供11000条渔船北斗数据，包括渔船ID、经纬度坐标、上报时间、速度、航向信息。

- **数据特点**: 数据可能存在上报坐标错误、数据丢失、疯狂上报等问题。

- **数据示例**: 

  - | 渔船ID |         x         |         y         | 速度 | 方向 |    time    | type |
    | :----: | :---------------: | :---------------: | :--: | :--: | :--------: | :--: |
    |  1102  | 6283649.656204367 | 5284013.963699763 |  3   | 12.1 | 0921 09:00 | 围网 |

  - 渔船ID：渔船的唯一识别，结果文件以此ID为标示
    x: 渔船在平面坐标系的x轴坐标
    y: 渔船在平面坐标系的y轴坐标
    速度：渔船当前时刻航速，单位节
    方向：渔船当前时刻航首向，单位度
    time：数据上报时刻，单位月日 时：分
    type：渔船label，作业类型

### **复赛数据**

复赛考虑以往渔船在海上作业时主要依赖AIS数据，北斗相比AIS数据，数据上报频率和数据质量均低于AIS数据，因此复赛拟加入AIS轨迹数据辅助北斗数据更好的做渔船类型识别，其中AIS数据与北斗数据的匹配需选手自行实现

- **AIS数据：**

  - | ais_id |   lon    |   lat   | 船速 | 航向 |    time    |
    | :----: | :------: | :-----: | :--: | :--: | :--------: |
    |  110   | 119.6705 | 26.5938 |  3   | 12.1 | 0921 09:00 |

  - ais_id：AIS设备的唯一识别ID

## 提交说明

- **初赛提交**: 使用主办方提供的数据训练算法，预测测试集中的渔船作业类型，生成csv文件提交，提交文件示例（result.csv）。

- **文件要求**: 渔船ID和作业类型一一对应，utf-8编码，不含表头，包含三种作业类型。

- > 1901,围网 
  >
  > 1902,刺网

## 评估指标

- **初赛评估**: 以3种类别的F1值平均值作为评价指标，F1值越大越好。

- **计算公式**:

  $$F1=\frac{2*P*R}{P+R}$$

  $$Score=\frac{F1_{围网}+F1_{刺网}+F1_{拖网}}{3}$$

- 其中P为某类别的准确率，R为某类别的召回率，评测程序f1函数为sklearn.metrics.f1_score，average='macro'。

## 审核提交

- **初赛审核**: TOP160团队需提交算法代码、模型权重、说明文件等，压缩为.zip文件。
- **文件审核**: 审核内容包括代码、算法、运行说明，以及防止人工标注、抄袭等不当行为。
- **晋级条件**: 排名前120名且通过支付宝实名认证的队伍进入复赛。

## 复赛规则

- **数据匹配**: 复赛中将加入AIS轨迹数据辅助北斗数据，匹配工作由选手完成。
- **提交方式**: 采用docker容器镜像提交。
- **成绩审核**: TOP20参赛队伍的模型和代码将被审核。
- **决赛资格**: 总分排名前6的队伍将受邀参加决赛。

## 注意事项

- **禁止行为**: 禁止使用外部数据、人工修改评测结果、多账号刷分等。
- **惩罚措施**: 发现不当行为即取消比赛资格和奖励。
- **模型策略**: 不鼓励过度堆砌模型，复赛将限制算法运行时间。

## 其他信息

- **数据脱敏**: 原始数据经过脱敏处理，渔船信息被隐去。
- **专业知识**: 选手可通过学习围网、刺网、拖网等专业知识辅助数据处理。

# 本次竞赛：零基础入门金融风控-贷款违约预测

## 竞赛概述

- **竞赛链接**：[天池大赛赛题信息](https://tianchi.aliyun.com/competition/entrance/531830/information)
- **竞赛任务**：预测用户贷款是否违约

## 竞赛数据

- **数据量**：来自某信贷平台的贷款记录，数据量超过120万条
- **变量**：47列，包括15列匿名变量
- **数据划分**：
  - 训练集：80万条
  - 测试集A：20万条
  - 测试集B：20万条
- **脱敏信息**：employmentTitle、purpose、postCode、title等字段将进行脱敏处理

- **字段表**：

- |       Field        |                         Description                          |
  | :----------------: | :----------------------------------------------------------: |
  |         id         |                为贷款清单分配的唯一信用证标识                |
  |      loanAmnt      |                           贷款金额                           |
  |        term        |                        贷款期限（年）                        |
  |    interestRate    |                           贷款利率                           |
  |    installment     |                         分期付款金额                         |
  |       grade        |                           贷款等级                           |
  |      subGrade      |                        贷款等级之子级                        |
  |  employmentTitle   |                           就业职称                           |
  |  employmentLength  |                        就业年限（年）                        |
  |   homeOwnership    |              借款人在登记时提供的房屋所有权状况              |
  |    annualIncome    |                            年收入                            |
  | verificationStatus |                           验证状态                           |
  |     issueDate      |                        贷款发放的月份                        |
  |      purpose       |               借款人在贷款申请时的贷款用途类别               |
  |      postCode      |         借款人在贷款申请中提供的邮政编码的前3位数字          |
  |     regionCode     |                           地区编码                           |
  |        dti         |                          债务收入比                          |
  | delinquency_2years |       借款人过去2年信用档案中逾期30天以上的违约事件数        |
  |    ficoRangeLow    |            借款人在贷款发放时的fico所属的下限范围            |
  |   ficoRangeHigh    |            借款人在贷款发放时的fico所属的上限范围            |
  |      openAcc       |              借款人信用档案中未结信用额度的数量              |
  |       pubRec       |                      贬损公共记录的数量                      |
  | pubRecBankruptcies |                      公开记录清除的数量                      |
  |      revolBal      |                       信贷周转余额合计                       |
  |     revolUtil      | 循环额度利用率，或借款人使用的相对于所有可用循环信贷的信贷金额 |
  |      totalAcc      |              借款人信用档案中当前的信用额度总数              |
  | initialListStatus  |                      贷款的初始列表状态                      |
  |  applicationType   |       表明贷款是个人申请还是与两个共同借款人的联合申请       |
  | earliesCreditLine  |              借款人最早报告的信用额度开立的月份              |
  |       title        |                     借款人提供的贷款名称                     |
  |     policyCode     |     公开可用的策略\_代码=1 新产品不公开可用的策略_代码=2     |
  |   n系列匿名特征    |        匿名特征n0-n14，为一些贷款人行为计数特征的处理        |

## 评测标准

- **提交结果**：测试样本是1的概率（即违约概率）
- **评价方法**：AUC（Area Under the ROC Curve），值越大越好

## 结果提交

- **格式要求**：与sample_submit.csv格式一致

- **文件格式**：CSV

- **示例**：

- > id,isDefault
  >
  > 800000,0.5 
  >
  > 800001,0.5 
  >
  > 800002,0.5 
  >
  > 800003,0.5

## 比赛学习资料	

- **资料来源**：论坛中的往期选手发布的比赛baseline
- **内容**：交流、学习解题思路
