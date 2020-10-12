# 利用 AWS Sagemaker BlazingText 对不均衡文本进行多分类

## 背景
文本分类(Text Classification) 属于自然语言处理领域，是指计算机将载有信息的一篇文本映射到预先给定的某一类别或某几类别主题的过程。然而在现实问题中，经常会遇到数据样本的类别不平衡 (class imbalance) 现象，严重影响了文本分类的最终结果。所谓样本不均衡指的是给定数据集中有的类别数据多，有的数据类别少，且数据占比多的数据类别样本与占比小的数据类别样本两者之间达到较大的比例。

BlazingText 是 AWS Sagemaker 的一个内置算法，提供了 Word2vec 和文本分类算法的高度优化的实现。本文使用了 Sagemaker  BlazingText 实现了文本多分类。在样本不均衡问题上，使用了回译和 EDA 两个方法对少类别样本进行了过采样处理，其中回译方法调用了 AWS Translate 服务进行了翻译再翻译，而 EDA 方法主要使用同义词替换、随机插入、随机交换、随机删除对文本数据进行处理。 本文也使用了AWS Sagemaker 的自动超参数优化来为 BlazingText 的文本分类算法找到最优超参数。

本文使用基于 DBpedia 的公开数据集处理生成的含有14个类别的不均衡文本数据，并进行了不做任何样本不均衡处理的 Baseline 实验和包含回译和 EDA 两个方法的过采样实验。

在这个案例中，使用的数据集是根据文章的标题和摘要进行作者的多分类。然而，本文提出的文本分类方法适用于任何样本不均衡的文本分类场景，比如：

- 垃圾邮件分类，实际数据中只有少部分邮件是垃圾邮件；
- 根据诊断报告疾病预测，真实的临床数据中也只有少部分数据真的患有某种疾病；
- 新闻分类，对网站的大量新闻进行分类判断是属于经济的，还是文化的等，但是不同网站类别偏差较大，比如娱乐网站娱乐新闻偏多，但是也会有其他类别的新闻。

## 文件说明
- generateUnbalancedData.ipynb: 从公开数据集 dbpedia 中生成的不均衡数据集

- text_multi_classification_oversampling.ipynb: 经过样本不均衡处理的过采样实验
- text_multi_classification_baseline.ipynb: 没有经过样本不均衡处理的 baseline 实验

- text_multi_classification_oversampling_hyperparameter.ipynb: 经过样本不均衡处理的过采样实验(超参数优化)
- text_multi_classification_baseline_hyperparameter.ipynb: 没有经过样本不均衡处理的 baseline 实验(超参数优化)