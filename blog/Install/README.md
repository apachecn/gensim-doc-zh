# 安装

## 快速安装

在您的终端中运行（推荐）：

`pip install --upgrade gensim`

或者，对于conda环境：

`conda install -c conda-forge gensim`

而已！恭喜，您可以继续学习本[教程](https://radimrehurek.com/gensim/tutorial.html)。

如果失败，请确保您正在安装到可写位置（或使用sudo）。

---

## 代码依赖

Gensim在Linux，Windows和Mac OS X上运行，并且应该在支持Python 2.7+和NumPy的任何其他平台上运行。Gensim取决于以下软件：

* [Python](https://www.python.org/) >= 2.7 (tested with versions 2.7, 3.5 and 3.6)
* [NumPy](http://www.numpy.org/) >= 1.11.3
* [SciPy](https://www.scipy.org/) >= 0.18.1
* [Six](https://pypi.org/project/six/) >= 1.5.0
* [smart_open](https://pypi.org/project/smart_open/) >= 1.2.1

## 测试Gensim

Gensim使用持续集成，在每个pull请求上自动运行完整的测试套件

| CI service | Task | Build badge |
| :-- | :-- | :-- |
| Travis | 在Linux上运行测试并检查[代码样式](https://www.python.org/dev/peps/pep-0008/?) | ![Travis](/imgs/Introduction/gensim.svg) |
| AppVeyor | 在Windows上运行测试 | ![AppVeyor](/imgs/Introduction/develop_1.svg) |
| CircleCI | 构建文档 | ![CircleCI](/imgs/Introduction/develop.svg) |

## 问题

使用[Gensim讨论组](https://groups.google.com/group/gensim/)进行问题和故障排除。有关商业支持，请参阅[支持页面](https://radimrehurek.com/gensim/support.html)。
