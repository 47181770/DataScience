样例-TFIDF
~~~~~~~~~~~~~~~~~~~~~


- TF-IDF（单文本词频-逆文档频率）
  
  TF-IDF 是对搜索关键词的重要性的度量，具备很强的理论依据，就是不精通搜索的人，直接采用TF-IDF，效果也不会差。

  1. TF，term Frequency：用关键词**次数**除以网页总字数，也叫关键字频率：
       如某网页有1000个字，原子能 的 应用分别出现了2次、35次、5次，那么他们的词频分别是0.002、0.035、0.005 ，这三个数相加，和0.042就是相应的一个网页和查询“原子能的应用”的单文本词频
  
  2. IDF，Inverse Document Frequency逆文本频率指数：
       假定一个关键词w在Dw个网页中出现，那么Dw越大，w的权重越小，反之亦然。
       假设中文网页数是十亿，停止词“的”在每个网页中都有，那么Dw=10亿。 他的公式就是log(D/Dw) = log(10亿/10亿）=log(1) = 0；

  如果原子能在200万个网页中出现，那么公式是log(10亿/200万)=log(500)=8.96，那么权重就是8.96。
  所以，一段文本的相关性就由如下公式计算：TF1 * IDF1 + TF2 * IDF2 + TF3 * IDF3 ... + TFn * IDFn
 
 
.. code:: r


 # 利用文本数据集进行TF-IDF计算
 ## 从文本2020cio.txt读取文本
 x <- readLines(("D:\\2020cio.txt"))
 > corp <- Corpus(VectorSource(x))
 > head(corp)
 <<SimpleCorpus>>
 Metadata:  corpus specific: 1, document level (indexed): 0
 Content:  documents: 6
 > corp
 <<SimpleCorpus>>
 Metadata:  corpus specific: 1, document level (indexed): 0
 Content:  documents: 99
 # 语料库预处理
 ## 除去多余空格
 > corp <- tm_map(corp, stripWhitespace)
 Warning message:
 In tm_map.SimpleCorpus(corp, stripWhitespace) :
   transformation drops documents
 # 删除标点符号
 > corp <- tm_map(corp, removePunctuation)
 Warning message:
 In tm_map.SimpleCorpus(corp, removePunctuation) :
   transformation drops documents
 # 以单词名为行名构建矩阵，注意还有另外一种用法是以文件名为行构建矩阵就要用DocumentTermMatrix()
 > tdm <- TermDocumentMatrix(corp)
 > inspect(tdm)
 <<TermDocumentMatrix (terms: 790, documents: 99)>>
 Non-/sparse entries: 1905/76305
 Sparsity           : 98%
 Maximal term length: 17
 Weighting          : term frequency (tf)
 Sample             :
        Docs
 Terms   1 19 25 3 44 45 53 71 74 89
   2019  1  0  0 2  1  1  1  0  0  0
   2020  0  0  0 1  1  1  1  1  1  1
   and   1  1  1 3  0  2  0  0  0  2
   are   1  0  0 0  0  0  0  0  0  2
   cio   2  1  0 1  1  0  1  1  0  0
   cios  1  0  1 3  0  1  1  0  2  2
   focus 0  0  0 1  0  0  1  0  1  0
   for   0  0  0 0  1  2  0  0  1  0
   the   5  4  5 4  2  2  0  2  1  0
   will  1  0  1 1  0  1  0  0  0  0
 > library(SnowballC)
 # 消除停用词
 > corp <- tm_map(corp, removeWords, stopwords("english"))
 Warning message:
 In tm_map.SimpleCorpus(corp, removeWords, stopwords("english")) :
   transformation drops documents
 # 提取单词词干
 > corp <- tm_map(corp, stemDocument)
 Warning message:
 In tm_map.SimpleCorpus(corp, stemDocument) : transformation drops documents
 > tdm <- TermDocumentMatrix(corp)
 > inspect(tdm)
 <<TermDocumentMatrix (terms: 641, documents: 99)>>
 Non-/sparse entries: 1552/61907
 Sparsity           : 98%
 Maximal term length: 15
 Weighting          : term frequency (tf)
 Sample             :
            Docs
 Terms       1 15 20 3 44 53 54 71 74 89
   2019      1  0  0 2  1  1  0  0  0  0
   2020      0  1  1 1  1  1  1  1  1  1
   busi      0  0  0 0  0  0  0  0  0  1
   cio       2  0  1 1  1  1  1  1  0  0
   cios      1  2  0 3  0  1  0  0  2  2
   focus     0  2  1 1  1  1  1  1  1  1
   summit    0  0  0 0  0  0  1  0  0  1
   technolog 2  0  0 0  1  0  0  0  1  0
   the       0  0  2 0  0  0  1  0  0  0
   will      1  1  1 1  0  0  2  0  0  0
 # 计算TD-IDF
 > tdidf <- weightTfIdf(tdm)
 > inspect(tdidf)
 <<TermDocumentMatrix (terms: 641, documents: 99)>>
 Non-/sparse entries: 1552/61907
 Sparsity           : 98%
 Maximal term length: 15
 Weighting          : term frequency - inverse document frequency (normalized) (tf-idf)
 Sample             :
            Docs
 Terms               19         26         29        55         56         77         80        82         97        99
   2019      0.00000000 0.00000000 0.00000000 0.1138751 0.00000000 0.00000000 0.00000000 0.0000000 0.00000000 0.1214668
   2020      0.00000000 0.00000000 0.02941609 0.0000000 0.00000000 0.05883219 0.03125460 0.0000000 0.00000000 0.0000000
   busi      0.00000000 0.00000000 0.00000000 0.1763751 0.00000000 0.00000000 0.00000000 0.0000000 0.00000000 0.0000000
   cio       0.03702098 0.05721424 0.03702098 0.0000000 0.03702098 0.00000000 0.03933479 0.0000000 0.05721424 0.0000000
   cios      0.00000000 0.00000000 0.00000000 0.0000000 0.00000000 0.00000000 0.00000000 0.0000000 0.00000000 0.0000000
   jan       0.00000000 0.00000000 0.00000000 0.0000000 0.00000000 0.00000000 0.00000000 0.0000000 0.00000000 0.0000000
   summit    0.00000000 0.00000000 0.00000000 0.0000000 0.00000000 0.00000000 0.00000000 0.3320002 0.00000000 0.0000000
   technolog 0.00000000 0.00000000 0.00000000 0.0000000 0.00000000 0.00000000 0.00000000 0.0000000 0.00000000 0.0000000
   the       0.00000000 0.00000000 0.10717657 0.0000000 0.00000000 0.00000000 0.00000000 0.0000000 0.00000000 0.0000000
   will      0.00000000 0.00000000 0.00000000 0.0000000 0.00000000 0.00000000 0.00000000 0.0000000 0.00000000 0.0000000
 # 潜在语义分析LSA(latent semantic analysis)
 > library(lsa)
 > lsa.tfidf <- lsa(tfidf, dim = 20)
 # 转换成数据框，方能进行如glm模型预测及模型评估等工作
 > words.df <- as.data.frame(as.matrix(lsa.tfidf$dk))
 

 > library(wordcloud)
 > wordcloud(corp, colors = 'blue')
 > wsf <- TermDocumentMatrix(corp)
 > m <- as.matrix(wsf)
 > head(m)
         Docs
 Terms    1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52
   2019   1 1 2 1 1 0 1 0 0  1  0  0  0  0  0  0  0  0  0  0  1  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  1  1  1  1  0  0  1  0  1  1  0
   can    1 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  1  0  0  0  0  0  1  0  0  0  0  0  1  0  0  0  0  0  0  0  0  1  0  0  0  0  0  0  0  0  0  0  0  0
   chang  1 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  1  1  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  1  0  0  0  0
   cio    2 0 1 0 1 1 1 1 1  1  2  0  1  0  0  1  1  1  1  1  0  2  1  1  0  1  1  2  1  1  1  1  1  1  0  1  1  1  1  1  0  0  0  1  0  0  1  1  1  0  0  1
   cios   1 2 3 0 0 1 1 1 0  0  0  1  0  1  2  1  1  0  0  0  1  0  0  0  1  0  0  1  0  1  0  1  0  0  0  0  0  0  0  0  1  1  1  0  1  1  0  0  0  0  1  0
   consid 1 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
         Docs
 Terms    53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95 96 97 98 99
   2019    1  0  1  0  0  0  0  0  1  0  0  0  1  0  0  0  0  0  0  0  0  0  1  1  0  1  1  0  0  0  1  0  0  0  0  0  0  0  0  0  0  1  0  1  0  1  1
   can     0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
   chang   0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  1  0  0  0  0  0  0  0  0  0  0  0  0  0  1  0  0  1  0  0  0  0  0  0  0  0  1  0  0
   cio     1  1  0  1  0  1  1  0  0  0  0  0  0  2  1  1  1  1  1  1  0  0  1  0  0  2  2  1  0  0  1  2  1  0  1  1  0  1  1  2  2  1  0  0  1  0  0
   cios    1  0  0  0  1  0  2  1  0  2  1  1  1  0  1  0  0  0  0  1  1  2  0  1  0  0  0  0  1  0  1  0  0  0  0  1  2  1  1  0  1  0  1  0  0  1  0
   consid  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
 # 查看每个单词的出现次数，计算每行词频的和，然后排序
 > v <- sort(rowSums(m), decreasing=TRUE)
 > d <- data.frame(word = names(v), freq =v)
 > head(d)
        word freq
 2020   2020   94
 cio     cio   74
 focus focus   70
 cios   cios   52
 will   will   32
 the     the   32
  tdm <- TermDocumentMatrix(corp)
 > tdidf <- weightTfIdf(tdm)
 > tdidf
 <<TermDocumentMatrix (terms: 641, documents: 99)>>
 Non-/sparse entries: 1552/61907
 Sparsity           : 98%
 Maximal term length: 15
 Weighting          : term frequency - inverse document frequency (normalized) (tf-idf)
 > inspect(tdidf)
 <<TermDocumentMatrix (terms: 641, documents: 99)>>
 Non-/sparse entries: 1552/61907
 Sparsity           : 98%
 # 获取给定维数dim=20的潜在语义空间
 # 隐性语义分析引申阅读参考
 > lsa.tdidf <- lsa(tdidf, dim=20)
 Warning message:
 In lsa(tdidf, dim = 20) : [lsa] - there are singular values which are zero.
 # D：文档向量矩阵；T：词向量矩阵；s：对角矩阵 k=dims
 
 > summary(lsa.tdidf)
   Length Class  Mode   
 tk 12820  -none- numeric
 dk  1980  -none- numeric
 sk    20  -none- numeric
 > words.df <- as.data.frame(as.matrix(lsa.tdidf$dk))
 > head(words.df)
            V1          V2           V3           V4            V5            V6          V7          V8          V9         V10          V11           V12
 1 -0.11082959  0.03024325 -0.020250478  0.020210038 -8.713279e-03  0.0016374672 -0.01361382 0.031709640 -0.02864437 0.001580599 -0.020215305  0.0596256444
 2 -0.09911556 -0.02035441  0.009085474 -0.008437858  2.367281e-02  0.0208146481 -0.02424743 0.009212675 -0.05915897 0.059309929 -0.038972726  0.0354622876
 3 -0.10648037 -0.02218652 -0.035695585  0.027063384 -1.428463e-02  0.0010159345  0.03469075 0.003209064  0.02116949 0.015097879  0.016325384  0.0001395995
 4 -0.09267651 -0.02076691  0.015200384  0.037142322 -1.451094e-02 -0.0002165991 -0.02210088 0.041922725 -0.08501144 0.076511304 -0.067379381 -0.0308540249
 
 # 探索性画词云图
 > wordcloud(d$word, d$freq)
 # 利用RColorBrewer配色方案做词云图
 > if(require(RColorBrewer)){
 +     
 +     pal <- brewer.pal(9,"BuGn")
 +     pal <- pal[-(1:4)]
 +     wordcloud(d$word,d$freq,c(8,.3),2,,FALSE,,.15,pal)
 +     
 +     
 +     pal <- brewer.pal(6,"Dark2")
 +     pal <- pal[-(1)]
 +     wordcloud(d$word,d$freq,c(8,.3),2,,TRUE,,.15,pal)
 +     
 +     #random colors
 +     wordcloud(d$word,d$freq,c(8,.3),2,,TRUE,TRUE,.15,pal)
 + }


--------------------------------------------

生成词云图如下：

.. image:: _static/2020cio.png
   :align: center 



参考：
    1. 隐性语义分析（LSA）是一种自然语言处理NLP中用到的方法，其通过“矢量语义空间”来提取文档与词中的“概念”，进而分析文档与词之间的关系。LSA的基本假设是，如果两个词多次出现在同一文档中，则这两个词在语义上具有相似性。LSA使用大量的文本上构建一个矩阵，这个矩阵的一行代表一个词，一列代表一个文档，矩阵元素代表该词在该文档中出现的次数，然后在此矩阵上使用奇异值分解（SVD）来保留列信息的情况下减少矩阵行数，之后每两个词语的相似性则可以通过其行向量的cos值（或者归一化之后使用向量点乘）来进行标示，此值越接近于1则说明两个词语越相似，越接近于0则说明越不相似。
    2. 新闻归类、主题归类：余弦定理（准确性高，大数据量慢）
    3. 超大规模文本粗分类：奇异值分解得到粗分类结果，再利用个计算向量余弦的方法得到比较精确的结果
