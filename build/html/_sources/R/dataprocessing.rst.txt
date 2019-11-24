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


3. 基本数据管理
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  * 缺失值
  * 日期值
  * 类型转换
  * 数据排序
  * 数据集合并
  * 数据集取子集
  * SQL语句操作数据框

- 创建新变量，变量重命名
  样例如下，建议采用transform()函数新增数据框的列

.. code:: r

  > myd <- data.frame(x1 = c(2,2,6,3), x2 = c(3,4,5,6))
  > myd
    x1 x2
  1  2  3
  2  2  4
  3  6  5
  4  3  6
  > myd$sumx <- myd$x1 + myd$x2
  > myd
   x1 x2 sumx
  1  2  3    5
  2  2  4    6
  3  6  5   11
  4  3  6    9
  > my <- transform(myd, meanx = (x1 + x2)/2)
  > my
    x1 x2 sumx meanx
  1  2  3    5   2.5
  2  2  4    6   3.0
  3  6  5   11   5.5
  4  3  6    9   4.5
  > 
  transform()简化了按需创建新变量并将其保存到数据框的过程
  
  # 变量重命名
  > library(reshape)
  > my
    x1 x2 sumx meanx
  1  2  3    5   2.5
  2  2  4    6   3.0
  3  6  5   11   5.5
  4  3  6    9   4.5
  > myv2 <- rename(my, c(sumx = "Summary", meanx = "Mean"))
  > myv2
    x1 x2 Summary Mean
  1  2  3       5  2.5
  2  2  4       6  3.0
  3  6  5      11  5.5
  4  3  6       9  4.5
  > 
  > names(myv2)
  [1] "x1"      "x2"      "Summary" "Mean"  
  使用reshape包中的rename函数，快速修改变量名，使用格式为：
  rename(dataframe, c(oldname1 = "newname1", oldname2="newname2", ...))

- 缺失值
  R中，缺失值以符号NA（Not Available，不可用）表示。不可能出现的值（如÷0的结果），通过符号NaN（Not a Number非数值）表示。
  R中字符型和数值型数据使用的缺失值符号相同
  识别包含缺失值的函数：is.na()
  注意：缺失值不可比较，不能用 myvar == NA 来获取TRUE比较

.. code:: r

  y <- c(2,4,6,NA)
  > y
  [1]  2  4  6 NA
  > is.na(y)
  [1] FALSE FALSE FALSE  TRUE
  > 
  # is.na 识别出了缺失值  
  > y
  [1]  2  4  6 NA
  # 请注意观察，NA + 数值还是等于NA，大部分数值函数都有na.rm=TRUE选项，用于计算前删除缺失值，使用剩余值计算
  > sum(y)
  [1] NA
  > sum(y, na.rm=TRUE)
  [1] 12
  > 
  # 或者使用na.omit() 删除含有缺失值数据的行或缺失值
  > newy <- na.omit(y)
  > newy
  [1] 2 4 6
  #attr(,"na.action")
  [1] 4
  #attr(,"class")
  [1] "omit"
  > sum(newy)
  [1] 12
  #> 

- 日期值
  日期值通常以字符串的形式输入到R，然后转化成以数值形式存储的日期变量。
  字符转化为日期转化函数：as.Date(x, "input_format")，其中x是字符型数据，input_format给出了用于读入日期的适当格式，默认格式为yyyy-mm-dd
  日期转化为字符（很少用）as.character(Sys.Date())
  日期格式如下图

 =================== ================================ ========================
  符号                含义                              样例
 =================== ================================ ========================
  %d                  数字表示的日期（0~31）            01 ~ 31
  %a                  缩写的星期名                      Mon
  %A                  非缩写的星期名                    Monday
  %m                  月份                              00 ~ 12
  %b                  缩写的月份                        Jan
  %B                  非缩写的月份                      January
  %y                  两位数的年份                      07
  %Y                  四位数的年份                      2007
 =================== ================================ ========================

  返回当天日期函数：Sys.Date()
  返回当天日期和时间函数：Sys.time()
  返回当前的日期和时间：date()
  了解更多的字符型数据转换为日期：help(as.Date)、help(strftime)

