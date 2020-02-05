机器学习代码样例
~~~~~~~~~~~~~~~~~~~~~

 机器学习算法分为三大类：监督学习、非监督学习、新兴机器学习

 ============================= ======================================================================================
      类别                        算法
 ============================= ======================================================================================
     监督学习                   线性回归，逻辑回归，kNN k近邻，决策树，随机森林Random Forest
     非监督学习                 k-Means k均值，聚类，关联规则Apriori，协同过滤
     新兴机器学习               半监督学习，强化学习，马尔科夫决策过程，Q Learning
 ============================= ======================================================================================


- 回归
 

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


.. code:r 

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
 # xlim - 设置横坐标, freq=T为频数，FALSE为密度面积
 hist(all.residuals, breaks=breaks, xlab="Residuals", freq = FALSE, main = "", col = "pink", xlim = c(-10000,60000))
 hist(all.residuals, breaks=breaks, xlab="Residuals", freq = TRUE, main = "", col = "pink", xlim = c(-10000,60000))


-----------------------------------------------------------------------

- 聚类

 ============================================================= ========================================================
                   Python 代码                                             R 代码
 ============================================================= ========================================================
  # Import 导入相关库                                                # 引入相关库 Library()
 ============================================================= ========================================================



