---
layout: post
title: "2021 ATT&CK技术与应用论坛"presentations阅读笔记
categories: Cybersecurity
description: 论坛presentations阅读笔记
keywords: ATT&CK, Cybersecurity
---



> 9月9日，以“推进ATT&CK防御体系落地，应对数字时代网络威胁挑战”为主题的2021 ATT&CK技术与应用论坛在北京成功召开。本次论坛由赛可达实验室、国家计算机病毒应急处理中心、国家网络与信息系统安全产品质量监督检验中心联合主办，北京升鑫网络科技有限公司（青藤云安全）、北京微步在线科技有限公司、绿盟科技集团股份有限公司、厦门服云信息科技有限公司（安全狗）协办。政府主管领导、ATT&CK技术资深专家、科研院所安全专家、金融等行业机构安全负责人、知名互联网企业安全负责人、安全测评机构负责人、网络安全厂商专家、网络安全从业者等200多人出席了会议。
>
> Presentation下载地址：[赛可达实验室](http://c.skdlabs.com/index.php?m=content&c=index&a=lists&catid=375)



## ATT&CK的技术研究与应用探索

*姜政伟*

*中国科学院信息工程研究所，网络安全防护技术北京市重点实验室*

### 背景

![image-20211123105812347](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123105812347.png)



![image-20211123110211140](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123110211140.png)

### 学术界对ATT&CK的研究概况  

学术研究覆盖恶意文件分析、 威胁检测与狩猎、 攻击模拟与防御评估、 漏洞挖掘与分析、 攻击归因与攻击者画像、 威胁建模， 以及安全知识图谱构建等研究方向。  

![image-20211123110522816](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123110522816.png)

- 恶意文件分析
- 威胁检测与威胁狩猎
  - 基于ATT&CK的框架 ， ==从含有攻击技术描述的文本中抽取实体和关系==， 构建对应于各攻击技术的APT攻击语义规则， 在日志数据中进行威胁检测与威胁狩猎；*Milajerdi S M, Gjomemo R, Eshete B, et al. Holmes: real-time apt detection through correlation of suspicious information flows[C]//2019 IEEE Symposium on Security and Privacy (S&P).IEEE, 2019: 1137-1152*
- 攻击模拟和防御评估
  - 对 ATT&CK中各攻击技术的影响和复杂程度分配后果严重性权重， 并依据攻击模拟的成功与否对企业网进行风险评估；*Manocha H, Srivastava A, Verma C, et al. Security Assessment Rating Framework for Enterprises using MITRE ATT&CK Matrix[J]. arXiv preprint arXiv:2108.06559, 2021.*
  - 基于 ATT&CK ICS **构建多种攻击向量**， 在各类工控场景下进行攻击模拟和防御评估；*Toker F S, Akpinar K O, ÖZÇELİK İ. MITRE ICS Attack Simulation and Detection on EtherCAT Based Drinking Water System[C]//2021 9th International Symposium on Digital Forensics andSecurity (ISDFS). IEEE, 2021: 1-6*
  - 开发网络安全培训系统， **利用 ATT&CK 攻击知识生成网络威胁**， 用于安全教育培训和防御演练；*Kim D, Kim Y, Ahn M K, et al. Automated Cyber Threat Emulation Based on ATT&CK for Cyber Security Training[J]. Journal of the Korea Society of Computer and Information, 2020, 25(9): 71-80*
- 漏洞挖掘与漏洞分析
- 攻击归因与攻击者画像
- 网络威胁建模
- 网络安全知识图谱的构建
  - 基于文本分类的方法对**攻击描述文本的 ATT&CK 分类**， 在漏洞知识研究中**将CVE映射到ATT&CK**；*Legoy V, Caselli M, Seifert C, et al. Automated Retrieval of ATT&CK Tactics and Techniques for Cyber Threat Reports[J]. arXiv preprint arXiv:2004.14322, 2020*、*Husari G, Al-Shaer E, Ahmed M, et al. TTPdrill: Automatic and accurate extraction of threat actions from unstructured text of cti sources[C]//Proceedings of the 33rd Annual Computer Security Applications Conference. 2017: 103-115*
  - 采用**信息检索**的方法， 利用预构建的攻击技术词袋进行信息检索， 抽取攻击技术描述；*Al-Shaer R, Spring J M, Christou E. Learning the Associations of MITRE ATT & CK Adversarial Techniques[C]//2020 IEEE Conference on Communications and Network Security (CNS). IEEE, 2020: 1-9*
  - 利用短语嵌入的语义聚类方法代替词袋模型和正则表达式， **依据建立的 ATT&CK 技术本体来完成攻击技术抽取**；*Purba M D, Chu B, Al-Shaer E. From Word Embedding to Cyber-Phrase Embedding: Comparison of Processing Cybersecurity Texts[C]//2020 IEEE International Conference on Intelligence and Security Informatics (ISI). IEEE, 2020: 1-6*

### 团队对ATT&CK的探索与应用

![image-20211123135621607](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123135621607.png)

- 知识情报提取
  - 攻击工具提取
  - 攻击手法抽取
- 恶意文件分析：基于规则的行为映射
- 黑客组织画像
- 事前攻击模拟
- 事中威胁检测
- 事后攻击归因

### 存在的问题

- 在应用中的“框架对齐”与“语义鸿沟”
  - 在恶意样本分析任务中的框架难以对齐： ATT&CK的技术描述粒度相对较粗， 表述维度与恶意样本的分析维度不一致；
  - 在威胁检测/攻击技术挖掘任务中的“鸿沟”： MITRE官方没有提供结构化的攻击技术表述， **通过NLP技术将ATT&CK用于技术知识获取、 威胁检测时， 存在“技术知识” 与“检测对象” 或“技术描述实例” 之间的语义鸿沟**。
    - 例如：**攻击技术名称** “Input Capture:GUI Input Capture” 与**攻击技术实例描述**中的关键“Password” 和“Credentials” 没有直接的关系
- 以点状的攻击技术描述为主， 缺乏攻击技术之间的组合与关联
  - **当下的ATT&CK更像是攻击技术的枚举与条目丰富， 尚未能指出如何进行攻击技术之间的组合与关联**， 不利于实现TTPs（Tactics,Techniques,Proceedings——战技过程） 链条的构建
- 对网络空间可观测数据的覆盖性不充分
  - 主机侧的攻击技术占了大部分（74%) ，流量侧的较少（17%) ，云和虚拟化相关的也不多（19%） （部分技术在主机或云上都可使用，被重复统计），对于以骨干网、网络出口为主的安全监管场景适用性低。  