.. code:: r

  > Sys.Date()
  [1] "2019-11-24"
  > date()
  [1] "Sun Nov 24 10:36:04 2019"
  > today <- Sys.Date()
  > today
  [1] "2019-11-24"
  > format(today, format= "%B %A %Y")
  [1] "十一月 星期日 2019"
  > format(today, format= "%d")
  [1] "24"
  > format(today, format= "%a")
  [1] "周日"
  > 
  # 计算时间间隔
  > startdate <- as.Date("2019-11-18")
  > days <- today - startdate
  > days
  Time difference of 6 days
  > difftime(today, startdate, units="weeks")
  Time difference of 0.8571429 weeks
  > difftime(today, startdate, units="days")
  Time difference of 6 days
  > difftime(today, startdate, units="hours")
  Time difference of 144 hours
  # difftime units 应当是“auto”, “secs”, “mins”, “hours”, “days”, “weeks”其中的一个
  > difftime(today, startdate, units="mins")
  Time difference of 8640 mins
  > difftime(today, startdate, units="secs")
  Time difference of 518400 secs
  > 
  > a = as.character(Sys.Date())
  > typeof(a)
  [1] "character"
  > a
  [1] "2019-11-24"
  > 

- 数据排序
  对数据集iris的Sepal.Length排序，获得新的数据集newiris

.. code:: r

  > newiris <- iris[order(iris$Sepal.Length),]
  > newiris
      Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
  14           4.3         3.0          1.1         0.1     setosa
  9            4.4         2.9          1.4         0.2     setosa
  39           4.4         3.0          1.3         0.2     setosa
  43           4.4         3.2          1.3         0.2     setosa
  42           4.5         2.3          1.3         0.3     setosa
  4            4.6         3.1          1.5         0.2     setosa
  

- 数据集合并
  
  * 添加列： 横向合并两个数据框，使用merge()函数，如：total <- merge(dfA, dfB, by="ID")，如果直接横向合并两个矩阵或者数据框，并且不需要指定一个公共索引，直接使用cbind()函数。 total<- cbind(A, B)，cbind()要求每个对象必须有相同的行数且以相同的顺序排序
  * 添加行： 纵向合并两个数据框（数据集），使用rbind()。如：total <- rbind(dfA, dfB)，两个数据框必须拥有相同的变量，顺序不一定要相同

- 数据集取子集
  数据框的元素通过 dataframe[row indices, column indices]这样的记号
  建议采用subset()选择变量和观测的最简单方法
  随机抽样：sample()

.. code:: r

  > newiris_1 <- newiris[, c(4:5)]
  > newiris_1
      Petal.Width    Species
  14          0.1     setosa
  9           0.2     setosa
  39          0.2     setosa
  > newiris_2 <- newiris[1:3]
  > newiris_2
      Sepal.Length Sepal.Width Petal.Length
  14           4.3         3.0          1.1
  9            4.4         2.9          1.4
  # 取Sepal.Length<=4.4的两列数据集
  >iris_3 <- subset(iris, Sepal.Length <=4.4, select=c(Sepal.Length,Species))
  > newiris_3
     Sepal.Length Species
  9           4.4  setosa
  14          4.3  setosa
  39          4.4  setosa
  43          4.4  setosa
  > 
  >iris_3 <- subset(iris, Sepal.Length <=4.4, select=c(Sepal.Length:Species))
  > typeof(newiris_3)
  [1] "list"
  > newiris_3
     Sepal.Length Sepal.Width Petal.Length Petal.Width Species
  9           4.4         2.9          1.4         0.2  setosa
  14          4.3         3.0          1.1         0.1  setosa
  39          4.4         3.0          1.3         0.2  setosa
  43          4.4         3.2          1.3         0.2  setosa
  > newiris_5 <- subset(iris, Sepal.Length <=4.4, select=Sepal.Length:Species)
  > newiris_5
     Sepal.Length Sepal.Width Petal.Length Petal.Width Species
  9           4.4         2.9          1.4         0.2  setosa
  14          4.3         3.0          1.1         0.1  setosa
  39          4.4         3.0          1.3         0.2  setosa
  43          4.4         3.2          1.3         0.2  setosa
  # 随机抽样
  > iris_sample <- iris[sample(1:nrow(iris), 3, replace=FALSE),]
  > iris_sample
      Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
  107          4.9         2.5          4.5         1.7 virginica
  21           5.4         3.4          1.7         0.2    setosa
  17           5.4         3.9          1.3         0.4    setosa
  > 

