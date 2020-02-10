机器学习代码样例
~~~~~~~~~~~~~~~~~~~~~


 机器学习算法分为三大类：监督学习、非监督学习、新兴机器学习

 监督学习中，主要有三大类预测输出变量的类型：
 
     1. 预测数值变量：如房价
     2. 预测类成员：预测类别变量，如会不会采购？
     3. 倾向性预测

 关于分类预测，要注意分类器的两种给不同的预测用法：
        1. 分类：目标是用于预测新记录属于哪个类成员；
        2. 排序：目标是在一组新记录中检测最有可能属于某个兴趣类别的记录，按概率排序；


 ============================= ======================================================================================
      类别                        算法
 ============================= ======================================================================================
     监督学习                   线性回归，逻辑回归，kNN k近邻，决策树，随机森林Random Forest
     非监督学习                 k-Means k均值，聚类，关联规则Apriori，协同过滤
     新兴机器学习               半监督学习，强化学习，马尔科夫决策过程，Q Learning
 ============================= ======================================================================================


- 线性回归
 

 ============================================================= ========================================================
                        Python 代码                                             R 代码
 ============================================================= ========================================================
       # Import 相关的库                                             # 引入相关库 library()
        # 获取数据集（训练集、验证集、测试集）                       # 获取数据集（训练集、验证集、测试集） 
        # 确定特征变量与应变量                                        # 确定特征变量与应变量
       # 值必须是数值型                                              # 值必须是数值型
       from sklearn import linear_model
       x_train = input_variables_values_training_datasets            x_train <- input_variables_values_training_datasets
       y_train = target_variables_values_training_datasets           y_train <- target_variables_values_training_datasets
         x_test = input_variables_values_test_datasets                 x_test  <- input_variables_values_test_datasets
         # 创建线性回归对象                                            
         linear = linear_model.LinearRegression()                      x<- cbind(x_train, y_train)
         # 利用训练集训练模型                                          # 利用训练集训练模型
         # 检查分数                                                    # 检查分数
        linear.fit(x_train, y_train)                                  linear <- lm(y_train ~ ., data = x)
         linear.score(x_train, y_train)                                summary(linear)

         # 预测输出                                                    # 预测输出
         predicted = linear.predict(x_test)                            predicted = predict(linear, x_test)

 ============================================================= ========================================================


.. code:: r 

 # 读取丰田车数据
 car.df <- read.csv("D://Books//bap//ToyotaCorolla.csv")
 head(car.df)
 dim(car.df)
 # 查看行记录数
 dim(car.df[,1])
 dim(car.df)[1]
 length(car.df[,1])
 # 取1000条记录
 car.df1k <- car.df[1:1000,]
 head(car.df1k)
 dim(car.df1k)
 # 查看列名
 colnames(car.df1k)
 t(t(colnames(car.df1k)))
 # 去相应的列
 selected.var <- c(3,4,7,8,9,10,12,13,14,17,18)
 # 设置种子
 set.seed(2)
 # 随机采样取600条记录
 train.index <- sample(c(1:1000), 600)
 head(train.index)
 # 设定训练集与验证集
 train.df1k <- car.df1k[train.index, selected.var]
 head(train.df1k)
 valid.df1k <- car.df1k[-train.index, selected.var]
 # 线性回归模型lm算法来分析价格与各列的关系
 car.lm <- lm(Price ~., data = train.df1k)
 summary(car.lm)
 # options scipen取消科学计数法
 options(scipen = 999)
 summary(car.lm)
 # 加载预测包
 library(forecast)
 install.packages("forecast")
 library(forecast)
 summary(car.lm)
 car.lm.pred <- predict(car.lm, valid.df1k)
 options(scipen=999, digits = 0)
 ?options
 accuracy(car.lm.pred, valid.df1k$Price)
 head(car.lm.pred[1:20])
 colnames(car.lm.pred)
 summary(car.lm.pred)
 car.lm.pred 
 # 生成预测值与实际值的数据框
 x <- data.frame("Predicted" = car.lm.pred[1:20], "Actual_Price" = valid.df1k$Price[1:20])
 x['Residual'] <- x['Actual_Price'] - x['Predicted']
 # 残差计算
 all.residuals <- valid.df1k$Price - car.lm.pred
 # 预览直方图
 hist(all.residuals, breaks=200, xlab="Residuals", main = "")
 # 根据summary(car.lm.pred)数据设置breaks
 breaks = c(-5900,-800,0,100,800,50000)
 # xlim - 设置横坐标, freq=T为频数，FALSE为密度面积 参考下图
 hist(all.residuals, breaks=breaks, xlab="Residuals", freq = FALSE, main = "", col = "pink", xlim = c(-10000,60000))
 hist(all.residuals, breaks=breaks, xlab="Residuals", freq = TRUE, main = "", col = "pink", xlim = c(-10000,60000))

 # 使用regsubsets()来执行穷尽搜索
 # 类别预测因子必须要手工转换成虚拟变量
 library(leaps)
 head(car.df1k)
 # 创建Fule_type的虚拟变量
 Fuel_Type <- as.data.frame(model.matrix( ~ 0 + Fuel_Type, car.df1k))
 head(Fuel_Type)
 # 使用3个虚拟变量列替换Fuel_Type列
 train.car.df1k <- cbind(car.df1k[, -4], Fuel_Type)
 head(train.car.df1k)
 > search <- regsubsets(Price ~ ., data = train.car.df1k, nbest = 1, nvmax = dim(train.car.df1k)[2], method = 'exhaustive', really.big = TRUE)
 Reordering variables and trying again:
 # 运行时间超长。不适合使用R在本地服务器执行穷尽搜索



