.. _header-n2:

算法
====

机器学习常用算法数学公式

.. _header-n4:

机器学习算法
------------

.. _header-n6:

监督学习算法
~~~~~~~~~~~~

.. _header-n7:

一、朴素贝叶斯
^^^^^^^^^^^^^^

   请以\ **嫁与不嫁高富帅矮矬穷**\ 的分类做理解

   假设：每对特征之间相互\ **独立**\ ，
   很多情况下，朴素贝叶斯工作情况良好，特别是文本分类、文档分类、语言主题分类、垃圾邮件过滤

给定一个类别 :math:`y` 和一个从 :math:`x_1`\ 到 :math:`x_n`
的相关的特征向量， 贝叶斯定理阐述了以下关系，方程式如下：

.. math:: P(y \mid x_1, \dots, x_n) = \frac{P(y) P(x_1, \dots x_n \mid y)}{P(x_1, \dots, x_n)}  = \frac{P(y) P(x_1  \mid y) \dots P(x_n \mid y)}{P(x_1) \dots P(x_n)}

关系简化为如下：

.. math:: P(y \mid x_1, \dots, x_n) = \frac{P(y) \prod_{i=1}^{n} P(x_i \mid y)}{P(x_1, \dots, x_n)}]

在如上公式分类规则中，给定的如下分母\ :math:`P(x_1, \dots , x_n) = P(x_1) \dots P(x_n)`
的\ **分类**\ 算法中，

.. math:: {P(x_1, \dots , x_n)} = P(x_1) \dots P(x_n)

这是一个 **常量**
，在类别对比中，分母相同，所以可以直接去掉分母，只比较分子，公式\ **简化为**\ 如下分类规则：

.. math:: P(y \mid x_1, \dots, x_n) \propto P(y) \prod_{i=1}^{n} P(x_i \mid y)

.. math:: \Downarrow

.. math:: \hat{y} = \arg\max_y P(y) \prod_{i=1}^{n} P(x_i \mid y),

.. _header-n21:

1.1 高斯朴素贝叶斯
''''''''''''''''''

GuassianNB
实现了运用分类的高斯朴素贝叶斯算法，特征的可能性（即概率）假设为高斯分布：

.. math:: P(x_i \mid y) = \frac{1}{\sqrt{2 \pi \sigma_y^2}} exp(- \frac {(x_i - \mu_y)^2}{2 \sigma_y^2})

-  说明：参数\ :math:`\mu_y`\ 与\ :math:`\sigma_y`
   使用\ **最大似然法估计**

-  代码参考：

   .. code:: python

      from sklearn import datasets
      iris = datasets.load_iris()
      from sklearn.naive_bayes import GaussianNB
      gnb = GaussianNB()
      y_pred = gnb.fit(iris.data, iris.target).predict(iris.data)
      print("Number of mislabeled points out of a total %d points : %d" % (iris.data.shape[0],(iris.target != y_pred).sum()))
      Number of mislabeled points out of a total 150 points : 6

.. _header-n31:

1.2 多项式分布朴素贝叶斯
''''''''''''''''''''''''

MultinomialNB 实现了服从多项分布数据的朴素贝叶斯算法，适用于文本分类，
与TF-IDF 同属于适用文本分类的\ **两大经典算法**

-  说明：文本分类表现良好

-  用途

.. _header-n38:

1.3 伯努利朴素贝叶斯
''''''''''''''''''''

-  说明

-  用途

.. _header-n44:

1.4 基于外存的朴素贝叶斯模型拟合
''''''''''''''''''''''''''''''''

-  说明

-  用途

.. _header-n50:

二、广义线性模型
^^^^^^^^^^^^^^^^

目标值 :math:`y` 是输入变量 :math:`x` 的线性组合。定义向量
:math:` (w_1, \dots , w_p)`\ 代表系数coef\_，\ :math:`w_0`
代表截距intercept\_，数学概念表示为：

.. math:: \hat y (w,x) = w_0 + w_1 x_1 + \dots + w_p x_p

-  

-  用途

.. _header-n58:

三、线性判别分析与二次线性判别分析
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

-  说明

-  用途

.. _header-n64:

四、支持向量机
^^^^^^^^^^^^^^

