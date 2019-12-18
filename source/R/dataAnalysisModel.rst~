数据分析
--------------


1. 独立性检验
~~~~~~~~~~~~~~~~~~~~~~~~

  许多的数据分析问题都可以归结为统计检验问题，如新的投资策略能否跑赢指数基金的收益？
  要解答如上问题，需要提出一个原假设，设计一个实验，搜集所需数据并进行分析。

- 三种基本显著性检验方法

    显著性检验评估了是否存在充分的证据以**拒绝变量间相互独立的原假设**，如果可以拒绝原假设，那么就转向衡量相关性强弱的相关性度量。

    * 卡方独立性Pearson's Chi-squared test检验：chisq.test()
      对二维表的行变量和列变量进行卡方独立性检验，计算的p值表示从总体中抽取的样本行变量与列变量是相互独立的概率。如果p<0.05说明看上去存在着某种关系，也就是说两个变量之间独立的概率低；如果p>0.05则说明两个变量之间独立的概率高，也就是不存在关系，相互独立。
    * Fisher精确检验：fisher.test()
      原假设：边界固定的列联表中行和列是相互独立的
    * Cochran-Mantel-Haenszel卡方检验：mantelhaen.test()
      原假设：两个名义变量在第三个变量的每一层中都是条件独立的，用于3变量之间的检验



.. code:: r

  # 两变量之间的独立性检验
  library("vcd")
  mytable4 <- xtabs(~Treatment + Improved, data=Arthritis)
  mantelhaen.test(mytable4)
  #Error in mantelhaen.test(mytable4) : 'x'必需是三维陣列
  chisq.test(mytable4)
  #        Pearson's Chi-squared test

  #data:  mytable4
  #X-squared = 13.055, df = 2, p-value = 0.001463  # 从p值看相互独立的概率低，具有一定相关性

  > fisher.test(mytable4)

          Fisher's Exact Test for Count Data
  
  data:  mytable4
  p-value = 0.001393  # 从p值看相互独立的概率低，具有一定相关性
  alternative hypothesis: two.sided

  > mytable4
           Improved
  Treatment None Some Marked
    Placebo   29    7      7
    Treated   13    7     21
  > 
  # 检验治疗情况和改善效果在性别的每个水平下是否独立
  > mytable5 <- xtabs(~Treatment + Improved + Sex, data=Arthritis)
  > mantelhaen.test(mytable5)

        Cochran-Mantel-Haenszel test

  data:  mytable3
  Cochran-Mantel-Haenszel M^2 = 14.632, df = 2, p-value = 0.0006647 # 从p值看相互独立的概率低，具有一定相关性

  > mytable6 <- xtabs(~Treatment + Improved, data=Arthritis)
  > mytable6
           Improved
  Treatment None Some Marked
    Placebo   29    7      7
    Treated   13    7     21
  > assocstats(mytable6)
                      X^2 df  P(> X^2)
  Likelihood Ratio 13.530  2 0.0011536
  Pearson          13.055  2 0.0014626

  Phi-Coefficient   : NA 
  Contingency Coeff.: 0.367 
  Cramer's V        : 0.394 
  > kappa(mytable6)
  [1] 2.760342
   



- 相关性度量

    * 计算二维列连表的phi系数、列连系数和Cramer's V系数，vcd包中的函数用法：assocstats(mytable)
      较大的值意味着较强的相关性。vcd包中也提供了一个kappa()函数，计算混淆矩阵的Cohen's kappa值以及加权的kappa值

- 相关系数

    相关系数可以用来描述定量变量之间的关系。相关系数的符号（正负号）表明关系的方向（正相关或负相关），值大小表示关系强弱的程度，完全相关时为1，完全不相关时为0

    * Pearson相关系数
      Pearson积差相关系数衡量了两个定量变量之间的线性相关程度
    * Spearman相关系数
      Spearman等级相关系数则衡量分级定序变量之间的相关程度
    * Kendall相关系数
      Kendall's Tau相关系数是一种非参数的等级相关度量

    如上三种相关系数可以用cor()函数计算，而cov()函数可以用来计算协方差：

    cor(x, use= , method=) ; cov(x, use= , method=)

    x为矩阵或数据框；

    use指定缺失数据的处理方式（默认为everything，可选为all.obs不存在缺失数据-遇缺失数据则报错、complete.obs行删除、pairwise.complete.obs成对删除）

    method指定相关系数类型默认为pearson。可选类型：pearson、spearman或kendall

    计算好相关系数以后，如何对他们进行统计显著性检验呢？常用的原假设为变量间不相关（即总体的相关系数为0）.

    如果对某一组变量与另一组变量之间的关系感兴趣时，cor仍然非常有用