下图为残差密度直方图：

.. image:: _static/hist_lm_density.PNG
   :align: center



* 预测因子（变量）选择
  线性回归中预测因子选择非常重要，直接决定了模型的正确性。减少自变量（预测因子）的数量有两种方法：
  1. 穷尽搜索Exhaustive Search：不建议在R中使用，效率极低，速度极慢
  2. 共有三种流行的迭代搜索算法：前向选择forward selection，后向评估backward elimination，逐步回归stepwise regression


.. code:: r

 # 使用step()运行逐步回归
 # set directions = "forward" "backward" "both"
 car.lm.step.both <- step(car.lm, direction = "both")
 summary(car.lm.step.both)
 # 检查哪一个变量应该删除，此统计结果给出了需要保留的六个特征
 Call:
 ## 如下是重点
 lm(formula = Price ~ Age_08_04 + KM + Fuel_Type + HP + Quarterly_Tax + 
     Weight, data = train.df1k)
 
 Residuals:
     Min      1Q  Median      3Q     Max 
 -8959.9  -833.1   -16.8   835.3  5058.9 
 # 其他逐步回归方法结果 
 car.lm.step.forward <- step(car.lm, direction = "forward")
 summary(car.lm.step.forward)
 car.lm.step.backward <- step(car.lm, direction = "backward")
 summary(car.lm.step.backward)
  
 # 线性模型准确性检查，对比不同模型的ME, RMSE, MAE, MPE, MAPE值
 # 根据选择的6特征变量的模型来预测验证，检查模型预测的准确性
 car.lm.step.pred <- predict(car.lm.step.both, valid.df1k)
 > accuracy(car.lm.step.pred, valid.df1k$Price)
               ME     RMSE      MAE        MPE     MAPE
 Test set 59.26886 1334.978 1024.979 -0.4212544 9.357492 


-----------------------------------------------------------------------

- 朴素贝叶斯分类
  
  完全（精确）贝叶斯计算与朴素贝叶斯计算样例：
     1. 用10条记录计算有法律麻烦的大小公司欺诈的可能性 - 贝叶斯 & 朴素贝叶斯（样例1）
     2. 利用FlightDelays.csv数据集来预测航班误点可能性 - 朴素贝叶斯（样例2）
 
 
.. code:: r


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



-----------------------