- SQL 操作数据框
  在数据框上使用SQL的select语句，sqldf()

.. code:: r

  > iris_sqldf <- sqldf("select * from iris where Species='virginica'", row.name=TRUE)
  > iris_sqldf
      Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
  101          6.3         3.3          6.0         2.5 virginica
  102          5.8         2.7          5.1         1.9 virginica
  103          7.1         3.0          5.9         2.1 virginica

4. 高级数据管理
~~~~~~~~~~~~~~~~~~~~~~~~~

  * 数据处理难题
  * 数值和字符处理函数
  * 数据处理难题的解决方案
  * 处理缺失数据的高级方法

- 数学函数

  R基础的数据处理函数，可以分为数值（数学、统计、概率）函数和字符处理函数。

  * 常用数学函数
    绝对值abs()，平方根sqrt()，不小于x的最小整数ceiling(x)，不大于x的最大证书floor(x)，向0的方向截取x中的整数部分trunc(x)，将x舍入为指定位的小数round(x, digits=n)，将x摄入为指定的有效数字位数signif(x, digits=n)，对x取以n为底的对数log(x, base=n) 

  * 统计函数
    平均数mean，中位数median，标准差sd，方差var，绝对中位差mad，分位数quantile，值域range，求和sum，滞后差分diff，最小值min，最大值max，为数据对象x按列进行中心化(center=TRUE)或标准化scale(x, center=TRUE, scale=TRUE)
    默认情况下，函数scale()对矩阵或数据框的指定列进行均值为0，标准差为1的标准化，非常有用。在非数值型列上使用scale()会报错
    newdata <- transform(mydata, myvar = scale(myvar)*SD+M) 将变量myvar标准化为均值为M、标准差为SD的变量

  * 概率函数
    概率函数通常用来生成特征已知的模拟数据，以及在用户编写的统计函数中计算概率值。
    [dpqr] distribution_abbreviation()
    其中第一个字母表示其所分布的某一方面:
    d = 密度函数（density）
    p = 分布函数（distribution function）
    q = 分位数函数（quantile function）
    r = 生成随机数（随机偏差）

 =================== ================================ 
  缩写                分布名称                           
 =================== ================================
  beta                Beta分布        
  binom               二项分布
  cauchy              柯西分布
  chisq               （非中心）卡方分布
  exp                 指数分布
  f                   F分布
  gamma               Gamma分布
  geom                几何分布
  hyper               超几何分布
  lnorm               对数正态分布
  logis               Logistic分布
  multinom            多项分布
  nbinom              负二项分布
  norm                正态分布
  pois                泊松分布
  signrank            Wilcoxon符号轶分布
  t                   t分布
  unif                均匀分布
  weibull             Weibull分布
  wilcox              Wilcoxon轶和分布
 =================== ================================


- 数据清洗

  常用字符处理函数：
      计算x中字符数量nchar(x)
      提取或替换一个字符向量中的子串substr(x, start, stop)
      在x中搜索某种模式grep(pattern, x, ignore, case=FALSE, fixed=FALSE)，若fixed=FALSE，则pattern为一个正则表达式。若fixed=TRUE，则pattern为一个文本字符串，返回值为匹配的下标
      在x中搜索某种模式，并以文本replacement替换，sub(pattern, replacement, x, ignore.case=FALSE, fixed=FALSE)，若fixed=FALSE，则pattern为一个正则表达式。若fixed=TRUE，则pattern为一个文本字符串
      在split处分割字符向量x中的元素，fixed如grep与sub，strsplit(x, split, fixed=FALSE)
      连接字符串paste(..., sep="")，sep为分隔符
      大写转换toupper(x)
      小写转换tolower(x)