-  说明

-  用途

.. _header-n70:

五、最近邻KNN
^^^^^^^^^^^^^

-  说明

-  用途

.. _header-n78:

六、决策树
^^^^^^^^^^

-  说明

-  用途

.. _header-n85:

七、集成方法
^^^^^^^^^^^^

-  说明

-  用途

.. _header-n92:

八、TF-IDF（单文本词频-逆文档频率）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

-  说明：TF：Term Frequency - IDF：Inverse Document Frequency
   搜索关键词权重的科学度量、是对搜索关键词的重要性的度量，具备很强的理论依据

-  用途：TF-IDF是信息检索中最重要的发明，在搜索、文献分类和其他领域有广泛的应用

-  度量网页和查询的相关性的原理：

   1. 如果一个查询（贝叶斯算法的用途）包含\ :math:`N`\ 个关键词\ :math:`w_1,w2,\dots,w_n`\ ，他们在一个特定网页中的词频分别是：\ :math:`TF_1,TF_2,\dots,TF_N `\ ，那么这个查询的单文本词频就是每个\ *分词*\ 后的词语的和：\ :math:`TF_1 + TF_2 + \dots + TF_N`\ 。比如某网页上一共1000个词，“贝叶斯”、“算法”、“的”、“用途”分别出现了3次、10次、20次、5次，那么他们的词频分别是：0.003、0.01、0.02、0.005，其和
      :math:`0.003 + 0.01 + 0.02 + 0.005 = 0.038`
      就是“贝叶斯算法的用途”的“单文本词频”

   2. 上面词语中"的"是停用词，权重为零

   3. 假定关键词\ :math:`w`\ 在\ :math:`D_w`\ 个网页中出现过，那么\ :math:`D_w`\ 越大，w的权重就越小，\ :math:`D_w`\ 越小，w权重越大，如上面“的”在每个网页都有，所以权重就最小为0。\ **逆文本频率IDF**\ 公式为\ :math:`log (\frac {D} {D_w})`\ ，其中\ :math:`D`\ 是总网页数目，\ :math:`D_w`\ 是出现关键词的网页数目

   4. 短语相关性的计算公式就由词频简单求和变成了加权求和，公式如下

      .. math:: TF_1 \cdot IDF_1 + TF_2 \cdot IDF_2 + \cdots + TF_N \cdot IDF_N

   1. 共有10亿个网页\ :math:`D=10亿`\ ，如“的”字，在10亿个网页中都出现过，就是\ :math:`D_w=10亿`\ ，所以“的”字权重就是\ :math:`log( \frac {D}{D_w})=log( \frac {D=10亿}{D_w=十亿}) = log(1) = 0`\ ，如“贝叶斯”在100万个网页中出现，“贝叶斯”的权重就是\ :math:`log( \frac {D}{D_w}) = log(\frac {D=10亿}{D_w=100万})=log(1000)=9.966`\ ；如“算法”在250万个网页中出现，“算法”的权重就是\ :math:`log( \frac {D}{D_w}) = log(\frac {D=10亿}{D_w=250万})=log(400)=8.6438`\ ；如“用途”在500万个网页中出现，“用途”的权重就是\ :math:`log( \frac {D}{D_w}) = log(\frac {D=10亿}{D_w=500万})=log(200)=7.6438`\ ；“的”为停用词，权重为0。

      .. code:: python

         import math
         print(math.log(1000,2))
         print(math.log(400,2))
         print(math.log(200,2))

   2. "贝叶斯算法的用途" 短语的TF-IDF详细计算结果如下：

      .. math:: 0.003 \cdot 9.966 + 0.01 \cdot 8.6438 + 0.02 \cdot 0 + 0.005 \cdot 7.6438 = 0.154555

   3. 结合网页排名（PageRank）算法，给定一个查询，搜索有关网页的综合排名大致由\ **相关性和网页排名的乘积**\ 决定。

.. _header-n196:

非监督学习算法
~~~~~~~~~~~~~~

.. _header-n100:

一、高斯混合模型
^^^^^^^^^^^^^^^^

-  说明

-  用途

.. _header-n106:

二、聚类
^^^^^^^^

-  说明

-  用途
