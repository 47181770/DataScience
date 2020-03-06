样例- k-NN k近邻
~~~~~~~~~~~~~~~~~~~~~


- k-NN（k-Nearest Neighbors k近邻）
  
  k近邻算法是一种常用的监督学习算法，主要用于分类也可用于回归，是一个理论上成熟，应用最简单的机器学习算法之一。没有显式的训练过程，训练时间开销为零，是“懒惰学习 Lazy learning”的注明代表。应用领域在计算机视觉、推荐系统（如歌曲、影视等），数据缺失值预处理亦可。
  k近邻工作机制：

     1. 给定测试样本，基于某种距离度量（如欧氏距离等），计算已知数据集中的点与当前点的距离

     2. 按距离递增次序排序，找出训练集中与其最靠近的k个训练样本

     3. 基于这k个“邻居”的信息，选取与当前数据点最近的k个点（简而言之：给一个新的数据时，离它最近的 k 个点中，哪个类别多，这个数据就属于哪一类）

     4. “投票法”：选择k个样本中出现最多的类别标记作为预测结果

     5. “平均法”：回归任务中，将这k个样本的实值输出标记的平均值作为预测结果

     6. “加权平均”或“加权投票”：基于距离远近进行，距离越近，样本权重越大

  kNN算法优缺点如下：

 ============================= ======================================================================================
        优点                                                  缺点
 ============================= ======================================================================================
  简单且有效                           在发现特征之间关系上的能力有限，类别评分不像概率评分
  对数据的分布没有要求                 分类阶段慢
  训练阶段很快（训练与重训）           新记录预测，全部重计算复杂度高、空间复杂度高，需大量的内存，如何实时预测？
  泛化错误率不超贝叶斯2倍              样本不平衡时，预测偏差大
 ============================= ======================================================================================


- kNN预测代码样例


