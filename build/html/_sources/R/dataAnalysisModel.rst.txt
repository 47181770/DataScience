数据分析模型
--------------

1. 独立性检验
~~~~~~~~~~~~~~~~~~~~~~~~


- 三种基本检验方法

    * 卡方独立性Pearson's Chi-squared test检验：chisq.test()
    * Fisher精确检验：fisher.test()
    * Cochran-Mantel-Haenszel卡方检验：mantelhaen.test()

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
    计算好相关系数以后，如何对他们进行统计显著性检验呢？常用的原假设为变量间不相关（即总体的相关系数为0）：
    cor.test(x, y, alternative = , method = )
    x和y为要检验相关性的变量
    alternative 用来指定进行双侧检验或单侧检验（取值为：two.side、less或greater），当研究的假设为总体的相关系数小于0时，请使用alternative = "less"，研究的假设为总体的相关系数大于0时，使用alternative = "greater"，默认为two.side总体相关系数不等于0.
    method可选类型：pearson、spearman或kendall
    遗憾的是cor.test每次只能检验一种相关关系。psych包中提供的corr.test()函数可以计算相关矩阵并进行显著性检验

.. code:: r

  # 使用state.x77美国数据集
  > sfstates <- state.x77[,1:6]
  > library(psych)
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
 
- t检验



-----------------------------------------


2. 类与面向对象编程
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


- **类**
    1) 类是对象的蓝图和模板，而对象是类的实例，类是创建对象的机制
    2) 类通常是由函数（称为方法, method)、变量和计算出的属性（称为特性）组成的集合
    3) 类是抽象的概念，对象是具体的东西
    4) 类的实例是以函数形式调用类对象来创建的
    5) 原始类称为基类或超类


- **对象**

.. tip::
  在R中一切皆为对象，对象都有属性和行为，每个对象都是独一无二的，而且对象一定属于某个类

- 封装
    1) 把程序接口从具体的实现细节中分离开来的过程称为封装

- 继承inheritance
    1) 继承是一种创建新类的机制，目的是使用或修改现有类的行为，原始类称为基类或超类，新类称谓派生类或子类
    2) 可以使得某个子类（subclass）基于另一个类(superclass)


- 多态
    1) 允许同一个方法名操纵不同的对象并得到不同的结果，称为多态(Polymorphism)


-----------------------------------------


3. 包
~~~~~~~~~~~~~~~~~~~~

- 命令行安装R包

    1) $R CMD INSTALL aplpack_1.2.1.tgz

- R控制台安装/删除R包
    1）> install.packages(c("tree","map"))
    2）> remove.packages(c("tree","map"))

- 引入R包
    1）> library(devtools)
