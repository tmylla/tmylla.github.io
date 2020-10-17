---
layout: post
title: (QA paper解读)Graph-Based Reasoning over Heterogeneous External Knowledge for Commonsense QA
categories: DailyPaper
description: GNN_CommonsenseQA_AAAI2020
keywords: paper
---



> Graph-Based Reasoning over Heterogeneous External Knowledge for Commonsense Question Answering
>
> AAAI2020；[PDF](http://kcsg.net/papers/1_lv_AAAI2020.pdf)



# Abstract

常识问答是指需要一些背景知识/常识经验的问答，一般采用的方法包括：人工标注进行学习；从结构化或非结构化数据源选择相关信息支撑回答。本文结合结构化知识库ConceptNet和非结构化知识库Wikipedia在CommonsenseQA数据集上进行常识问答。具体而言，首先从不同源抽取问句相关的知识组成图，然后使用基于图网络的方法选择正确答案，包括：结合拓扑结构信息的上下文编码，GCN消息传递与聚合从而更新节点编码，以及结合图注意力机制的正确答案选择。

# Introduce

常识问答需要两个条件：一是问句相关的背景信息/知识（The evidence），二是根据相关信息选择正确答案（The principle）。过去的方法或多或少缺少一些条件，因此本文结合知识图谱ConceptNet和文本库Wikipedia两个数据源进行常识问答，之所以做出这样的选择原因见下图。这样，就可以从这两种不同类型的数据源获取支持信息，以类似“投票”的方式选择复核不同数据源的答案，避免单一数据源偏差。面向CommonsenseQA数据集（一个多项选择问答数据集），提出基于图方法的模型。

![](https://gitee.com/misite_J/blog-img/raw/master/img/2020-10-15_01.png)

# Approach 

![](https://gitee.com/misite_J/blog-img/raw/master/img/2020-10-15_02.png)

上图直观表现了本文方法，即，（知识抽取）根据问句和答案选项从不同源数据库抽取相关信息，组成图形式；（图推理）应用图网络方法解析图，选择正确答案。

## 知识抽取

1. 从ConceptNet抽取相关知识

   根据问句和候选答案在ConceptNet中选择相应实体，然后选择从问句实体到答案实体的三跳以内的路径，合并得到问答相关子图（Concept-Graph）。为后续处理，进一步根据预定义的关系模板将三元组转化为自然语言序列。

2. 从Wikipedia抽取相关知识

   去问句和选项中的提用词，拼接分词结果，据此在Elastic Search中选择K=10个匹配度最高的句子。根据这些句子，使用Semantic Role Labeling (SRL) 抽取每个句子中predicate对应的(subjective, objective) ，以三元组形式构成图（Wiki-Graph）。

## 图推理

![](https://gitee.com/misite_J/blog-img/raw/master/img/2020-10-15_03.png)

1. 基于图的词上下文表示

   该模块结合预训练语言模型XLNet和图结构信息获取更好的上下文单词表示。注意，此处使用的图信息并非直接前一阶段获取的Concept-Graph和Wiki-Graph，而是构建句子图，即以句子为节点的图：对Wiki-Graph，如果两个句子分别包含的实体p和q在Wiki-Graph之间有连接，则在这两个句子添加一条边，如此构成Wiki句子图；至于Concept-Graph，前面提到根据预定义的关系模板将三元组转化为自然语言序列，进行同样的处理得到Concept句子图。面向句子图，作者提出拓扑排序算法对句子关联性进行排序。将排序的Concept句子、Wiki句子、问句和答案选项输入给XLNet，输出新的上下文词向量。**这一部分的高明之处在于通过将前一阶段获得的实体子图重新构建为句子图，这样不同句子之间的关联直观的显示在拓扑结构上。讲道理的说，模型其他部分的结构很多人也能够想到，但这部分基于图结构对词向量的重新嵌入实在是高明，推荐看原文**。

2. 图推理：GCN图节点表示+Graph-Attention选择答案

   首先使用GCN对Concept-Graph和Wiki-Graph中的节点进行编码，然后使用图点积注意力实现答案选择。该模块原理即以下五个公式，结合模型图还是容易明白的，难点是代码实现Orzzz...

   ![](https://gitee.com/misite_J/blog-img/raw/master/img/2020-10-15_04.png)

# Experiments

数据集：CommonsenseQA，常识问答经典数据集。

实验结果：与其他模型的比较见下图，说实话效果真的很好。

![](https://gitee.com/misite_J/blog-img/raw/master/img/2020-10-15_05.png)

Ablation Study ：

![](https://gitee.com/misite_J/blog-img/raw/master/img/2020-10-15_06.png)

Case Study

Error Analysis

# Related Work

- 常识推理研究
- 预训练语言模型进展
- GNN在NLP中的应用

# Conclusion

未来改进方向：

- 融合更多知识源
- 推理模块的性能提升（本质是提高模型推理能力，不同应用中均应关注）