## ATT&CK：从威胁框架到攻击链路（--基于ATT&CK的入侵检测体系  ）

*陈杰*

*微步在线*

### ATT&CK威胁框架

![image-20211123143623978](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123143623978.png)

- 不能照搬ATT&CK框架构建实战化的威胁检测体系。

  - 攻击视角 vs 防守视角：相交，但不重合；
  - 单个行为的告警不准；
  - 单个行为的告警溯源困难。

- 基于ATT&CK框架构建实战化的威胁检测体系：多级入侵检测体系 

  ![image-20211123144123656](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123144123656.png)

### 单点检测

![image-20211123144237201](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123144237201.png)

### 行为组合检测

- 风险行为组合

  ![](https://gitee.com/misite_J/blog-img/raw/master/img/%E9%A3%8E%E9%99%A9%E8%A1%8C%E4%B8%BA%E7%BB%84%E5%90%88.png)

### 攻击链路检测与事件聚合

- 一次完整的APT攻击过程往往使用多种攻击战术与技术，并呈现一定的攻击流程。 

  ![image-20211123144813018](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123144813018.png)

  - Web攻击场景

    ![image-20211123145047697](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123145047697.png)

  - 木马投递场景

    ![image-20211123145129315](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123145129315.png)

  - 建立远控通道场景

    ![image-20211123145206610](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123145206610.png)

- 关联恶意行为上下文，精准告警

- 聚合相关告警，还原攻击链路

- 威胁链路可视化，加快溯源

## ATT&CK 在攻击事件关联分析中的实践  

*韩博*

*国家互联网应急中心*

### 网空威胁框架 ATT&CK  

![image-20211123151634584](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123151634584.png)

- ATT&CK 框架典型应用场景：威胁情报

  - 情报交换
    - ATT&CK 规范化的威胁情报可以在组织间更好地进行情报的交换与沟通，帮助分析人员更好地理解攻击者的
      行为
    - ATT&CK 也可以与网络威胁情报领域中的其他框架/标准，如结构化威胁信息表达式（STIX）、**通用攻**
      **击模式枚举与分类（CAPEC）**等结合使用
  - 情报生产
    - MITRE 开发了==开源威胁情报提取工具TRAM==，用于自动从英文的分析报告中解析文件内容中存在的攻击行为，映射到对应的 ATT&CK 技术项

- ATT&CK 框架典型应用场景：检测分析

- ATT&CK 框架典型应用场景：攻击模拟

  - ATT&CK 可以应用在攻击模拟（Adversary Emulation）场景中，通过模拟真实攻击者的攻击行为进行安全测试

    ![image-20211123152526889](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123152526889.png)

- ATT&CK 框架典型应用场景：评估测试

### ATT&CK 在攻击事件关联分析中的实践

![image-20211123152947316](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123152947316.png)

1. 网空威胁映射 - ATT&CK 技术项标定

   - 依据攻击技术特征构建 ATT&CK 技术项映射规则，从恶意样本与网络行为两方面进行威胁标记

2. 多源数据运营

   - 网空实体与关系：利用**威胁数据**（报文告警、文件深度分析）与**情报数据**（威胁情报）作为起点 ，筛选重点关注的威胁或重点关注的目标相关的数据，与各种网络痕迹数据相关联，汇总得到网空实体与实体在网络空间中的关联关系；

     ![image-20211123153743496](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123153743496.png)

   - 数据处理流转：提取重点单位和威胁情报相关的网络实体属性及关联关系，支持基于实体关系图数据快速发现威胁事件，及时获取事件脉络，为攻击场景还原和受害者判定提供决策依据。  

     ![image-20211123153753154](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123153753154.png)

3. 攻击场景还原 - 攻击过程还原  

   ![image-20211123153947133](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123153947133.png)

4. 组织威胁狩猎 - TTP 描述  

   - 通过将掌握的攻击组织常用技战术手段从低层的 IOC 类威胁指标扩展到高层的 TTP 类，这些更稳定、更不易改变的攻击行为特征可以在 ATT&CK 框架的统一描述下帮助支撑威胁狩猎  

## 如何建立企业内部的ATT&CK  

*程度*

*青藤云安全COO*

**ATT&CK模型关系图**

![](https://gitee.com/misite_J/blog-img/raw/master/img/ATT&CK%E6%A8%A1%E5%9E%8B%E5%85%B3%E7%B3%BB%E5%9B%BE.png)

**企业设计自己的ATT&CK**  

![image-20211123155635243](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123155635243.png)

- ATT&CK Workbench项目

  ![image-20211123155712836](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123155712836.png)

  - 创建红队技术，以便像现有的 ATT&CK 技术一样
  - 记录针对企业或组织以及软件
  - 更新 ATT&CK 数据以内部、专有或其他报告
  - 使用 ATT&CK 知识库范围之外的新技术和策略开发企业矩阵  

**ATT&CK 运营的十种方式  **

- 威胁情报：抽取相关TTP,更新ATT&CK矩阵

- 模拟攻击：模拟攻击TTP或者组织,提高ATT&CK覆盖率  

- 合规：降低合规压力，在ATT&CK运营中进行合规  

  - ATT&CK 和PCI DSS映射
  - ATT&CK 和NIST 800-53映射

- 安全分析师训练：以ATT&CK框架作为培训基础，提高员工安全能力  

  ![image-20211123160321080](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123160321080.png)

- 威胁狩猎：使用ATT&CK框架落地，增加高级安全分析能力  

- 安全风险管理和规划：对数据质量和分析能力进行评估，找出安全差距  

- 安全控制合理性：利用ATT&CK能力覆盖，评估合理的安全控制  

- 安全投资评估：根据ATT&CK检测能力建设，合理安全投资

- 安全有效性评估：持续进行安全验证，进行安全有效性评估 

- 业务使能：对于新上线业务和新收购公司进行安全评估  



## ATT&CK威胁检测技术在云工作负载的实践

*陈俊杰*

*安全狗*

ATT&CK可用于评估、 强化安全产品的能力。我们实验室的研究人
员在实践中总结出了“**基于ATT&CK--》模拟红蓝对抗--》构建攻防图谱--》确定数据源--》确定威胁检测算法**” 的方法和经验。

![image-20211123170029136](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123170029136.png)

- 模拟红蓝对抗

  ![image-20211123173352314](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123173352314.png)

  ![image-20211123173402555](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123173402555.png)

- 基于“模拟红蓝对抗”构建ATT&CK“攻防图谱”

  ![image-20211123173338618](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123173338618.png)

- 基于“攻防图谱”确定“数据源”

  ![image-20211123173319960](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123173319960.png)

- 基于“数据源”确定“威胁检测算法”

  ![image-20211123173254864](https://gitee.com/misite_J/blog-img/raw/master/img/image-20211123173254864.png)



## 安全运营中ATT&CK框架的实用性挑战与应对

*张润滋*

*绿盟科技 天枢实验室*

### ATT&CK框架带来新机遇

- 数据归一化、本体化及关联性提升  
  - ATT&CK矩阵为企业或组织内数据湖的数据融合提供了**技战术抽象层次的对齐方案**；
  - 数据中隐含的数据实体及其关联关系，能够在**统一的框架下实现本体化建模**， 为知识图谱等基于网络和图的数据结构的构建提供基础。
- 促进分析能力与业务的解耦
  - 通用算法能力能够从传统的数据分析孤岛中抽象出来，并与上一层的安全业务需求解耦
- 促进分析算法的语义化
  - ATT&CK通过矩阵的战术阶段划分，在目标层、分析层以及数据层上自然的提供了有明确语义的关联关系

### 安全运营中ATT&CK实用性挑战

1. 采集与分析系统瓶颈
2. 数据隐私风险
3. 召回模型与高误报率
4. 一词多义现象
5. 信息流依赖爆炸现象

### ATT&CK相关技术实战化展望

- 去中心化架构
- 人机协同的分析机制
- 实战化攻防模拟
  - ATT&CK衔接和凝聚了攻防两端，通过实战化、原子化、模板化攻防模拟习得数据与攻防行为之间的映射关系；
  - 大型的、标准的、取得共识的统一评测，促进数据的标准化与兼容性提升。
  - ==Atomic Red Team-==
- 可运营的智能分析手段
  - 通过**因果建模、上下文建模与可解释性提升**，构建可运营的分析策略闭环，缓解信息流的爆炸；
  - 构建**超融合的知识图谱**，提升上下文富化的自动化水平。

探索更完备的攻防知识元语言。 ATT&CK适度抽象，但是仍然缺乏对标准化、规范化检测的指导意义， **需从规范的攻防靶场模拟推动攻防知识的扩展与统一**。  

