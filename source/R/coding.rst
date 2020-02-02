R 代码样例
--------------

1. 数据处理
~~~~~~~~~~~~~~~~~~~~~~~~


.. tip::
   以波士顿WEST ROXBURY (BOSTON) 房产价值数据集为例！

.. code:: r

 # 读取WEST ROXBURY西罗克斯伯里数据集
 housing.df <- read.csv("D:\\dataset\\WestRoxbury.csv", header=TRUE)
 # 读取数据集的行、列数
 dim(housing.df)
 # 显示数据框的列名
 colnames(housing.df)
 [1] "TOTAL.VALUE" "TAX"         "LOT.SQFT"    "YR.BUILT"    "GROSS.AREA"  "LIVING.AREA" "FLOORS"      "ROOMS"       "BEDROOMS"    "FULL.BATH"  
 [11] "HALF.BATH"   "KITCHEN"     "FIREPLACE"   "REMODEL"    
 # 打印列表变量，与colnames同 
 names(housing.df)
  [1] "TOTAL.VALUE" "TAX"         "LOT.SQFT"    "YR.BUILT"    "GROSS.AREA"  "LIVING.AREA" "FLOORS"      "ROOMS"       "BEDROOMS"    "FULL.BATH"  
 [11] "HALF.BATH"   "KITCHEN"     "FIREPLACE"   "REMODEL" 
 # 显示行名
 rownames(housing.df)
 
 # 查看前6行数据
 head(housing.df)
 # 查看前10条记录
 head(housing.df, 10)
 TOTAL.VALUE  TAX LOT.SQFT YR.BUILT GROSS.AREA LIVING.AREA FLOORS ROOMS BEDROOMS FULL.BATH HALF.BATH KITCHEN FIREPLACE REMODEL
       344.2 4330     9965     1880       2436        1352      2     6        3         1         1       1         0    None
       412.6 5190     6590     1945       3108        1976      2    10        4         2         1       1         0  Recent
       330.1 4152     7500     1890       2294        1371      2     8        4         1         1       1         0    None
 # 查看后10条记录
 tail(housing.df, 10)
 # 在新的窗格窗口查看数据
 View(housing.df)

 # 练习显示不同数据子集

 ## 显示第一列的前10行 
 housing.df[1:10, 1]
 ## 显示所有列的前6行
 housing.df[1:6,]
 ## 显示前六列的第五条记录
 housing.df[5, 1:6]
   TOTAL.VALUE  TAX LOT.SQFT YR.BUILT GROSS.AREA LIVING.AREA
 5       331.5 4170     5000     1910       2370        1438
 ## 显示前10列的第5条记录
 housing.df[5, 1:10]
  TOTAL.VALUE  TAX LOT.SQFT YR.BUILT GROSS.AREA LIVING.AREA FLOORS ROOMS BEDROOMS FULL.BATH
 5       331.5 4170     5000     1910       2370        1438      2     7        3         2
 
 ## 显示某些列的第五条记录
  housing.df[5, c(1:2,8,11:14)]
  TOTAL.VALUE  TAX ROOMS HALF.BATH KITCHEN FIREPLACE REMODEL
  5       331.5 4170     7         0       1         0    None
  housing.df[5, c(1,5,12)]
  TOTAL.VALUE GROSS.AREA KITCHEN
  5       331.5       2370       1
 ## 显示第一列的所有行
 housing.df[, 1]
 ## 显示第一~第二列的所有行
 housing.df[, 1:2] 
 ## 显示第一列所有行的另一种方法
 housing.df$TOTAL_VALUE 
 ## 显示第一列的前六行
 housing.df$TOTAL.VALUE[1:6]
 [1] 344.2 412.6 330.1 498.6 331.5 337.4
 ## 显示第一列的长度
 > length(housing.df$TOTAL.VALUE)
 [1] 5802
 > length(housing.df$TAX)
 [1] 5802
 ## 显示第一列TOTAL.VALUE的均值 
 > mean(housing.df$TOTAL.VALUE)
 [1] 392.6857
 ## 显示第一列TOTAL.VALUE的中位数
 > median(housing.df$TOTAL.VALUE)
 [1] 375.9
 ## 每个列的汇总统计信息
 summary(housing.df) 

 # 随机采样观测点
 ## 数据框的行数
 row.names(housing.df)
 ## 对数据框随机采样5个观测点
 s <- sample(row.names(housing.df), 5)
 s
 [1] "330"  "1799" "3913" "5711" "1749"
 > housing.df[s,]
      TOTAL.VALUE  TAX LOT.SQFT YR.BUILT GROSS.AREA LIVING.AREA FLOORS ROOMS BEDROOMS FULL.BATH HALF.BATH KITCHEN FIREPLACE REMODEL
 330        310.7 3908     6465     1960       1948        1404    1.0     5        3         1         1       1         1    None
 1799       323.2 4065     4794     1900       2443        1171    1.5     6        3         1         1       1         0  Recent
 3913       360.0 4528     8005     1950       2891        1042    1.0     6        2         1         0       1         1    None
 5711       368.9 4640     6416     1950       2995        1321    1.0     5        2         1         0       1         1  Recent
 1749       361.5 4547     8162     1954       2660        1457    1.5     6        3         2         0       1         1     Old
 > 
 # oversample houses with over 10 rooms 
 ## ifelse(housing.df$ROOMS>10,0.9,0.1) 如果房间数大于10，则值为0.9，否则为0.01
 ## prob 是被用来抽样的向量元素的一个概率权重的向量
 s <- sample(row.names(housing.df), 5, prob = ifelse(housing.df$ROOMS>10, 0.9, 0.01)) 
 housing.df[s,]

 # 打印可读格式的列名

 > t(names(housing.df))
      [,1]          [,2]  [,3]       [,4]       [,5]         [,6]          [,7]     [,8]    [,9]       [,10]       [,11]       [,12]    
 [1,] "TOTAL.VALUE" "TAX" "LOT.SQFT" "YR.BUILT" "GROSS.AREA" "LIVING.AREA" "FLOORS" "ROOMS" "BEDROOMS" "FULL.BATH" "HALF.BATH" "KITCHEN"
      [,13]       [,14]    
 [1,] "FIREPLACE" "REMODEL"
 > y <- t(names(housing.df))
 > typeof(y)
 [1] "character"
 > dim(y)
 [1]  1 14
 > t(y)
      [,1]         
  [1,] "TOTAL.VALUE"
  [2,] "TAX"        
  [3,] "LOT.SQFT"   
  [4,] "YR.BUILT"   
  [5,] "GROSS.AREA" 
  [6,] "LIVING.AREA"
  [7,] "FLOORS"     
  [8,] "ROOMS"      
  [9,] "BEDROOMS"   
  [10,] "FULL.BATH"  
  [11,] "HALF.BATH"  
  [12,] "KITCHEN"    
  [13,] "FIREPLACE"  
  [14,] "REMODEL"    
 # 修改第一列的名字"TOTAL.VALUE" 为 TOTAL_VALUE
 > colnames(housing.df)[1] <- c("TOTAL_VALUE")
 > colnames(housing.df)
  [1] "TOTAL_VALUE" "TAX"         "LOT.SQFT"    "YR.BUILT"    "GROSS.AREA"  "LIVING.AREA" "FLOORS"      "ROOMS"       "BEDROOMS"    "FULL.BATH"  
  [11] "HALF.BATH"   "KITCHEN"     "FIREPLACE"   "REMODEL"    
  > 
 # 显示列为因子变量或者整数变量
 > class(housing.df$REMODEL)
 [1] "factor"
 > class(housing.df[, 14])
 [1] "factor"
 > class(housing.df[, 13])
 [1] "integer"


-----------------------------------------


数据集说明
~~~~~~~~~~~~~~~~~~~~


===================== ===================================================================
TOTAL VALUE             房产估价总值，单位：千美元
TAX                     税单金额根据总评估价值乘以税率，单位：美元
LOT SQ FT               总平方英尺数
YR BUILT                房产建成年份
GROSS AREA              总建筑面积
LIVING AREA             房产的总居住面积
FLOORS                  楼层数
ROOMS                   房间数
BEDROOMS                卧室数
FULL BATH               全浴总数
HALF BATH               半浴总数
KITCHEN                 厨房总数
FIREPLACE               壁炉总数
REMODEL                 何时重装修的？(Recent/Old/None)
===================== ===================================================================



参考：
   1. R 数据处理，`R_data_mining`_

.. _R_data_mining: Data Mining for Business Analytics Concepts Techniques and Applications in R 