.. Tip::
  
  函数grep()、sub()和strsplit()能够搜索某个文本字符串（fixed=TRUE）或某个正则表达式（fixed=FALSE，默认为FALSE），正则表达式为文本模式匹配提供了一套清晰简练的语法，了解更多参考正则表达式regular expression

.. code:: r
  
  > x <- c('ab','cde','fghijk')
  > x
  [1] "ab"     "cde"    "fghijk"
  > length(x)
  [1] 3
  > x[3]
  [1] "fghijk"
  > nchar(x[2])
  [1] 3
  > nchar(x[3])
  [1] 6
  > nchar(x[1])
  [1] 2
  > substr(x[3], 2,5)
  [1] "ghij"
  > 
  > grep("A", c("b","A","c","A"), fixed=TRUE)
  [1] 2 4
  > grep("A", c("b","A","c"), fixed=TRUE)
  [1] 2
  
  > paste("y", 1:3, sep="-")
  [1] "y-1" "y-2" "y-3"
  > 

 
  其他实用函数
    对象x的长度：length(x)
    生成一个序列：seq(from, to, by)
    将x重复n次：rep(x,n)
    创建美观的分割点：pretty(x, n)
    连接...中的对象：cat(..., file="myfile", append=FALSE)

.. code:: r
  
  > seq(1,10,5)
  [1]  1  6 11 16
  
  > rep(1:3,2)
  [1] 1 2 3 1 2 3
  > rep(5,10)
   [1] 5 5 5 5 5 5 5 5 5 5
  > 
  > cut(1:10,5)
   [1] (0.991,2.8] (0.991,2.8] (2.8,4.6]   (2.8,4.6]   (4.6,6.4]   (4.6,6.4]  
   [7] (6.4,8.2]   (6.4,8.2]   (8.2,10]    (8.2,10]   
  Levels: (0.991,2.8] (2.8,4.6] (4.6,6.4] (6.4,8.2] (8.2,10]
  > pretty(x, 2)
  [1]  0  5 10
  > pretty(x, 5)
  [1]  0  2  4  6  8 10
  > firstname <- c("Vic")
  > firstname
  [1] "Vic"
  > cat("hello", firstname, "\n")
  hello Vic 
  > 


- 将函数应用于矩阵和数据框
  
  R中提供了apply()函数，可以将任意函数“应用”到矩阵、数组、数据框的任何维度，格式如下：
  apply(x, MARGIN, FUN, ...) x为数据对象，MARGIN是维度下标（矩阵或数据框MARGIN=1表示行，MARGIN=2表示列），FUN是由你指定的函数，而...包含了任何想要传递给FUN的参数
  apply()可把函数应用到数组/矩阵/数据框的某个维度上
  lapply()与sapply()则将函数应用到列表list上
  将一个求均值的函数应用到矩阵所有行与所有列，样例代码如下：

.. code:: r

  > sfdata <- matrix(rnorm(20), nrow=5)
  > sfdata
             [,1]        [,2]         [,3]        [,4]
  [1,]  0.6531234 -1.44524449  1.020830090  0.04615841
  [2,] -3.3208655 -0.49716791  0.009787577 -1.36340204
  [3,] -0.1022215 -0.08201499  0.067866033  0.34209709
  [4,]  0.1355523 -0.83764166  1.420953707  0.25286233
  [5,]  0.5651459 -0.03760827 -0.713492718  0.48885481
  # 计算5行均值
  > apply(sfdata, 1, mean)
  [1]  0.06871686 -1.29291197  0.05643166  0.24293166  0.07572492
  # 计算4列均值
  > apply(sfdata, 2, mean)
  [1] -0.41385309 -0.57993547  0.36118894 -0.04668588
  # 计算截尾均值，截尾均值位于中间60%的数据，最高和最高20%的值均被忽略
  > apply(sfdata, 2, mean, trim=0.2)
  [1]  0.1994922 -0.4722749  0.3661612  0.2137059
  > 
  

5. 总结数据
~~~~~~~~~~~~~~~~~~~~~~

  分析两变量间关系的基本方法，包括相关性、t检验、卡方检验和非参数方法
  详细记录在下一章节：数据分析

参考：
    1. R in Action R语言实战
