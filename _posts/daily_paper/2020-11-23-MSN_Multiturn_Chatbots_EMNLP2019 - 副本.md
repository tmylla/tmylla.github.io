---
layout: post
title: (Chatbot paper解读)Multi-hop Selector Network for Multi-turn Response Selection in Retrieval-based Chatbots
categories: DailyPaper
description: MSN_MultiturnChatbots_EMNLP2019
keywords: paper
---


> Multi-hop Selector Network for Multi-turn Response Selection in Retrieval-based Chatbots
>
> EMNLP2019; 	[PDF下载](http://kcsg.net/papers/7_yuan_EMNLP19.pdf)



[TOC]



# Abstract

在基于信息抽取的多轮对话中，现有方法过多使用多粒度级别上下文匹配候选响应，而忽略了过多信息的副作用，如噪声和非必要信息。为此，作者提出多跳选择网络(MSN，multi-hop selector network)，通过多跳选择器选择上下文相关表达，使用过滤后的信息进行匹配，从而实现性能提升。（More does not mean better）



# Introduction

众所周知，多轮对话系统是人工智能领域可能最现实、服务大众的应用。实现多轮对话一般包括生成式和检索式两种方法，在当前阶段，检索式方法的多轮对话能提供更流畅、优质的服务。

研究人员开展了大量工作，典型思路如在字符、词、短语、句子等不同层次上提取匹配特征，或是词间的短期/长期依赖。一般来说，信息越多，越有利于对话进行，但也带来噪声、冗余信息等问题，这容易让机器犯一些人类觉的不可思议的错误，毕竟要达到真正的认知智能任重道远。

作者试验发现，之前的方法在对抗样本情况下非常脆弱，于是提出本文的MSN方法。

## Related Work

- 生成式对话
- 检索式对话：研究过程都是从易到难，先是单轮对话，然后多轮对话。问题复杂一点，模型复杂一点，左加一点又加一点，曰创新，但真正的创新还就是在这样的发展趋势下横空出世。

# Model

## 问题定义

直接看下面的图吧。为完成图中描述的任务，我们需要解决两个问题：一是从前几轮的对话中选择决定下一句的相关性强的信息，因为我们之前说过，信息过多不一定是好事；二是对于筛选的信息，该如何整合表示，不然机器看都看不懂还怎么回话。

![](https://gitee.com/misite_J/blog-img/raw/master/img/2020-10-15_07.png)

## MSN模型架构

![](https://gitee.com/misite_J/blog-img/raw/master/img/2020-10-15_08.png)

- 模型输入前n轮对话和候选响应。

- 根据前n轮对话，注意力模块更新单词的上下文表示。

- 接下来是本文的重头戏：文本选择器，顾名思义，就是要在多轮对话中选择相关度高的合适的句子。可以看到图中有k个多跳选择器，**n跳选择器中的n是指根据前n个句子的信息对之前的多轮对话进行筛选（虽然复杂，但是很合理）**。以1跳选择器为例，仅依赖上一句话，论文从词和句子两个层次进行信息筛选，即Word Selector和Utterance Selector。其中，Word Selector在单词层级上获取最后一句话与其他语句之间的关联，这样的关联不存在语义信息，为此需要加上Utterance Selector，在此通过余弦相似度计算最后一句话与其他语句之间的相关度。这样，从单词和句子两个层次上分别得到相关度，加权平均后即得到前几轮各句子的不同重要性。如果上一句话信息很少，如“好的”“没问题”，这时就要多跳选择器了。

- 将k个选择器的相似度得分进行整合，选择得分较高的句子。

- 之后，将筛选得到的信息和候选响应进行匹配，论文给出三种匹配方法：Origin Matching、Self Matching和Cross Matching。根据前一阶段得到的筛选语句集和候选响应，Origin Matching是直接计算两者在词级别上的点积和余弦相似度；Self Matching如图中橙色线条所示，将多跳选择器的Context Fusion和候选响应再次进行注意力操作，注意，**由于经过MSN，此时的Context Fusion已经学习到不同句子之间的词相关性，如此的“回锅肉”操作是会为多轮对话的表示增加更丰富有用的信息，妙啊**！之后的操作同之前一样，计算点积和余弦相似度；Cross Matching与Self Matching基本相同，唯一的区别在于多轮对话和候选响应的注意力表示操作是相互之间的，作者说这样是为了学习相互之间的依赖关系，使表示更丰富。怎么说呢，个人觉得这一步的操作可有可无，似乎有点反直觉，在多轮对话中没有必要让之前若干轮的句子表示包含下一句的关联信息，文中也没有给出关于此操作的Ablation Study，但想必作者这样做也有自己的考虑吧。总之，三种匹配矩阵计算大致如下图。

  ![](https://gitee.com/misite_J/blog-img/raw/master/img/2020-10-16_01.png)

- 将上述三种相似度匹配拼接，应用2D CNN和最大池化操抽取配词级别匹配矩阵的特征，将特征输入给GRU，建模多轮对话的上下文时序关联。最后，应用单层感知机计算多轮与候选响应的得分。

# Experiments

以此介绍数据集、评估指标、Baseline模型、训练设置和实验结果，论文模型优于其他模型。感兴趣的建议直接阅读原文部分。

# Further Analysis

- Ablation Study：分别对比有无单词选择器，有无句子选择器，仅有1/2/3跳选择器，有无选择器。结论当然是有比没有好。
- Parameter Sensitivity：这一部分对论文重要的超参数进行设计，包括k跳选择器k个数的选择，结论是3个表现最好，过多过少表现都会下降；还有就是多轮对话筛选时相关度值门槛的设置，即相关度大于多少时认为该句子与下一句话的生成相关，实现给出的值为0.3/0.5。就像欠拟合和过拟合问题一样，这在深度学习中是一个永恒的主题，最优的、不大不小的那个参数你要能找到它。

# Conclusion

- 进一步研究多轮对话与候选响应的逻辑一致性问题

