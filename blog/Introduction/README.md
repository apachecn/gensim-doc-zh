# 介绍

Gensim是一个[免费的](https://radimrehurek.com/gensim/intro.html#availability) Python库，旨在从文档中自动提取语义主题，尽可能高效（计算机方面）和 painlessly（人性化）。

Gensim旨在处理原始的非结构化数字文本（**`纯文本`**）。

在Gensim的算法，比如[`Word2Vec`](https://radimrehurek.com/gensim/models/word2vec.html#gensim.models.word2vec.Word2Vec "gensim.models.word2vec.Word2Vec")，[`FastText`](https://radimrehurek.com/gensim/models/fasttext.html#gensim.models.fasttext.FastText "gensim.models.fasttext.FastText")，潜在语义分析（LSI，LSA，see [`LsiModel`](https://radimrehurek.com/gensim/models/lsimodel.html#gensim.models.lsimodel.LsiModel "gensim.models.lsimodel.LsiModel")），隐含狄利克雷分布（LDA，见[`LdaModel`](https://radimrehurek.com/gensim/models/ldamodel.html#gensim.models.ldamodel.LdaModel "gensim.models.ldamodel.LdaModel")）等，自动训练文档的躯体内检查统计共生模式发现的文件的语义结构。这些算法是**无监督的**，这意味着不需要人工输入 - 您只需要一个纯文本文档。

一旦找到这些统计模式，任何纯文本文档（句子，短语，单词......）都可以在新的语义表示中简洁地表达，并查询与其他文档（单词，短语......）的主题相似性。

> 注意
如果前面的段落让您感到困惑，您可以在Wikipedia上阅读有关[向量空间模型](https://en.wikipedia.org/wiki/Vector_space_model)和[无监督文档分析的](https://en.wikipedia.org/wiki/Latent_semantic_indexing)更多信息。

## 功能

* **内存独立性** - 任何时候都不需要整个训练语料库完全驻留在RAM中（可以处理大型的Web级语料库）。
* **内存共享** - 经过训练的模型可以持久保存到磁盘并通过[mmap](https://en.wikipedia.org/wiki/Mmap)加载回来。多个进程可以共享相同的数据，从而减少RAM占用空间。
* 一些流行的向量空间算法的高效实现，包括[`Word2Vec`](https://radimrehurek.com/gensim/models/word2vec.html#gensim.models.word2vec.Word2Vec "gensim.models.word2vec.Word2Vec")，[`Doc2Vec`](https://radimrehurek.com/gensim/models/doc2vec.html#gensim.models.doc2vec.Doc2Vec "gensim.models.doc2vec.Doc2Vec")，[`FastText`](https://radimrehurek.com/gensim/models/fasttext.html#gensim.models.fasttext.FastText "gensim.models.fasttext.FastText")，TF-IDF，潜在语义分析（LSI，LSA，见[`LsiModel`](https://radimrehurek.com/gensim/models/lsimodel.html#gensim.models.lsimodel.LsiModel "gensim.models.lsimodel.LsiModel")），隐含狄利克雷分布（LDA，见[`LdaModel`](https://radimrehurek.com/gensim/models/ldamodel.html#gensim.models.ldamodel.LdaModel "gensim.models.ldamodel.LdaModel")）或随机投影（见[`RpModel`](https://radimrehurek.com/gensim/models/rpmodel.html#gensim.models.rpmodel.RpModel "gensim.models.rpmodel.RpModel")）。
* 来自几种流行数据格式的I / O包装器和读卡器。
* 在语义表示中对文档进行快速相似性查询。

Gensim背后的**主要设计目标**是：

1. 为开发人员提供简单的接口和低API学习曲线。适合原型设计。
2. 记忆独立性与输入语料库的大小有关; 所有中间步骤和算法都以流式方式运行，一次访问一个文档。

也可以看看

我们还为NLP，文档分析，索引，搜索和集群构建了一个高性能的商业服务器：[https](https://scaletext.ai/)：[//scaletext.ai](https://scaletext.ai/)。ScaleText既可以在本地使用，也可以作为SaaS使用。

到达 info@scaletext.com 如果你需要专业支持的工业级NLP工具。

## 可用性

Gensim根据OSI批准的[GNU LGPLv2.1许可证授权](https://www.gnu.org/licenses/old-licenses/lgpl-2.1.en.html)，可以从其[Github存储库](https://github.com/piskvorky/gensim/) 或[Python Package Index下载](https://pypi.python.org/pypi/gensim)。

也可以看看

有关Gensim部署的更多信息，请参阅[安装](https://radimrehurek.com/gensim/install.html)页面。

## 核心概念

> 文集

数字文档的集合。Corpora在Gensim担任两个角色：

1. 模型训练的输入语料库用于自动训练机器学习模型，例如 [`LsiModel`](https://radimrehurek.com/gensim/models/lsimodel.html#gensim.models.lsimodel.LsiModel "gensim.models.lsimodel.LsiModel")或[`LdaModel`](https://radimrehurek.com/gensim/models/ldamodel.html#gensim.models.ldamodel.LdaModel "gensim.models.ldamodel.LdaModel")。

    模型使用此*培训语料库*来查找共同的主题和主题，初始化其内部模型参数。

    Gensim专注于*无监督*模型，因此无需人工干预，例如昂贵的注释或手工标记文件。

2. 要组织的文件。训练之后，可以使用主题模型从新文档中提取主题（培训语料库中未见的文档）。

    这样的语料库可以通过语义相似性，聚类等进行[索引](https://radimrehurek.com/gensim/tut3.html)，查询。

> 向量空间模型

在向量空间模型（VSM）中，每个文档由一系列要素表示。例如，单个功能可以被视为问答配对：

1. 单词*splonge*在文档中出现了多少次？零。
2. 该文件包含多少段？二。
3. 该文档使用了多少种字体？五。

问题通常只能由它的一个整数标识符（如所表示1，2和3在此），因此，该文件的表示变得一系列像`(1, 0.0), (2, 2.0), (3, 5.0)` 对。

如果我们提前知道所有问题，我们可能会隐瞒并简单地写成 `(0.0, 2.0, 5.0)`。

该答案序列可以被认为是 **向量(vector)**（在这种情况下是三维密集矢量）。出于实际目的，Gensim中只允许答案（或可以转换为）*单个浮点数的问题*。

每个文档的问题都是相同的，所以看两个向量（代表两个文档），我们希望能够得出结论，例如“这两个向量中的数字非常相似，因此原始文档必须类似“也是”。当然，这些结论是否与现实相符取决于我们选择问题的程度。

> Gensim 稀疏向量，Bag-of-words 向量

为了节省空间，在Gensim中我们省略了值为0.0的所有向量元素。例如，我们只写（注意缺失的）而不是三维密集向量。每个向量元素是一对（2元组）。此稀疏表示中所有缺失特征的值可以明确地解析为零。`(0.0,<span> 2.0,<span> 5.0)``[(2,<span> 2.0),<span> (3,<span> 5.0)]``(1,<span> 0.0)``(feature_id,feature_value)``0.0`

Gensim中的文档由稀疏向量（有时称为词袋向量）表示。

> Gensim流式语料库

Gensim没有规定任何特定的语料库格式。语料库只是一个稀疏向量序列（见上文）。

例如 `[ [(2, 2.0), (3, 5.0)], [(3, 1.0)] ]` 是两个文档的简单语料库=两个稀疏向量：第一个具有两个非零元素，第二个具有一个非零元素。这个特定的语料库表示为普通的Python 。

然而，Gensim的全部功能来自于语料库不必是a `list`，或`NumPy`数组，或`Pandas`数据帧等等。Gensim *接受任何对象，当迭代时，连续产生这些稀疏的袋子向量*。

这种灵活性允许您创建自己的语料库类，直接从磁盘，网络，数据库，数据帧...流式传输稀疏向量。实现Gensim中的模型，使得它们不需要所有向量一次驻留在RAM中。你甚至可以动态创建稀疏矢量！

请参阅我们的[Python流数据处理教程](https://rare-technologies.com/data-streaming-in-python-generators-iterators-iterables/)。

有关直接从磁盘流式传输的高效语料库格式的内置示例，请参阅中的Matrix Market格式[`mmcorpus`](https://radimrehurek.com/gensim/corpora/mmcorpus.html#module-gensim.corpora.mmcorpus "gensim.corpora.mmcorpus：矩阵市场格式的语料库")。有关如何创建自己的流式语料库的最小蓝图示例，请查看[CSV语料库](https://github.com/RaRe-Technologies/gensim/blob/develop/gensim/corpora/csvcorpus.py)的[源代码](https://github.com/RaRe-Technologies/gensim/blob/develop/gensim/corpora/csvcorpus.py)。

> 模型，转型

Gensim使用**模型**来引用将一个文档表示转换为另一个文档表示所需的代码和相关数据（模型参数）。

在Gensim中，文档被表示为向量（见上文），因此模型可以被认为是从一个向量空间到另一个向量空间的转换。从训练语料库中学习该变换的参数。

训练有素的模型（数据参数）可以持久保存到磁盘，然后加载回来，以继续培训新的培训文档或转换新文档。

Gensim实现多种模式，如[`Word2Vec`](https://radimrehurek.com/gensim/models/word2vec.html#gensim.models.word2vec.Word2Vec "gensim.models.word2vec.Word2Vec")， [`LsiModel`](https://radimrehurek.com/gensim/models/lsimodel.html#gensim.models.lsimodel.LsiModel "gensim.models.lsimodel.LsiModel")，[`LdaModel`](https://radimrehurek.com/gensim/models/ldamodel.html#gensim.models.ldamodel.LdaModel "gensim.models.ldamodel.LdaModel")， [`FastText`](https://radimrehurek.com/gensim/models/fasttext.html#gensim.models.fasttext.FastText "gensim.models.fasttext.FastText")等见[API参考](https://radimrehurek.com/gensim/apiref.html)的完整列表。

也可以看看

有关如何在代码中解决所有问题的一些示例，请转到[教程](https://radimrehurek.com/gensim/tutorial.html)。
