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
 # 显示第一列TOTAL.VALUE的中位数
 > median(housing.df$TOTAL.VALUE)
 [1] 375.9
 ## 每个列的汇总统计信息
 summary(housing.df) 

 # 随机采样观测点
 ## 数据框的行数
 row.names(housing.df)
 ## 对数据框随机采样5个观测点
 s <- sample(row.names(housing.df), 5)
 ## 也可以采用计算的方式采样观测点，隔5条记录取用1条记录，用which方法
 s <- which(1:length(housing.df[,1]) %% 5 == 0) 
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
 # 显示列类型：为因子变量或者整数变量或数字变量
 > class(housing.df$REMODEL)
 [1] "factor"
 > class(housing.df[, 14])
 [1] "factor"
 # 显示因子变量的类别
 > levels(housing.df[,14])
 [1] "None"   "Old"    "Recent"
 > class(housing.df[, 13])
 [1] "integer"
 > class(housing.df[, 1])
 [1] "numeric"

 # 创建二进制虚拟变量dummy variables
 ## 使用model.matrix() 转换分类变量为一系列的虚拟变量
 ## 注意：虚拟变量转完后为矩阵格式，必须转换矩阵格式数据至数据框 

 xtotal <- model.matrix(~ 0 + BEDROOMS + REMODEL, data = housing.df) 
 > names(xtotal)
 NULL  #注意 
 > colnames(xtotal)
 [1] "BEDROOMS"      "REMODELNone"   "REMODELOld"    "REMODELRecent"
 > typeof(xtotal)
 [1] "double"
 > class(xtotal)
 [1] "matrix"
 # 因为xtotal为矩阵，所以无法使用数据框的语法
 > xtotal$BEDROOMS
 #Error in xtotal$BEDROOMS : $ operator is invalid for atomic vectors
 > xtotal.df <- as.data.frame(xtotal)
 > class(xtotal.df)
 [1] "data.frame"
 > names(xtotal.df)
 [1] "BEDROOMS"      "REMODELNone"   "REMODELOld"    "REMODELRecent"
 # 检查虚拟变量的列
 > t(t(names(xtotal.df)))
     [,1]           
 [1,] "BEDROOMS"     
 [2,] "REMODELNone"  
 [3,] "REMODELOld"   
 [4,] "REMODELRecent"
 >  head(xtotal.df)
  BEDROOMS REMODELNone REMODELOld REMODELRecent
  1        3           1          0             0
  2        4           0          0             1
  3        4           1          0             0
  4        5           1          0             0
  5        3           1          0             0
  6        3           0          1             0
  # 删除最后一列REMODELRecent
 > head(xtotal.df[,-4])
  BEDROOMS REMODELNone REMODELOld
  1        3           1          0
  2        4           0          0
  3        4           1          0
  4        5           1          0
  5        3           1          0
  6        3           0          1
 > 
 # 利用中位数处理缺失数据
 # 随机找10条记录，把BEDROOMS改为NA
 > rows.missing10 <- sample(row.names(housing.df), 10)
 > rows.missing10
  [1] "4012" "5071" "2849" "2900" "2378" "4650" "1446" "2159" "3476" "1948"
 > housing.df[rows.missing10,]$BEDROOMS <- NA
 > housing.df[rows.missing10,]$BEDROOMS
 [1] NA NA NA NA NA NA NA NA NA NA
 # 检查BEDROOMS列，已经有10条空记录了，其中中位数为3
 > summary(housing.df$BEDROOMS)
 #  Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
 #  1.00    3.00    3.00    3.23    4.00    9.00      10  
 # 把BEDROOMS为NA的列，替换为中位数3，注意使用na.rm = TRUE 是为了在计算中位数时忽略缺失值
 > median(housing.df$BEDROOMS)
  [1] NA # 计算中位数如果包括了为空的列，则中位数是NA
 > median(housing.df$BEDROOMS, na.rm = TRUE)
  [1] 3
  # 将NA值替换为中位数 
 > housing.df[rows.missing10,]$BEDROOMS <- median(housing.df$BEDROOMS, na.rm = TRUE)
 > housing.df[rows.missing10,]$BEDROOMS
    [1] 3 3 3 3 3 3 3 3 3 3
 > summary(housing.df$BEDROOMS)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  1.000   3.000   3.000   3.229   4.000   9.000 
  # 将数据集分成训练集、验证集、测试集
  ## set.seed()用于设定随机数种子,在重新运行代码或调试程序或者做展示的时候，结果可重复性是很重要的，所以设定此值
  ## set.seed(?)中的数字，代表设置的是第几号种子，不会参与运算，是个标记
 > set.seed(1)
 > dim(housing.df)[1]
 [1] 5802
 > train.rows
   [1] "2580" "1530" "4439" "4136" "4633" "4344" "1222" "2426" "2087" "2483" "2858" "5510" "5400" "1696" "526"  "1069" "5522" "22"   "5491"
  [20] "1128" "983"  "1791" "5612" "3910" "5736" "1639" "4939" "465"  "5092" "5645" "1200" "3863" "1134" "84"   "1895" "3101" "5261" "2300"
  #......
 > train.data <- housing.df[train.rows,]
 > head(train.data)
      TOTAL_VALUE  TAX LOT.SQFT YR.BUILT GROSS.AREA LIVING.AREA FLOORS ROOMS BEDROOMS FULL.BATH HALF.BATH KITCHEN FIREPLACE REMODEL
  2580       469.8 5910     4700     1912       3978        2499      2     8        4         1         1       1         1     Old
  1530       476.0 5988    12180     1965       3591        2167      1     8        3         2         0       1         1  Recent
  4439       416.2 5235     4440     1935       3074        1832      2     8        4         2         0       1         0    None
  4136       413.4 5200     5696     1951       2773        1544      2     7        3         1         1       1         1    None
  4633       445.9 5609     5500     1920       3890        2110      2     9        4         1         2       1         1    None
  4344       355.2 4468     3999     1960       2458        1372      2     6        2         1         1       1         1    None
  # 请观察train.rows 的前六个行数与head(train.data)的前六行对比
  ## 将不在训练集的其他40%记录划给验证集，setdiff(x,y)是求向量x与y中不同的元素（只取x中不同的元素）
 > valid.rows <- setdiff(row.names(housing.df), train.rows)
 > valid.rows
   [1] "2"    "5"    "6"    "8"    "9"    "10"   "11"   "12"   "16"   "20"   "21"   "23"   "24"   "25"   "31"   "33"   "35"   "37"   "42" 
 > setdiff(1:5, 2:6)
 [1] 1
 > valid.data <- housing.df[valid.rows,]
 > head(valid.data)
 ## 确认最终数据集按照60:40比例分割
 > dim(valid.data)
 [1] 2321   14
 > dim(train.data)[1]
 [1] 3481
 > dim(housing.df)[1] * 0.6
 [1] 3481.2
 > dim(housing.df)[1] 
 [1] 5802
 > dim(housing.df)[1] * 0.4
 [1] 2320.8
 # 将数据集按照训练集50%、验证集30%、测试集20%划分
 # 训练集为总数的50%
 > train2.rows <- sample(row.names(housing.df), dim(housing.df)[1] * 0.5)
 > dim(train2.rows)
 NULL
 > summary(train2.rows)
   Length     Class      Mode 
     2901 character character 
 > head(train2.rows)
 [1] "1182" "3409" "5296" "5442" "4956" "2316"
 > train2.data <- housing.df[train2.rows,]
 > dim(train2.data)
 [1] 2901   
 # 利用setdiff获取训练集以外的50%记录，并取其中的30%
 > valid2.rows <- sample(setdiff(row.names(housing.df), train2.rows), dim(housing.df)[1] * 0.3)
 > valid2.data <- housing.df[valid2.rows, ]
 > dim(valid.data)
 [1] 1740   14
 > dim(housing.df)[1] * 0.3
 [1] 1740.6
 > 
 # 利用setdiff获取总数据集 减 训练集+验证集(union(x,y)) 以外的20%
 > test2.rows <- setdiff(row.names(housing.df), union(train2.rows,valid2.rows))
 > test2.data <- housing.df[test2.rows, ]
 > dim(test2.data)
 [1] 1161   14
 > dim(housing.df)[1] * 0.2
 [1] 1160.4
 # 检查创建的这三个数据子集
 > summary(train2.data)
 > summary(valid2.data)
 > summary(test2.data)


 # 利用数据集2进行Complete（Exact）精确贝叶斯计算

 ## 
 df <- read.csv(("D:\\Books\\dataset\\company10.csv"))
 > df
    company plt CompanySize     Status
 1        1 Yes       Small   Truthful
 2        2  No       Small   Truthful
 3        3  No       Large   Truthful
 4        4  No       Large   Truthful
 5        5  No       Small   Truthful
 6        6  No       Small   Truthful
 7        7 Yes       Small Fraudulent
 8        8 Yes       Large Fraudulent
 9        9  No       Large Fraudulent
 10      10 Yes       Large Fraudulent


 # 计算有法律麻烦的小公司，欺诈的概率 P(fraudulent|PriorLegal = y,Size = small) = 1/2=0.5
 dfsmall <- subset(df, df$plt == 'Yes' & df$CompanySize == 'Small')
 dfsmall
   company plt CompanySize     Status
 1       1 Yes       Small   Truthful
 7       7 Yes       Small Fraudulent
 
 # 计算有法律麻烦的大公司，欺诈的概率P(fraudulent|PriorLegal = y,Size = large) = 2/2 = 1 
 dflarge <- subset(df, df$plt == 'Yes' & df$CompanySize == 'Large')
 > dflarge
   company plt CompanySize     Status
 8        8 Yes       Large Fraudulent
 10      10 Yes       Large Fraudulent

 # 计算没有法律麻烦的小公司，欺诈的概率P(fraudulent|PriorLegal = n,Size = small) = 0/3 = 0
 > dfNsmall <- subset(df, df$plt == 'No' & df$CompanySize == 'Small')
 > dfNsmall
   company plt CompanySize   Status
 2       2  No       Small Truthful
 5       5  No       Small Truthful
 6       6  No       Small Truthful

 # 计算没有法律麻烦的大公司，欺诈的概率 P(fraudulent|PriorLegal = n,Size = large) = 1/3 = 0.33
 > dfNlarge <- subset(df, df$plt == 'No' & df$CompanySize == 'Large')
 > dfNlarge
   company plt CompanySize     Status
 3       3  No       Large   Truthful
 4       4  No       Large   Truthful
 9       9  No       Large Fraudulent

 # 利用数据集进行 朴素贝叶斯 计算

 # 引用e1071 朴素贝叶斯程序包
 library(e1071)
 df.nb2 <- naiveBayes(Status ~ ., data = df[, c(2,3,4)])
 df.nb2
 Naive Bayes Classifier for Discrete Predictors
 
 Call:
 naiveBayes.default(x = X, y = Y, laplace = laplace)
 
 A-priori probabilities:
 Y
 Fraudulent   Truthful 
        0.4        0.6 
 
 Conditional probabilities:
             plt
 Y                   No       Yes
   Fraudulent 0.2500000 0.7500000
   Truthful   0.8333333 0.1666667
 
             CompanySize
 Y                Large     Small
   Fraudulent 0.7500000 0.2500000
   Truthful   0.3333333 0.6666667
 > prop.table(table(df$Status, df$CompanySize, df$plt), margin=1)
 , ,  = No
 
             
                  Large     Small
   Fraudulent 0.2500000 0.0000000
   Truthful   0.3333333 0.5000000
 
 , ,  = Yes
 
             
                  Large     Small
   Fraudulent 0.5000000 0.2500000
   Truthful   0.0000000 0.1666667
 
 # P(fraudulent|PriorLegal = y, Size = small) = 
 # P(PriorLegal = yes, Size = small|fraudulent) * P(fraudulent) / 
 # P(PriorLegal = yes, Size = small|fraudulent) * P(fraudulent) + P(PriorLegal = yes, Size = small|Truthful) * P(Truthful)
 # 把所有欺诈的记录找出来，求取在欺诈记录中，有法律麻烦plt=yes的概率：3/4 小公司的概率：1/4
 > subset(df_nb, df_nb$Status == 'Fraudulent')
   plt CompanySize     Status
 7  Yes       Small Fraudulent
 8  Yes       Large Fraudulent
 9   No       Large Fraudulent
 10 Yes       Large Fraudulent
 # P(PriorLegal = yes|fraudulent) = 3/4 
 #
 # P(Size = small|fraudulent) = 1/4

 # P(fraudulent)
 > dim(subset(df_nb, df_nb$Status == 'Fraudulent'))[1]/dim(df_nb)[1]
 [1] 0.4
 # 分子为：P(PriorLegal = yes, Size = small|fraudulent) * P(fraudulent) =
 # P(PriorLegal = yes|fraudulent) * P(Size = small|fraudulent) * P(fraudulent)
 > 3/4 * 1/4 * 0.4
 [1]  0.075
 # 把所有诚实的记录找出来，求取在诚实的记录中，有法律麻烦plt=yes的概率，小公司的概率：
 > subset(df_nb, df_nb$Status == 'Truthful')
  plt CompanySize   Status
 1 Yes       Small Truthful
 2  No       Small Truthful
 3  No       Large Truthful
 4  No       Large Truthful
 5  No       Small Truthful
 6  No       Small Truthful
 
 # 求取分母另一项：P(PriorLegal = yes|Truthful) * P(Size = small|Truthful) * P(Truthful)
 # P(PriorLegal = yes|Truthful) 为 1/6
 # P(Size = small|Truthful) 为 4/6
 # P(Truthful) = 6/10
 # P(PriorLegal = yes|Truthful) * P(Size = small|Truthful) * P(Truthful) = 
 > 1/6 * 4/6 * 6/10 
 [1] 0.067
 # 有法律麻烦的小企业，欺诈的概率 = 0.053 
 # P(fraudulent|PriorLegal = y, Size = small) = 0.075 / ( 0.075 + 0.067) 约为0.53
 [1] 0.0528
 # 同理，可以用同样的方法计算有法律麻烦的大企业，欺诈的概率
 # P(fraudulent|PriorLegal = y, Size = large) = 0.87
 # P(PriorLegal = y|fraudulent) * P( Size = large|fraudulent) * P(fraudulent)/
 # [P(PriorLegal = y|fraudulent) * P( Size = large|fraudulent) * P(fraudulent) + P(PriorLegal = y|Truthful) * P( Size = large|Truthful) * P(Truthful)]
 > 3/4 * 3/4 * 4/10 / (3/4 * 3/4 * 4/10 + 1/6 * 2/4 * 6/10) 
 [1] 0.8181
 # P(fraudulent|PriorLegal = n, Size = small) = 0.07
 # P(fraudulent|PriorLegal = n, Size = large) = 0.31
 # 如果从概率排序看，有法律麻烦的大企业的欺诈的概率高于有法律麻烦的小企业
 # 注意这些朴素贝叶斯概率和精确贝叶斯概率有多接近
 # 尽管他们不相等，但是他们会导致 相同的 分类，概率的排序甚至比概率本身更接近于准确的贝叶斯方法


-----------------------------------------


2. 数据集说明
~~~~~~~~~~~~~~~~~~~~


- 数据集1

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

- 数据集2 （用于完全（精确）贝叶斯计算/朴素贝叶斯计算）

 ============= ============================================
  company       公司序号
  PLT           Prior Legal Trouble 法律上的麻烦
  Companysize   公司大小（large 或small）
  Status        真实的Truthful，欺骗的Fraudulent
 ============= ============================================


参考：
   1. R 数据处理，`R_data_mining`_

.. _R_data_mining: Data Mining for Business Analytics Concepts Techniques and Applications in R 