.. code:: r

  > library(vcd)
  > x <- sfstats[,c("Population", "Income", "Illiteracy", "HS Grad")]
  > y <- sfstats[,c("Life Exp", "Murder")]
  > cor(x,y)
                  Life Exp     Murder
  Population -0.06805195  0.3436428
  Income      0.34025534 -0.2300776
  Illiteracy -0.58847793  0.7029752
  HS Grad     0.58221620 -0.4879710
  > 
  
  # 可以判断一组变量与另一组变量之间的关系 
  # cor.test 检验某种相关系数的显著性，如检查文盲率Illiteracy与谋杀Murder之间的相关性
  > cor.test(sfstats[,3], sfstats[,5])

  #        Pearson's product-moment correlation

  #data:  sfstats[, 3] and sfstats[, 5]
  #t = 6.8479, df = 48, p-value = 1.258e-08
  #alternative hypothesis: true correlation is not equal to 0
  #95 percent confidence interval:
  # 0.5279280 0.8207295
  #sample estimates:
  #      cor 
  #0.7029752 
  #> 
  > head(sfstats)
             Population Income Illiteracy Life Exp Murder HS Grad
  Alabama          3615   3624        2.1    69.05   15.1    41.3
  Alaska            365   6315        1.5    69.31   11.3    66.7
  Arizona          2212   4530        1.8    70.55    7.8    58.1
  Arkansas         2110   3378        1.9    70.66   10.1    39.9
  California      21198   5114        1.1    71.71   10.3    62.6
  Colorado         2541   4884        0.7    72.06    6.8    63.9
  > colnames(sfstats)
  [1] "Population" "Income"     "Illiteracy" "Life Exp"   "Murder"     "HS Grad"   

 

  cor.test(x, y, alternative = , method = )

    x和y为要检验相关性的变量

    alternative 用来指定进行双侧检验或单侧检验（取值为：two.side、less或greater），当研究的假设为总体的相关系数小于0时，请使用alternative = "less"，研究的假设为总体的相关系数大于0时，使用alternative = "greater"，默认为two.side总体相关系数不等于0.

    method可选类型：pearson、spearman或kendall
    遗憾的是cor.test每次只能检验一种相关关系。psych包中提供的corr.test()函数可以计算相关矩阵并进行显著性检验

    * 偏相关系数
      偏相关是指在控制一个或多个定量变量时，另外两个定量变量之间的相互关系，常用于社会科学的研究中。使用ggm的pcor()函数计算偏相关系数，在多元正态性的假设下，ggm中的pcor.test()函数可以用来检验在控制一个或多个额外变量时两个变量之间的条件独立性。
      pcor(u, S)
      u是一个数值向量
      S为变量的协方差阵
      
      pcor.test(r, q, n)
      r 是由pcor计算得到的偏相关系数
      q 为要控制的变量数（以数值表示位置）
      n 为样本大小
      psych包中的r.test()提供了多种实用性的显著性检验方法，参阅help(r.test)：
      1. 某种相关系数的显著性
      2. 两个独立相关系数的差异是否显著
      3. 两个基于一个共享变量得到的非独立相关系数的差异是否显著
      4. 两个基于完全不同的变量得到的非独立相关系数的差异是否显著

    * 多分格（polychoric）相关系数
    * 多系列（polyserial）相关系数
    

.. code:: r

  # 使用state.x77美国50个州的人口、收入、文盲率、预期寿命、谋杀率、高中毕业率数据集
  > sfstates <- state.x77[,1:6]
  > library(psych)
  > cor(sfstats)
              Population     Income Illiteracy    Life Exp     Murder     HS Grad
  Population  1.00000000  0.2082276  0.1076224 -0.06805195  0.3436428 -0.09848975
  Income      0.20822756  1.0000000 -0.4370752  0.34025534 -0.2300776  0.61993232
  Illiteracy  0.10762237 -0.4370752  1.0000000 -0.58847793  0.7029752 -0.65718861
  Life Exp   -0.06805195  0.3402553 -0.5884779  1.00000000 -0.7808458  0.58221620
  Murder      0.34364275 -0.2300776  0.7029752 -0.78084575  1.0000000 -0.48797102
  HS Grad    -0.09848975  0.6199323 -0.6571886  0.58221620 -0.4879710  1.00000000
  
  > corr.test(sfstates, use = "complete", method = "pearson")
  Call:corr.test(x = sfstates, use = "complete", method = "pearson")
  Correlation matrix 
             Population Income Illiteracy Life Exp Murder HS Grad
  Population       1.00   0.21       0.11    -0.07   0.34   -0.10
  Income           0.21   1.00      -0.44     0.34  -0.23    0.62
  Illiteracy       0.11  -0.44       1.00    -0.59   0.70   -0.66
  Life Exp        -0.07   0.34      -0.59     1.00  -0.78    0.58
  Murder           0.34  -0.23       0.70    -0.78   1.00   -0.49
  HS Grad         -0.10   0.62      -0.66     0.58  -0.49    1.00
  Sample Size 
  [1] 50
  Probability values (Entries above the diagonal are adjusted for multiple tests.) 
             Population Income Illiteracy Life Exp Murder HS Grad
  Population       0.00   0.59       1.00      1.0   0.10       1
  Income           0.15   0.00       0.01      0.1   0.54       0
  Illiteracy       0.46   0.00       0.00      0.0   0.00       0
  Life Exp         0.64   0.02       0.00      0.0   0.00       0
  Murder           0.01   0.11       0.00      0.0   0.00       0
  HS Grad          0.50   0.00       0.00      0.0   0.00       0
  收入与高中毕业有一定相关性
 

 
