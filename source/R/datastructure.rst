R数据类型与数据结构
----------------------

- 数据类型
 
  经典的数据类型是由一系列数据组成的变量，按照通常数据处理的分类，有**数值变量（或定量变量）**，分为连续性（定比）与离散型（定距），还有非数值变量（定性变量），分为类别型（定类）与有序性（定序）

  * 字符型 characters
  * 数值型 numerics 
  * 逻辑判断 Logical (TRUE or FALSE)
  * 因子factors

- 数据结构

  * 标量 Atomic Vector
  * 向量 Vector
  * 矩阵 Matrix
  * 数组 Array
  * 列表 List
  * 数据框 Data Frame

- 数据结构常用方法

  * as 转换数据类型
  * class() 是什么类型的对象
  * typeof() 对象的数据类型
  * length() 对象多长，两维对象呢？
  * attributes() 有元数据？是什么
  * head() 显示前6行数据
  * tail() 显示后6行数据
  * dim()  数据框维度，行列数
  * nrow() 行数
  * ncol() 列数
  * str() 数据框的结构， name, type and preview of data in each column
  * names() or colnames() 显示一个数据框的属性
  * sapply(dataframe, class) - shows the class of each column in the data frame
 

- 使用样例如下：

.. code:: r

 2 * 3
 # 返回 [1] 6

 myvector <- c(1, 3 ,8, 6, 9, 10, 5)

 myvector[0]
 # 返回  numeric(0)
 myvector[1]
 # 返回 [1] 1
 typeof(myvector)
 # 返回 [1] "double"
 class(myvector)
 # 返回 [1] "numeric"
 length(myvector)
 # [1] 7
 attributes(myvector)
 # NULL
 head(myvector)
 #[1]  1  3  8  6  9 10
 tail(myvector)
 #[1]  3  8  6  9 10  5
 str(myvector)
 # num [1:7] 1 3 8 6 9 10 5



.. Tip::

   R语言的向量索引下标从1开始，这个不同于Python 列表索引从0开始
