---
layout: post
title: (NER经典之作BiLSTM-CRF解读)Bidirectional LSTM-CRF Models for Sequence Tagging
categories: DailyPaper
description: 2015年，首次应用BiLSTM-CRF解决NER问题
keywords: NER, BiLSTM-CRF
---



[TOC]

# Abstract

论文以LSTM为基础，对比LSTM、BiLSTM、CRF、LSTM-CRF和BiLSTM-CRF一系列序列标注模型，实验对比表明BiLSTM-CRF模型在序列标注任务中的极大优势（该模型在后续几年成为NER任务的标杆，几乎知道NER任务的人都有听说过BiLSTM-CRF，足以见其影响力）。模型名称（BiLSTM-CRF）直观显示了模型结构与优势，其中BiLSTM通过前向/后向传递的方式学习序列中某字符依赖的过去和将来的信息，CRF则考虑到标注序列的合理性。这些优势使得论文模型在当时取得SOTA结果。



# Introduction

序列标注是NLP经典任务，包括词性标注POS、语义组块Chunking、命名实体识别NER等。传统的机器学习方法包括隐马尔科夫模型HMM、最大熵马尔可夫模型MEMMs和条件随机场CRF，神经网络方法CNN、RNN也可用来解决该问题。论文设计一系列LSTM模型对比实验，包括BiLSTM、LSTM-CRF和BiLSTM-CRF。其中经典的BiLSTM-CRF方法就是在该论文首次提出。



# Models

论文该部分以NER任务为例，分5小节分别介绍LSTM、BiLSTM、CRF、LSTM-CRF和BiLSTM-CRF序列标注模型，读者用不着被这么多模型吓到，理解该部分的核心在于LSTM和CRF（如果你了解LSTM和CRF，直接看下面五张图即可了解该章节内容）。对LSTM和CRF不熟悉的推荐学习[命名实体识别（NER）：BiLSTM-CRF原理介绍+Pytorch_Tutorial代码解析](https://www.yanxishe.com/columnDetail/21153)，学习完后估计对论文该章节的内容不会再有疑问。

![](https://gitee.com/misite_J/blog-img/raw/master/img/2020-10-26_01.png)



# Training procedure 

以BiLSTM-CRF为例，训练过程如下图所示，其余模型与此类似。这一部分的介绍比较简陋，不过比起现在大多数论文理论知识、模型、实验分析的“三板斧”套路，仅在实验部分介绍模型参数。要想真正理解是如何操作/实践的，还是要看代码，该论文并未提供代码，读者可自行搜索“BiLSTM-CRF NER”相关代码阅读，几乎大同小异。

![](https://gitee.com/misite_J/blog-img/raw/master/img/2020-10-26_02.png)



# Experiments

## Data

- Penn TreeBank (PTB)  --> POS
- CoNLL 2000 --> chunking 
- CoNLL 2003 --> NER

## Features

特征工程有利于模型效果改善。尽管特征工程需要人工定义，当前DL研究趋势是尽可能的减少人工干预，实现智能全自动化，但在工业应用或是数据竞赛中，特征工程对于应用模型落地和比赛结果提升仍然至关重要，因此在此处对论文中提到的文本处理特征工程进行介绍，相信了解这些内容对于NLP研究会有一定帮助。

论文将文本特征分为拼写特征（见下图）和上下文特征（主要指N元模型n-gram）。

![](https://gitee.com/misite_J/blog-img/raw/master/img/2020-10-26_03.png)

**Features connection tricks**：对于如何合理使用特征工程得到的特征（拼写、上下文）和神经网络得到的嵌入特征（char-/word embedding），直接的想法是将其进行拼接，然后输入模型，当然多数模型就是这样做的。但论文作者提出，可以使用下图所示的方法，即仅仅**将神经网络的嵌入特征传递给BiLSTM，而特征工程的特征无需BiLSTM直接传递给CRF**，作者表示，这样做可以加快训练速度，同时并不会影响实验结果。当然要注意，拼写/上下文特征与BiLSTM的输出存在较大的维度差异，需要使用全连接方式进行适当处理。

![](https://gitee.com/misite_J/blog-img/raw/master/img/2020-10-26_04.png)



## Results

- POS：BiLSTM-CRF > LSTM-CRF > CRF$\approx$ BiLSTM $>$ LSTM
- Chunking：BiLSTM-CRF > LSTM-CRF > BiLSTM > CRF > LSTM
- NER：BiLSTM-CRF > LSTM-CRF > BiLSTM > CRF > LSTM

实验验证得到的其他一些经验：

- 相比之前的方法Conv-CRF，BiLSTM-CRF具有更小的词向量依赖性。BiLSTM-CRF中，使用Senna词向量较随机初始化词向量在POS/Chunking/NER任务分别提升0.12%/0.33%/4.57%，而Conv-CRF则分体提升0.92%/3.99%/7.20%。（同时可以得到，**对NER任务，使用预训练的词向量可有效改善模型性能**）
- CRF的表现严重依赖特征工程的好坏。
- 在训练标注任务上，BiLSTM-CRF较过去的方法更具优势。



