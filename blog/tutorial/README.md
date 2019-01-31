# 教程

这些教程被组织为一系列示例，突出了Gensim的各种功能。假设读者熟悉[Python语言](https://www.python.org/)，[安装了gensim](/blog/Install/README.md) 并阅读了[介绍](/blog/Introduction/README.md)。

这些例子分为以下部分：


*   [语料库和向量空间](blog/tutorial/1.md)
    *   从字符串到向量
    *   语料库流 - 一次一个文档
    *   语料库格式
    *   与NumPy和SciPy的兼容性
*   [主题和转换](blog/tutorial/2.md)
    *   转换界面
    *   可用的转换
*   [相似性查询](blog/tutorial/3.md)
    *   相似界面
    *   下一个在哪里
*   [英语维基百科上的实验](blog/tutorial/4.md)
    *   准备语料库
    *   潜在语义分析
    *   潜在的Dirichlet分配
*   [分布式计算](blog/tutorial/5.md)
    *   为何分布式计算？
    *   先决条件
    *   核心概念
    *   可用分布式算法

## 预赛

所有示例都可以直接复制到Python解释器shell。[IPython](http://ipython.scipy.org/)的 `cpaste` 命令对于复制代码片段（包括主要 `>>>>` 字符）特别方便。

Gensim使用Python的标准 `logging` 模块来记录各种优先级的各种东西; 要激活日志记录（这是可选的），请运行

```py
>>> import logging
>>> logging.basicConfig(format='%(asctime)s : %(levelname)s : %(message)s', level=logging.INFO)
```

## 快速示例

首先，让我们导入gensim并创建一个包含九个文档和十二个特征的小型语料库[[1]](https://radimrehurek.com/gensim/tutorial.html#id2)：

```py
>>> from gensim import corpora, models, similarities
>>>
>>> corpus = [[(0, 1.0), (1, 1.0), (2, 1.0)],
>>>           [(2, 1.0), (3, 1.0), (4, 1.0), (5, 1.0), (6, 1.0), (8, 1.0)],
>>>           [(1, 1.0), (3, 1.0), (4, 1.0), (7, 1.0)],
>>>           [(0, 1.0), (4, 2.0), (7, 1.0)],
>>>           [(3, 1.0), (5, 1.0), (6, 1.0)],
>>>           [(9, 1.0)],
>>>           [(9, 1.0), (10, 1.0)],
>>>           [(9, 1.0), (10, 1.0), (11, 1.0)],
>>>           [(8, 1.0), (10, 1.0), (11, 1.0)]]
```

在Gensim中，*语料库*只是一个对象，当迭代时，返回其表示为稀疏向量的文档。在这种情况下，我们使用元组列表的列表。如果您不熟悉[矢量空间模型](https://en.wikipedia.org/wiki/Vector_space_model)，我们将在下一个关于[Corpora和Vector Spaces的](https://radimrehurek.com/gensim/tut1.html)教程中弥合**原始字符串**，**语料库**和**稀疏矢量**之间的差距。[](https://radimrehurek.com/gensim/tut1.html)

如果您熟悉向量空间模型，您可能会知道解析文档并将其转换为向量的方式会对任何后续应用程序的质量产生重大影响。

> 注意:
在此示例中，整个语料库作为Python列表存储在内存中。但是，语料库接口只表示语料库必须支持对其组成文档的迭代。对于非常大的语料库，有利的是将语料库保持在磁盘上，并且一次一个地顺序访问其文档。所有操作和转换都以这样的方式实现，使得它们在内存方面独立于语料库的大小。

接下来，让我们初始化一个*转换*：

```py
>>> tfidf = models.TfidfModel(corpus)
```

转换用于将文档从一个向量表示转换为另一个向量表示：

```py
>>> vec = [(0, 1), (4, 1)]
>>> print(tfidf[vec])
[(0, 0.8075244), (4, 0.5898342)]
```

在这里，我们使用了[Tf-Idf](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)，这是一种简单的转换，它将文档表示为词袋计数，并应用对常用术语进行折扣的权重（或者等同于促销稀有术语）。它还将得到的向量缩放到单位长度（在[欧几里德范数中](https://en.wikipedia.org/wiki/Norm_%28mathematics%29#Euclidean_norm)）。

[主题和转换](https://radimrehurek.com/gensim/tut2.html)教程中详细介绍了[转换](https://radimrehurek.com/gensim/tut2.html)。

要通过TfIdf转换整个语料库并对其进行索引，以准备相似性查询：

```py
>>> index = similarities.SparseMatrixSimilarity(tfidf[corpus], num_features=12)
```

并查询我们的查询向量`<span class="pre">vec</span>`与语料库中每个文档的相似性：

```py
>>> sims = index[tfidf[vec]]
>>> print(list(enumerate(sims)))
[(0, 0.4662244), (1, 0.19139354), (2, 0.24600551), (3, 0.82094586), (4, 0.0), (5, 0.0), (6, 0.0), (7, 0.0), (8, 0.0)]
```

如何阅读此输出？文档编号为零（第一个文档）的相似度得分为0.466 = 46.6％，第二个文档的相似度得分为19.1％等。

因此，根据TfIdf文档表示和余弦相似性度量，最类似于我们的查询文档vec是文档号。3，相似度得分为82.1％。请注意，在TfIdf表示中，任何不具有任何共同特征的 `vec` 文档（文档编号4-8）的相似性得分均为0.0。有关更多详细信息，请参阅[Similarity Queries](https://radimrehurek.com/gensim/tut3.html)教程。

---

> [1] 这与 [Deerwester等人](http://www.cs.bham.ac.uk/~pxt/IDA/lsa_ind.pdf) 使用的语料库相同[。](http://www.cs.bham.ac.uk/~pxt/IDA/lsa_ind.pdf)[（1990）：通过潜在语义分析进行索引](http://www.cs.bham.ac.uk/~pxt/IDA/lsa_ind.pdf)，表2。
