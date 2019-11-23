R数据处理
----------------------



1. 数据处理
~~~~~~~~~~~~~~~~~~~~~~

  数据处理方法，把任务清晰地分成分析和验证两步。

  1.1 数据分析

      用数值摘要表(Summary table) 和基本可视化方法从数据中寻找隐含的模式

  1.2 数据验证

      * 如果认为自己在一个新的数据集上发现了模式，那就用另一批数据来测试这个模式的正规模型 (交叉验证)
      * 利用概率论来测试你在原始数据集中发现的模式是否只是巧合（假设验证）

|


.. Tip::

   简单的方法一度得不到重视，但是要训练数据分析的直觉，最好是用简单的方法去操作数据


2. 数值摘要
~~~~~~~~~~~~~~~~~

  数值摘要就是一些基本的统计项目：均值（mean）和众数（mode）、百分位数（percentile）和中位数（median）、标准差（standard deviation）和方差（variance）
  基本的可视化工具：直方图、核密度估计、散点图 

.. code:: r

  > summary(iris)
  Sepal.Length    Sepal.Width     Petal.Length    Petal.Width          Species  
  Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.100   setosa    :50  
  1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600   1st Qu.:0.300   versicolor:50  
  Median :5.800   Median :3.000   Median :4.350   Median :1.300   virginica :50  
  Mean   :5.843   Mean   :3.057   Mean   :3.758   Mean   :1.199                  
  3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.800                  
  Max.   :7.900   Max.   :4.400   Max.   :6.900   Max.   :2.500 
  
  Min 向量中的最小值
  1st Qu 第一个四分位数（也称为第25个百分位数，即大于25%的数据中最小的那个）
  Median 中位数（称为第50个百分位数）
  Mean 均值
  3rd Qu 第三个四分位数（也成为第75个百分位数）
  Max 最大值

  > quantile(iris$Sepal.Length)
    0%  25%  50%  75% 100% 
   4.3  5.1  5.8  6.4  7.9 
  > 
  quantile统计数据集的0% 25% 50% 75% 100%位置处的数据。如果要得到其他位置的分位数，需要传一个参数probs，如得到每隔10%的位置的分位数：
  > quantile(iris$Sepal.Length, probs = seq(0, 1, by = 0.1))
  0%  10%  20%  30%  40%  50%  60%  70%  80%  90% 100% 
  4.30 4.80 5.00 5.27 5.60 5.80 6.10 6.30 6.52 6.90 7.90
  seq函数在这里用于在 0 ~ 1 之间产生一个步长为0.1的序列
  如在运营部，需要记录用户反馈的响应时间，可能关心前99%顾客的收益远大于只关心中位数个数顾客情况

  # 求标准差与方差
  > sd(iris$Sepal.Length)
  [1] 0.8280661
  > var(iris$Sepal.Length)
  [1] 0.6856935
  > sd(iris$Sepal.Length)
  [1] 0.8280661
  #验证方差为标准差的平方：
  > print(0.8280661^2)
  [1] 0.6856935
  

|


3. 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  * 标量 Atomic Vector
  * 向量 Vector
  * 矩阵 Matrix
  * 数组 Array
  * 因子 factors
  * 列表 List
  * 数据框 Data Frame

|



.. Tip::

   R语言的向量索引下标从1开始，这个不同于Python 列表索引从0开始