- t检验
  T检验，又称student t检验，主要用于样本含量较小（例如n<30），总体标准差未知的正态分布。用T分布理论来推断差异发生的概率，从而判定两个平均数的差异是否显著
  想知道更多组件差异的细节信息，使用pairwise.t.test（x, g, p.adjust.method = p.adjust.methods, pool.sd = !paried, ...）函数。x是一个数值型向量，g指定一个用于分组的因子变量

- 正态性检验
  Shapiro-Wilk检验：shapiro.test(x)

- 分布的对称性检验
  Kolmogorov-Smirnov检验来查看一个向量是否来自对称的概率分布（不限于正态分布）

    ks.test(x, y, ...,
            alternative = c("two-side", "less", "greater"),
            exact = NULL)
  
  x为待检验数据
  y为指定对称分布的类型，可以是数值型向量、指定概率分布函数的字符串或是一个分布函数 

- 二项式检验
  如抛硬币，只有正反两面两种

- 列联表检验，Fisher确切概率检验：fisher.test()

- 列联表非参数检验，Friedman禾失和检验是一个非参数版本的双边ANOVA方差检验，用friedman.test来执行检验

-----------------------------------------


2. 数据分析常用方法
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. tip::

  讲解如何对数值型结果变量和一系列数值型和（或）类别型预测变量之间的关系建模

- **回归**
  回归模型的假设条件很苛刻：结果或响应变量要是数值型，并且必须是来自正态分布的随机抽样。（很多情况数据并不满足正态分布），回归有许多特殊的变种，对于回归模型的拟合，R提供了200+个函数，参考`r_regression`_


  * 回归模型：数据满足正态分布
    回归分析是统计学的核心，是一个广义的概念，通常用于基于一个或多个预测变量（自变量或解释变量）来预测响应变量（也称因变量、结果变量）。回归分析可用来挑选与响应变量相关的解释变量，可以描述两者的关系，也可以生成一个方程等式来预测一个响应变量的值。

  * 数据来源于未知或混合分布，分析方法参考重抽样与自助法

- **方差分析**


- **功效分析**

- **重抽样与自助法**


- **广义线性模型**

- **主成分与因子分析**



-----------------------------------------


3. 统计术语解释
~~~~~~~~~~~~~~~~~~~~

- 显著性检验
  Significance test是实现对总体（随机变量）的参数或总体分布形式做出一个假设，然后利用样本信息来判断这个假设是否合理，即判断总体的真是情况与原假设是否有显著性差异。是针对我们对总体所作的假设做检验，原理就是“小概率事件实际不可能性原理”来接受或否定假设
  步骤如下：
  1. 提出虚无假设和备择假设，同时与备择假设相应，指出所作检验为双尾检验还是左单尾或右单尾检验
  2. 构造检验统计量，收集样本数据，计算检验统计量的样本观察值
  3. 根据所提出的显著水平，确定临界值和拒绝域
  4. 计算检验统计量的值
  5. 做出检验决策
  
  常用检验如下：

  * t检验：适用于计量资料、正态分布、方差具有齐性的两组间小样本比较
  * t'检验：与t检验大致同，但t'检验用于两组间方差不齐时
  * U检验：应用条件与t检验基本一致，当大样本时用U检验
  * 方差分析
    用于正态分布、方差齐性的多组间计量比较。常见的有单因素分组的多样本均数比较及双因素分组的多样本均数比较，方差分析首先是比较各组间总的差异，如总差异有显著性，再进行组间两两比较，组件比较用q检验或LST检验等，如p值小于0.05，则表明两总体的方差的差异是统计显著的。
  * X2检验
    用于两个或多个百分比（率）的比较，如：四格表资料、配对资料、多余2行*2列资料及组内分组X2检验
  * 零反应检验
    属于直接概率及算法，X2检验的一种特殊形式
  * 符号检验、禾失和检验和Ridit检验
    属于非参数统计方法，可用于各种非正态分布的资料、未知分布资料及半定量资料的分析，凡是正态分布或可通过数据转换成正态分布者尽量不要用这些方法
  * Hotelling检验
    用于计量资料、正态分布、两组间多项指标的综合差异显著性检验


- 建模

- 预测



.. _r_regression: https://cran.r-project.org/doc/contrib/Ricci-refcard-regression.pdf

