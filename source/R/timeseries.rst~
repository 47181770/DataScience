R 代码处理时序数据
--------------------

1. 股票数据处理
~~~~~~~~~~~~~~~~~~~~~~~~


.. tip::
   以如意集团002193股票数据集为例，本分析方法仅为了便于说明如何用R处理时序数据，不构成投资建议！

.. code:: r


 # 读取002193股票资金流入流出数据
 ruyi <- read.csv("D:\\stocks\\002193_zj.csv")
 # 读取数据集的行、列数
 dim(ruyi)
 [1] 2214   21
 # 显示数据框的列名
 > colnames(ruyi)
 [1] "X"               "ts_code"         "trade_date"      "buy_sm_vol"      "buy_sm_amount"   "sell_sm_vol"     "sell_sm_amount"  "buy_md_vol"     
 [9] "buy_md_amount"   "sell_md_vol"     "sell_md_amount"  "buy_lg_vol"      "buy_lg_amount"   "sell_lg_vol"     "sell_lg_amount"  "buy_elg_vol"    
 [17] "buy_elg_amount"  "sell_elg_vol"    "sell_elg_amount" "net_mf_vol"      "net_mf_amount"  
 > t(t(colnames(ruyi)))
       [,1]             
  [1,] "X"              
  [2,] "ts_code"        
  [3,] "trade_date"     
  [4,] "buy_sm_vol"     
  [5,] "buy_sm_amount"  
  [6,] "sell_sm_vol"    
  [7,] "sell_sm_amount" 
  [8,] "buy_md_vol"     
  [9,] "buy_md_amount"  
 [10,] "sell_md_vol"    
 [11,] "sell_md_amount" 
 [12,] "buy_lg_vol"     
 [13,] "buy_lg_amount"  
 [14,] "sell_lg_vol"    
 [15,] "sell_lg_amount" 
 [16,] "buy_elg_vol"    
 [17,] "buy_elg_amount" 
 [18,] "sell_elg_vol"   
 [19,] "sell_elg_amount"
 [20,] "net_mf_vol"     
 [21,] "net_mf_amount"  
 > 
 # 显示行名
 rownames(housing.df)
 
 # 查看前6行数据
 head(ruyi)
 # 查看前10条记录
 head(ruyi, 10)
 # 查看后10条记录
 tail(ruyi, 10)
 # 在新的窗格窗口查看数据
 View(ruyi)

 # 处理缺失值，把NA数据替换成0
 ruyi[is.na(ruyi)] <- 0
 # 把交易日期列转换成日期
 ruyi['trade_date'] <- as.Date(paste(ruyi$trade_date), '%Y%m%d')
 > class(ruyi$trade_date)
 [1] "Date"
 # 对数据进行排序
 ruyi2 <- ruyi[order(ruyi$trade_date),]
 > head(ruyi2)
         X   ts_code trade_date buy_sm_vol buy_sm_amount sell_sm_vol sell_sm_amount buy_md_vol buy_md_amount sell_md_vol
 2214 2213 002193.SZ 2010-01-05      11125       1328.24       11219        1340.84      12660       1513.23       12282
 2213 2212 002193.SZ 2010-01-06      18786       2316.96       22333        2757.52      21872       2695.81       20731
 2212 2211 002193.SZ 2010-01-07      31785       4051.61       45876        5847.48      38483       4899.21       42006
 2211 2210 002193.SZ 2010-01-08      28123       3682.86       30216        3961.44      34764       4555.62       33629
 2210 2209 002193.SZ 2010-01-11      24439       3307.78       24211        3283.48      33010       4482.70       34771
 > tail(ruyi2)
   X   ts_code trade_date buy_sm_vol buy_sm_amount sell_sm_vol sell_sm_amount buy_md_vol buy_md_amount sell_md_vol sell_md_amount
 6 5 002193.SZ 2020-02-05      15311       1009.27       15200        1007.62      20766       1371.21       22050        1460.13
 5 4 002193.SZ 2020-02-06      15728       1063.92       16657        1130.76      18941       1283.98       15824        1071.89
 4 3 002193.SZ 2020-02-07      23592       1657.57       23692        1671.36      22413       1576.68       22615        1585.79
 3 2 002193.SZ 2020-02-10      15974       1130.13       13684         968.75      21284       1506.34       18207        1288.05
 2 1 002193.SZ 2020-02-11      16383       1135.64        9369         649.33       9790        679.32       15698        1089.58
 1 0 002193.SZ 2020-02-12      11004        769.27       14431        1010.05      15465       1081.28       13204         923.56
 
 # 计算累计三、五、十、二十、三十天的移动净流入和 
 library(TTR)
 ruyi2['summa3'] <- SMA(ruyi2$net_mf_amount, 3) * 3
 ruyi2['summa5'] <- SMA(ruyi2$net_mf_amount, 5) * 5
 ruyi2['summa10'] <- SMA(ruyi2$net_mf_amount, 10) * 10
 ruyi2['summa40'] <- SMA(ruyi2$net_mf_amount, 40) * 40
 # 查看是否计算正确
 > tail(ruyi2[,c(1,3,19:28)]) 
 > tail(ruyi2[,c(1,3,19:28)])
  X trade_date sell_elg_amount net_mf_vol net_mf_amount total_buy  buy_sm  buy_md   summa3   summa5  summa10  summa40
 6 5 2020-02-05            0.00       3264        212.98     -1.65    1.65  -88.92   817.44  -355.32 -2041.66 -6591.12
 5 4 2020-02-06            0.00       -321        -19.04     66.84  -66.84  212.09   296.67   -16.86 -1599.25 -6585.54
 4 3 2020-02-07          167.99      -1842       -121.44     13.79  -13.79   -9.11    72.50   676.96 -1230.83 -6502.50
 3 2 2020-02-10            0.00      -8350       -588.38   -161.39  161.38  218.29  -728.86  -413.15 -1480.64 -7216.87
 2 1 2020-02-11            0.00     -10659       -737.75   -486.32  486.31 -410.26 -1447.57 -1253.63 -2390.93 -7728.08
 1 0 2020-02-12            0.00       2607        185.86    240.79 -240.78  157.72 -1140.27 -1280.75 -1636.07 -7552.36
 # 筛选2019-11-1 至今的数据
 ruyi3 <- subset(ruyi2, ruyi2$trade_date > '2019-11-1')
 > p3 <- ggplot(ruyi3, aes(x=ruyi3$trade_date)) +
 + geom_line(aes(y=ruyi3$total_buy), size=1, col='red') +
 + geom_line(aes(y=ruyi3$net_mf_amount), size=1, col='blue')
 > p3
 # 显示如下图
 
 
 
 
.. image:: _static/zijin.png
   :align: center
 
 
-----------------------------------------


2. 数据集说明
~~~~~~~~~~~~~~~~~~~~


- 股票数据集

 ===================== ===================================================================
  ts_code                股票代码
  trade_date             交易日期
  net_mf_amount          净流入额（万元）
 ===================== ===================================================================


参考：
   1. R 金融数据分析

