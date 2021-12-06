---
layout: post
title: transformers库学习笔记之一--安装与测试
categories: [transformers, DL]
description: transformers库学习笔记系列第一篇
keywords: keyword1, keyword2
---




> 印象中觉得transformers是一个庞然大物，但实际接触后，却是极其友好，感谢huggingface大神。

​	



## 安装

我的版本号：python 3.6.9；pytorch 1.2.0；CUDA 10.0。

```python
pip install transformers
```

pip之前确保安装pytorch1.1.0+。

![](https://gitee.com/misite_J/blog-img/raw/master/img/pip install transformers.png)

​	

## 测试

### 验证代码与结果

`python -c "from transformers import pipeline; print(pipeline('sentiment-analysis')('I hate you'))"`

在命令行输入如上命令后，transformers会自动下载依赖模型。输出以下结果，安装成果。

`[{'label': 'NEGATIVE', 'score': 0.9991129040718079}]`

​	

### transformer pipeline下载模型文件说明

transformers自动下载模型的保存位置：C:\Users\username\\.cache\torch\，在模型下载以后，可以保存到其他位置。各文件的说明如下：

![](https://gitee.com/misite_J/blog-img/raw/master/img/情感分析pipeline model.png)

1. json文件包含对应文件的‘url’和‘etag’标签。

   ![](https://gitee.com/misite_J/blog-img/raw/master/img/json示例.png)

2. ‘a41...’为配置文件：distilbert-base-uncased-config。

   ![](https://gitee.com/misite_J/blog-img/raw/master/img/example1.png)

3. ‘26b...’为词典文件：bert-base-uncased-vocab。

   ![](https://gitee.com/misite_J/blog-img/raw/master/img/example2.png)

4. ‘437...’为finetuned-sst-2的配置文件：distilbert-base-uncased-finetuned-sst-2-english-config，注意其与‘a41...’文件的不同。

   ![](https://gitee.com/misite_J/blog-img/raw/master/img/example3.png)

5. ‘57d...’为Modelcard文件：distilbert-base-uncased-finetuned-sst-2-english-modelcard。

   ![](https://gitee.com/misite_J/blog-img/raw/master/img/example4.png)

6. ‘dd7...’为模型参数文件：distilbert-base-uncased-finetuned-sst-2-english-pytorch_model.bin。



​	

### pipeline()简介

可以看到，通过执行`pipeline('sentiment-analysis')('I hate you')`，transformers自动下载GLUE中sst2数据集的distilbert-base-uncased-finetuned-sst-2模型，对'I hate you'进行情感分析。

Pipeline是一个简捷的NLP任务接口，执行*Input -> Tokenization -> Model Inference -> Post-Processing (Task dependent) -> Output*一系列操作。目前支持*Named Entity Recognition, Masked Language Modeling, Sentiment Analysis, Feature Extraction and Question Answering*等任务。

以Question Answering为例：

```python
from transformers import pipeline

nlp = pipeline("question-answering")

context = "Extractive Question Answering is the task of extracting an answer from a text given a question. An example of a question answering dataset is the SQuAD dataset, which is entirely based on that task. If you would like to fine-tune a model on a SQuAD task, you may leverage the `run_squad.py`."

print(nlp(question="What is extractive question answering?", context=context))
print(nlp(question="What is a good example of a question answering dataset?", context=context))
```

对QA任务，transformers使用SQuAD数据集的distilbert-base-cased-distilled-squad模型，模型文件同上文介绍。

![](https://gitee.com/misite_J/blog-img/raw/master/img/example5.png)

​	

### 移动模型到自定义文件夹

![](https://gitee.com/misite_J/blog-img/raw/master/img/移动模型到自定义文件夹.png)

以QA为例：

1. 首先我们建立一个文件夹，命名为distilbert-base-cased-distilled-squad，然后将词典文件、模型配置文件、模型参数文件三个文件放入这个文件夹，并且将文件重命名为config.json、vocab.txt、pytorch_model.bin即可。

2. 在代码中定义模型目录，`DISTILLED = './distilbert-base-cased-distilled-squad'`，完整代码如下。

   ```python
   from transformers import AutoTokenizer, AutoModelForQuestionAnswering
   import torch
   
   DISTILLED = './distilbert-base-cased-distilled-squad'
   tokenizer = AutoTokenizer.from_pretrained(DISTILLED)
   model = AutoModelForQuestionAnswering.from_pretrained(DISTILLED)
   
   text = """
   Transformers (formerly known as pytorch-transformers and pytorch-pretrained-bert) provides general-purpose
   architectures (BERT, GPT-2, RoBERTa, XLM, DistilBert, XLNet…) for Natural Language Understanding (NLU) and Natural
   Language Generation (NLG) with over 32+ pretrained models in 100+ languages and deep interoperability between
   TensorFlow 2.0 and PyTorch.
   """
   
   questions = [
       "How many pretrained models are available in Transformers?",
       "What does Transformers provide?",
       "Transformers provides interoperability between which frameworks?",
   ]
   
   for question in questions:
       inputs = tokenizer.encode_plus(question, text, add_special_tokens=True, return_tensors="pt")
       input_ids = inputs["input_ids"].tolist()[0]
   
       text_tokens = tokenizer.convert_ids_to_tokens(input_ids)
       answer_start_scores, answer_end_scores = model(**inputs)
   
       answer_start = torch.argmax(answer_start_scores)  # Get the most likely beginning of answer with the argmax of the score
       answer_end = torch.argmax(answer_end_scores) + 1  # Get the most likely end of answer with the argmax of the score
   
       answer = tokenizer.convert_tokens_to_string(tokenizer.convert_ids_to_tokens(input_ids[answer_start:answer_end]))
   
       print(f"Question: {question}")
       print(f"Answer: {answer}\n")
   ```

   

​	

## 参考

[https://huggingface.co/transformers/installation.html](https://huggingface.co/transformers/installation.html)