.. code:: r 


 # 以航班数据，预测航班误点可能性（朴素贝叶斯）
 > library(e1071)
 > delays.df <- read.csv("D:\\Books\\dataset\\FlightDelays.csv")
 > delays.df['dayweek'] <- factor(delays.df$dayweek)
 > head(delays.df)
    schedtime carrier deptime dest distance     date flightnumber origin weather dayweek daymonth tailnu  delay com_deptime
 1      1455      OH      15  JFK      184 1/1/2004         5935    BWI       0       4        1 N940CA ontime          15
 2      1640      DH      16  JFK      213 1/1/2004         6155    DCA       0       4        1 N405FJ ontime          16
 3      1245      DH      12  LGA      229 1/1/2004         7208    IAD       0       4        1 N695BR ontime          12
 4      1715      DH      17  LGA      229 1/1/2004         7215    IAD       0       4        1 N662BR ontime          17
 5      1039      DH      10  LGA      229 1/1/2004         7792    IAD       0       4        1 N698BR ontime          10
 6       840      DH       8  JFK      228 1/1/2004         7800    IAD       0       4        1 N687BR ontime           8
 > selected.var <- c(2,3,4,8,10,13)
 > train.df <- delays.df[train.index, selected.var]
 > valid.df <- delays.df[-train.index, selected.var]
 > delays.nb <- naiveBayes(delay ~ ., data = train.df)
 > delays.nb
 
 Naive Bayes Classifier for Discrete Predictors
 
 Call:
 naiveBayes.default(x = X, y = Y, laplace = laplace)
 
 A-priori probabilities:
 Y
   delayed    ontime 
 0.2015152 0.7984848 
 
 Conditional probabilities:
          carrier
 Y                  CO          DH          DL          MQ          OH          RU          UA          US
   delayed 0.056390977 0.285714286 0.090225564 0.206766917 0.011278195 0.248120301 0.007518797 0.093984962
   ontime  0.035104364 0.235294118 0.187855787 0.115749526 0.017077799 0.179316888 0.018026565 0.211574953
 
          deptime
 Y                   0           1           5           6           7           8           9          10          11          12          13
   delayed 0.003759398 0.000000000 0.000000000 0.026315789 0.067669173 0.037593985 0.033834586 0.022556391 0.033834586 0.018796992 0.045112782
   ontime  0.000000000 0.000000000 0.000000000 0.065464896 0.062618596 0.066413662 0.059772296 0.055028463 0.034155598 0.069259962 0.068311195
          deptime
 Y                  14          15          16          17          18          19          20          21          22          23
   delayed 0.033834586 0.176691729 0.101503759 0.071428571 0.078947368 0.086466165 0.063909774 0.041353383 0.052631579 0.003759398
   ontime  0.061669829 0.100569260 0.089184061 0.092979127 0.045540797 0.038899431 0.025616698 0.064516129 0.000000000 0.000000000
 
          dest
 Y               EWR       JFK       LGA
   delayed 0.4060150 0.1992481 0.3947368
   ontime  0.2865275 0.1707780 0.5426945
 
          origin
 Y                BWI        DCA        IAD
   delayed 0.09774436 0.52255639 0.37969925
   ontime  0.06261860 0.63662239 0.30075901
 
          dayweek
 Y                  1          2          3          4          5          6          7
   delayed 0.20676692 0.14285714 0.14661654 0.11278195 0.15413534 0.06390977 0.17293233
   ontime  0.12618596 0.14516129 0.14421252 0.17077799 0.18121442 0.13092979 0.10151803
 
 > dim(train.df)
 [1] 1320    6
 > dim(valid.df)
 [1] 881   6
 > dim(delays.df)
 [1] 2201   14
 > P(delayed|Carrier=DL, Day_Week=7, Dep_Time=10, Dest=LGA, Origin=DCA
 Error: unexpected '=' in "P(delayed|Carrier="
 > train.df.DL <- subset(train.df, train.df$carrier == 'DL')
 > length(train.df.DL)
 [1] 6
 > dim(train.df.DL)
 [1] 222   6
 > dim(subset(train.df, train.df$dayweek == '7'))[1]/1320
 [1] 0.1159091
 > dim(subset(train.df, train.df$carrier == 'DL'))[1]/1320
 [1] 0.1681818
 > dim(subset(train.df, train.df$deptime == '10'))[1]/1320
 [1] 0.04848485
 > dim(subset(train.df, train.df$dest == 'LGA'))[1]/1320
 [1] 0.5128788
 > dim(subset(train.df, train.df$ORI == 'LGA'))[1]/1320
 [1] 0
 > dim(subset(train.df, train.df$origin == 'DCA'))[1]/1320
 [1] 0.6136364
 > 0.1159091 * 0.1681818 * 0.04848485 * 0.5128788 * 0.6136364
 [1] 0.0002974599
 > 
 
 
 > df <- read.csv("D:\\Books\\HKU-BusinessAnalytics\\dataset\\FlightDelays.csv")
 > df$deptime <- round(df$deptime/100)
 > head(df)
   schedtime carrier deptime dest distance     date flightnumber origin weather dayweek daymonth tailnu  delay
 1      1455      OH      15  JFK      184 1/1/2004         5935    BWI       0       4        1 N940CA ontime
 2      1640      DH      16  JFK      213 1/1/2004         6155    DCA       0       4        1 N405FJ ontime
 3      1245      DH      12  LGA      229 1/1/2004         7208    IAD       0       4        1 N695BR ontime
 4      1715      DH      17  LGA      229 1/1/2004         7215    IAD       0       4        1 N662BR ontime
 5      1039      DH      10  LGA      229 1/1/2004         7792    IAD       0       4        1 N698BR ontime
 6       840      DH       8  JFK      228 1/1/2004         7800    IAD       0       4        1 N687BR ontime
 > df$deptime <- factor(df$deptime)
 > df$dayweek <- factor(df$dayweek)
 > head(df)
   schedtime carrier deptime dest distance     date flightnumber origin weather dayweek daymonth tailnu  delay
 1      1455      OH      15  JFK      184 1/1/2004         5935    BWI       0       4        1 N940CA ontime
 2      1640      DH      16  JFK      213 1/1/2004         6155    DCA       0       4        1 N405FJ ontime
 3      1245      DH      12  LGA      229 1/1/2004         7208    IAD       0       4        1 N695BR ontime
 4      1715      DH      17  LGA      229 1/1/2004         7215    IAD       0       4        1 N662BR ontime
 5      1039      DH      10  LGA      229 1/1/2004         7792    IAD       0       4        1 N698BR ontime
 6       840      DH       8  JFK      228 1/1/2004         7800    IAD       0       4        1 N687BR ontime
 > t(t(colnames(df)))
       [,1]          
  [1,] "schedtime"   
  [2,] "carrier"     
  [3,] "deptime"     
  [4,] "dest"        
  [5,] "distance"    
  [6,] "date"        
  [7,] "flightnumber"
  [8,] "origin"      
  [9,] "weather"     
 [10,] "dayweek"     
 [11,] "daymonth"    
 [12,] "tailnu"      
 [13,] "delay"       
 > selected.var <- c(10,3,8,4,2)
 > delay.train.index <- sample(c(1:dim(df)[1], dim(df)[1] * 0.6 ))
 > dim(delay.train.index)
 NULL
 > length(delay.train.index)
 [1] 2202
 > dim(df)
 [1] 2201   13
 > delay.train.index <- sample(c(1:dim(df)[1]), dim(df)[1] * 0.6 )
 > length(delay.train.index)
 [1] 1320
 > 2202 * 0.6
 [1] 1321.2
 > delay.train.data <- df[delay.train.index, selected.var]
 > delay.valid.data <- df[-delay.train.index, selected.var]
 > delay.naivebayes <- naiveBayes(delay ~ ., data = delay.train.data)
 Error in eval(predvars, data, env) : object 'delay' not found
 > selected.var <- c(10,3,8,4,2,13)
 > delay.train.data <- df[delay.train.index, selected.var]
 > delay.valid.data <- df[-delay.train.index, selected.var]
 > delay.naivebayes <- naiveBayes(delay ~ ., data = delay.train.data)
 > delay.naivebayes
 
 Naive Bayes Classifier for Discrete Predictors
 
 Call:
 naiveBayes.default(x = X, y = Y, laplace = laplace)
 
 A-priori probabilities:
 Y
   delayed    ontime 
 0.1931818 0.8068182 
 
 Conditional probabilities:
          dayweek
 Y                  1          2          3          4          5          6          7
   delayed 0.17254902 0.16078431 0.12549020 0.11764706 0.18431373 0.07450980 0.16470588
   ontime  0.12300469 0.14835681 0.14272300 0.17558685 0.18403756 0.13051643 0.09577465
 
          deptime
 Y                    0            1            5            6            7            8            9           10           11           12
   delayed 0.0039215686 0.0039215686 0.0000000000 0.0235294118 0.0588235294 0.0470588235 0.0235294118 0.0196078431 0.0235294118 0.0156862745
   ontime  0.0000000000 0.0000000000 0.0009389671 0.0647887324 0.0535211268 0.0760563380 0.0629107981 0.0488262911 0.0450704225 0.0685446009
          deptime
 Y                   13           14           15           16           17           18           19           20           21           22
   delayed 0.0666666667 0.0431372549 0.1215686275 0.1294117647 0.0784313725 0.0745098039 0.1058823529 0.0745098039 0.0235294118 0.0549019608
   ontime  0.0610328638 0.0704225352 0.0948356808 0.0835680751 0.1032863850 0.0394366197 0.0431924883 0.0291079812 0.0544600939 0.0000000000
          deptime
 Y                   23
   delayed 0.0078431373
   ontime  0.0000000000
 
          origin
 Y                BWI        DCA        IAD
   delayed 0.08235294 0.51764706 0.40000000
   ontime  0.05727700 0.65164319 0.29107981
 
          dest
 Y               EWR       JFK       LGA
   delayed 0.3450980 0.2039216 0.4509804
   ontime  0.2920188 0.1774648 0.5305164
 
          carrier
 Y                  CO          DH          DL          MQ          OH          RU          UA          US
   delayed 0.054901961 0.333333333 0.098039216 0.207843137 0.011764706 0.207843137 0.003921569 0.082352941
   ontime  0.036619718 0.243192488 0.200000000 0.119248826 0.014084507 0.176525822 0.013145540 0.197183099
 
 > prop.table(table(delay.train.data$delay, delay.train.data$dest, delay.train.data$origin), margin = 2)
 , ,  = BWI
 
          
                  EWR        JFK        LGA
   delayed 0.04511278 0.01244813 0.00000000
   ontime  0.11528822 0.06224066 0.00000000
 
 , ,  = DCA
 
          
                  EWR        JFK        LGA
   delayed 0.07769424 0.09958506 0.11323529
   ontime  0.32832080 0.28215768 0.72794118
 
 , ,  = IAD
 
          
                  EWR        JFK        LGA
   delayed 0.09774436 0.10373444 0.05588235
   ontime  0.33583960 0.43983402 0.10294118
 
 > prop.table(table(delay.train.data$delay, delay.train.data$dest), margin = 1)
          
                 EWR       JFK       LGA
   delayed 0.3450980 0.2039216 0.4509804
   ontime  0.2920188 0.1774648 0.5305164
 > P.delayed.case <- (0.98039216 * 0.16470588 * 0.0196078431 * 0.4509804 * 0.51764706) * 0.1931818
 > P.delayed.case
 [1] 0.0001427895
 > P.ontime.case <- (0.200000000 * 0.09577465 * 0.0488262911 * 0.5305164 * 0.65164319) * 0.8068182
 > P.ontime.case
 [1] 0.0002608667
 > P.delayed <- (P.delayed.case)/(P.delayed.case + P.ontime.case)
 > P.delayed
 [1] 0.3537404
 > P.ontime <- P.ontime.case / (P.delayed.case + P.ontime.case)
 > P.ontime
 [1] 0.6462596
 > ls()
  [1] "a"                 "age"               "b"                 "c"                 "count"             "d"                 "data"             
  [8] "data_index"        "data_indexed"      "delay.naivebayes"  "delay.train.data"  "delay.train.index" "delay.valid.data"  "delays.df"        
 [15] "delays.nb"         "df"                "df.nb"             "df.nb2"            "df_nb"             "df_text"           "df1"              
 [22] "df2"               "dflarge"           "dfNlarge"          "dfNsmall"          "dfsmall"           "fp"                "housing.df"       
 [29] "i"                 "o"                 "P.delayed"         "P.delayed.case"    "P.ontime"          "P.ontime.case"     "rows.missing10"   
 [36] "s"                 "selected.var"      "test2.data"        "test2.rows"        "train.data"        "train.df"          "train.df.DL"      
 [43] "train.index"       "train.rows"        "train2.data"       "train2.rows"       "v"                 "VADeaths"          "vald.data"        
 [50] "valid.data"        "valid.df"          "valid.rows"        "valid2.data"       "valid2.rows"       "weight"            "x"                
 [57] "xtotal"            "xtotal.df"         "y"                
 > pred.prob <- predict(delay.naivebayes, newdata = delay.valid.data, type="raw")
 > pred.class <- predict(delay.naivebayes, newdata = delay.valid.data)
 > df <- data.frame(actual = delay.valid.data$delay, predicted = pred.class, pred.prob)
 > head(df)
   actual predicted    delayed    ontime
 1 ontime    ontime 0.16321988 0.8367801
 2 ontime    ontime 0.09350922 0.9064908
 3 ontime    ontime 0.07361168 0.9263883
 4 ontime    ontime 0.09989439 0.9001056
 5 ontime    ontime 0.03150436 0.9684956
 6 ontime    ontime 0.03876232 0.9612377
 > head(pred.class)
 [1] ontime ontime ontime ontime ontime ontime
 Levels: delayed ontime
 > df[delay.valid.data$carrier == 'DL' & delay.valid.data$dayweek == 7 & delay.valid.data$deptime == 10 & delay.valid.data$dest == "LGA" & delay.valid.data$origin == 'DCA',]
 [1] actual    predicted delayed   ontime   
 <0 行> (或0-长度的row.names)
 > HEAD(DF)
 Error in HEAD(DF) : could not find function "HEAD"
 > head(df)
   actual predicted    delayed    ontime
 1 ontime    ontime 0.16321988 0.8367801
 2 ontime    ontime 0.09350922 0.9064908
 3 ontime    ontime 0.07361168 0.9263883
 4 ontime    ontime 0.09989439 0.9001056
 5 ontime    ontime 0.03150436 0.9684956
 6 ontime    ontime 0.03876232 0.9612377
 > df <- data.frame(delay.valid.data, actual = delay.valid.data$delay, predicted = pred.class, pred.prob)
 > head(df)
    dayweek deptime origin dest carrier  delay actual predicted    delayed    ontime
 4        4      17    IAD  LGA      DH ontime ontime    ontime 0.16321988 0.8367801
 5        4      10    IAD  LGA      DH ontime ontime    ontime 0.09350922 0.9064908
 7        4      12    IAD  JFK      DH ontime ontime    ontime 0.07361168 0.9263883
 11       4      21    IAD  LGA      DH ontime ontime    ontime 0.09989439 0.9001056
 15       4      14    DCA  LGA      DL ontime ontime    ontime 0.03150436 0.9684956
 16       4      17    DCA  LGA      DL ontime ontime    ontime 0.03876232 0.9612377
 > df[delay.valid.data$carrier == 'DL' & delay.valid.data$dayweek == 7 & delay.valid.data$deptime == 10 & delay.valid.data$dest == "LGA" & delay.valid.data$origin == 'DCA',]
  [1] dayweek   deptime   origin    dest      carrier   delay     actual    predicted delayed   ontime   
 > df
     dayweek deptime origin dest carrier   delay  actual predicted    delayed     ontime
 4         4      17    IAD  LGA      DH  ontime  ontime    ontime 0.16321988 0.83678012
 5         4      10    IAD  LGA      DH  ontime  ontime    ontime 0.09350922 0.90649078
 7         4      12    IAD  JFK      DH  ontime  ontime    ontime 0.07361168 0.92638832
 11        4      21    IAD  LGA      DH  ontime  ontime    ontime 0.09989439 0.90010561
 15        4      14    DCA  LGA      DL  ontime  ontime    ontime 0.03150436 0.96849564
 16        4      17    DCA  LGA      DL  ontime  ontime    ontime 0.03876232 0.96123768
 20        4      18    DCA  JFK      MQ  ontime  ontime    ontime 0.32533864 0.67466136
 
 
 
 > library(caret)
 载入需要的程辑包：lattice
 载入需要的程辑包：ggplot2
 Warning messages:
 1: 程辑包‘caret’是用R版本3.6.2 来建造的 
 2: 程辑包‘ggplot2’是用R版本3.6.2 来建造的 
 > confusionMatrix(pred.class, delay.valid.data$delay)
 Confusion Matrix and Statistics
 
           Reference
 Prediction delayed ontime
    delayed      25     12
    ontime      148    696
                                           
                Accuracy : 0.8184          
                  95% CI : (0.7913, 0.8433)
     No Information Rate : 0.8036          
     P-Value [Acc > NIR] : 0.1443          
                                           
                   Kappa : 0.1815          
                                           
  Mcnemar's Test P-Value : <2e-16          
                                           
             Sensitivity : 0.14451         
             Specificity : 0.98305         
          Pos Pred Value : 0.67568         
          Neg Pred Value : 0.82464         
              Prevalence : 0.19637         
          Detection Rate : 0.02838         
    Detection Prevalence : 0.04200         
       Balanced Accuracy : 0.56378         
                                           
        'Positive' Class : delayed         
                                           
 > pred.class.train <- predict(delay.naivebayes, newdata = delay.train.data)
 > confusionMatrix(pred.class.train, delay.train.data$delay)
 Confusion Matrix and Statistics
 
           Reference
 Prediction delayed ontime
    delayed      52     18
    ontime      203   1047
                                           
                Accuracy : 0.8326          
                  95% CI : (0.8113, 0.8523)
     No Information Rate : 0.8068          
     P-Value [Acc > NIR] : 0.008903        
                                           
                   Kappa : 0.2583          
                                           
  Mcnemar's Test P-Value : < 2.2e-16       
                                           
             Sensitivity : 0.20392         
             Specificity : 0.98310         
          Pos Pred Value : 0.74286         
          Neg Pred Value : 0.83760         
              Prevalence : 0.19318         
          Detection Rate : 0.03939         
    Detection Prevalence : 0.05303         
       Balanced Accuracy : 0.59351         
                                           
        'Positive' Class : delayed         
                                           
 > pred.class.valid <- predict(delay.naivebayes, newdata = delay.valid.data)
 > confusionMatrix(pred.class.valid, delay.valid.data$delay)
 Confusion Matrix and Statistics
 
           Reference
 Prediction delayed ontime
    delayed      25     12
    ontime      148    696
                                           
                Accuracy : 0.8184          
                  95% CI : (0.7913, 0.8433)
     No Information Rate : 0.8036          
     P-Value [Acc > NIR] : 0.1443          
                                           
                   Kappa : 0.1815          
                                           
  Mcnemar's Test P-Value : <2e-16          
                                           
             Sensitivity : 0.14451         
             Specificity : 0.98305         
          Pos Pred Value : 0.67568         
          Neg Pred Value : 0.82464         
              Prevalence : 0.19637         
          Detection Rate : 0.02838         
    Detection Prevalence : 0.04200         
       Balanced Accuracy : 0.56378         
                                           
        'Positive' Class : delayed         
                                           
 