.. code:: r 

 # 使用RidingMovers割草机的数据集
 > mower.df <- read.csv("D:\\Books\\RidingMowers.csv")
 > head(mower.df)
   Income Lot_Size Ownership
 1   60.0     18.4     Owner
 2   85.5     16.8     Owner
 3   64.8     21.6     Owner
 4   61.5     20.8     Owner
 5   87.0     23.6     Owner
 6  110.1     19.2     Owner
 > dim(mower.df)
 [1] 24  3
 > set.seed(100)
 # 采样60%用于训练：总计24条记录，采样14条
 > train.index <- sample(row.names(mower.df), 0.6 * dim(mower.df)[1])
 > train.index
  [1] "10" "23" "6"  "16" "19" "14" "12" "22" "4"  "7"  "17" "2"  "15" "18"
 > length(train.index)
 [1] 14
 > 24 * 0.6
 [1] 14.4
 > valid.index <- setdiff(row.names(mower.df), train.index)
 > train.df <- mower.df[train.index,]
 > valid.df <- mower.df[valid.index,]
 > dim(train.df)
 [1] 14  3
 > dim(valid.df)
 [1] 10  3
 # 新家庭的收入Income 6万美元与Lot Size 2万平方英尺（约2000平米） 
 > new.df <- data.frame(Income=60, Lot_Size=20)
 # 画出Income与Lot Size分类的Owner+/Noowner散点 图
 > plot(Lot_Size ~ Income, data=train.df, pch=ifelse(train.df$Ownership=="Owner", 1, 3))
 > text(train.df$Income, train.df$Lot_Size, rownames(train.df), pos=4)
 # 在图上标识新家庭的特征
 > text(60, 20, "X")
 > legend("topright", c("Owner", "Nonowner", "NewerPredict"), pch=c(1,3,4))
 > head(mower.df)
   Income Lot_Size Ownership
 1   60.0     18.4     Owner
 2   85.5     16.8     Owner
 3   64.8     21.6     Owner
 4   61.5     20.8     Owner
 5   87.0     23.6     Owner
 6  110.1     19.2     Owner
 > mower.norm.df <- mower.df
 # preProcess函数在caret包
 > norm.values <- preProcess(train.df[, 1:2], method=c("center", "scale"))
 Error in preProcess(train.df[, 1:2], method = c("center", "scale")) : 
   could not find function "preProcess"
 > library(caret)
 载入需要的程辑包：lattice
 载入需要的程辑包：ggplot2
 Warning messages:
 1: 程辑包‘caret’是用R版本3.6.2 来建造的 
 2: 程辑包‘ggplot2’是用R版本3.6.2 来建造的 
 # 对数据预处理：归一化Normalize
 > norm.values <- preProcess(train.df[, 1:2], method=c("center", "scale"))
 > head(norm.values)
 $dim
 [1] 14  2
 
 $bc
 NULL
 > norm.values
 Created from 14 samples and 2 variables
 
 Pre-processing:
   - centered (2)
   - ignored (0)
   - scaled (2)
 # 对训练集归一化
 > train.norm.df[, 1:2] <- predict(norm.values, train.df[, 1:2])
 > train.norm.df
        Income   Lot_Size Ownership
 10  0.9663169  1.1596670     Owner
 23 -0.7792879 -2.1260562  Nonowner
 6   1.6770275  0.3865557     Owner
 16 -1.1034716  0.9663892  Nonowner
 # 对预测数据进行归一化 
 > new.norm.df <- predict(norm.values, new.df)
 # 加载knn程序包 
 > library(FNN)
 Warning message:
 程辑包‘FNN’是用R版本3.6.2 来建造的 

 > NN <- knn(train = train.norm.df[, 1:2], test = new.norm.df, cl=train.norm.df[, 3], k=3)
 > row.names(train.norm.df)[attr(NN, "nn.index")]
 [1] "4"  "14" "16"
 # 请查看如下图，在k=3时，离预测数据最近的3个点是 4 14 16 

 # 取不同k值，计算不同k值的模型分类准确率
 > library(caret)
 > train.norm.df <- train.df
 > valid.norm.df <- valid.df
 > mower.norm.df <- mower.df
 > train.norm.df[, 1:2] <- predict(norm.values, train.df[, 1:2])
 > valid.norm.df[, 1:2] <- predict(norm.values, valid.df[, 1:2])
 > mower.norm.df[, 1:2] <- predict(norm.values, mower.df[, 1:2])
 > accuracy.df <- data.frame(k = seq(1, 14,1), accuracy = rep(0, 14))
 > head(accuracy.df)
   k accuracy
 1 1        0
 2 2        0
 3 3        0
 4 4        0
 5 5        0
 6 6        0
 > knn.pred <- knn(train = train.norm.df[, 1:2], test = valid.norm.df[, 1:2], cl=train.norm.df[,3], k=3)
 > knn.pred
  [1] Nonowner Owner    Owner    Owner    Owner    Nonowner Owner    Nonowner Nonowner Nonowner
 attr(,"nn.index")
       [,1] [,2] [,3]
  [1,]   14   13   11
  [2,]    9    6    7
  [3,]    1    9    7
  [4,]    1    7    9
  [5,]    9    7    6
  [6,]    6    9    4
  [7,]    7    9    1
  [8,]   13   14   11
  [9,]    5   14   13
 [10,]    5    2   13
 attr(,"nn.dist")
            [,1]      [,2]      [,3]
  [1,] 0.5923761 0.6131935 1.0697703
  [2,] 0.4101667 0.6310080 1.0252026
  [3,] 1.3757348 1.7186339 1.7572844
  [4,] 0.8817142 1.1620776 1.1753323
  [5,] 0.4965799 0.4987442 0.7763791
  [6,] 0.5846398 0.7257086 0.8383294
  [7,] 0.3155040 0.8068617 0.9465120
  [8,] 0.5819745 0.7981022 0.8420827
  [9,] 0.5348852 0.5846398 0.8200081
 [10,] 0.5988272 0.6310080 1.1620776
 Levels: Nonowner Owner
 > confusionMatrix(knn.pred, valid.norm.df[,3])$overall[1]
 Accuracy 
      0.7 
 > confusionMatrix(knn.pred, valid.norm.df[,3])
 Confusion Matrix and Statistics
           Reference
 Prediction Nonowner Owner
   Nonowner        3     2
   Owner           1     4
                Accuracy : 0.7             
                  95% CI : (0.3475, 0.9333)
     No Information Rate : 0.6             
     P-Value [Acc > NIR] : 0.3823          
 > for (i in 1:14){
 +  knn.pred <- knn(train = train.norm.df[, 1:2], test = valid.norm.df[, 1:2], cl = train.norm.df[, 3], k=i)
 +  accuracy.df[i,2] <- confusionMatrix(knn.pred, valid.norm.df[, 3])$overall[1]
 + }
 > accuracy.df
     k accuracy
 1   1      0.7
 2   2      0.6
 3   3      0.7
 4   4      0.6
 5   5      0.7
 6   6      0.6
 7   7      0.6
 8   8      0.6
 9   9      0.6
 10 10      0.4
 11 11      0.4
 12 12      0.4
 13 13      0.4
 14 14      0.4


.. image:: _static/knn.png
     :align: center



.. Tip::

   为什么要对k取不同值时的预测准确率做对比？如下案例说明当k=3时，分类为Owner；但k=5时，分类为Nonowner


.. code:: r

 # 实际预测在k取不同值时，会导致分类不同，案例如下
 > plot(Lot_Size ~ Income, data=mower.df, pch=ifelse(mower.df$Ownership=="Owner", 1, 3))
 > text(mower.df$Income, mower.df$Lot_Size, rownames(mower.df), pos=4)
 > text(60, 20, "XX")
 > legend("topright", c("Owner", "Nonowner", "NewerPredict"), pch=c(1,3,4))
 
 > knn.pred.k3 <- knn(train = mower.norm.df[, 1:2], test = new.norm.df, cl=mower.norm.df[,3], k=3)
 > knn.pred.k3
 [1] Owner
 attr(,"nn.index")
      [,1] [,2] [,3]
 [1,]    9    4   14
 attr(,"nn.dist")
           [,1]      [,2]      [,3]
 [1,] 0.3740582 0.3915507 0.4888494
 Levels: Owner
 > knn.pred.k4 <- knn(train = mower.norm.df[, 1:2], test = new.norm.df, cl=mower.norm.df[,3], k=5)
 > knn.pred.k4
 [1] Nonowner
 attr(,"nn.index")
      [,1] [,2] [,3] [,4] [,5]
 [1,]    9    4   14   13   16
 attr(,"nn.dist")
           [,1]      [,2]      [,3]      [,4]      [,5]
 [1,] 0.3740582 0.3915507 0.4888494 0.6527033 0.7244985
 Levels: Nonowner


.. image:: _static/knnthinking.png
     :align: center

