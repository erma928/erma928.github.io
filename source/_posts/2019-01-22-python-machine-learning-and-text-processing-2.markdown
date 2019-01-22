---
layout: post
title: "python machine learning and text processing (2)"
date: 2019-01-22 16:44
comments: true
categories: [programming, machine learning, NLP]
---

机器学习是个大课题，也是一种解决现实问题的思维模式。虽然单纯学习其算法不算太难，然而要有对解决实际问题的纯熟运用，却需要在理论与实践相结合方面下足功夫。python在这方面提供了足够便利的工具。

<!--more-->

### 简介

1. 监督学习
   1. 分类问题
   2. 回归问题
2. 非监督学习
   1. 聚类
   2. 降维

### 步骤

1. 收集数据
   1. [Disasters on Social Media](https://data.world/crowdflower/disasters-on-social-media) 数据集
2. 数据清洗
   1. 去除不相关字符
   2. 拆分独立单词
   3. 去除不相关词语
   4. 转换成小写
   5. 多种拼法御特定表达绑定
   6. 词型还原
3. 特征提取
   1. bag of words 词袋法
      1. `John is happy he is not hungary for apples. -> [0,2,1,1,1,1,1,1,1]`
      2. 完全忽略单词顺序
   2. one-hot 
      1. `John is happy he is not hungary for apples. -> [0,1,1,1,1,1,1,1,1]`
   3. TF-IDF
      1. ![TF](http://www.ruanyifeng.com/blogimg/asset/201303/bg2013031504.png)
      2. ![IDF](http://www.ruanyifeng.com/blogimg/asset/201303/bg2013031506.png)
   4. word2vec
      1. 使用浅层双层神经网络，映射每一个词到一个向量，使之能表达词语之间的相互关系，以及上下文的关系。如：`V(queen) - V(woman) + V(man) = V(king)`
      2. 训练算法
         1. skip-grams: 由当前单词预测上下文
         2. CBOW: 由上下文预测当前单词
         3. ![nn](https://static.leiphone.com/uploads/new/article/740_740/201706/594b31d0920ef.png?imageMogr2/format/jpg/quality/90)
         4. ![weights](https://static.leiphone.com/uploads/new/article/740_740/201706/594b3267c64f4.png?imageMogr2/format/jpg/quality/90) 中间300weights就是car单词的词向量
         5. 句子嵌入的向量表示，可以采用将句子中词汇的word2vec平均化
4. 分类
   1. 数据集划分为训练集和测试集
   2. 分类算法：逻辑回归，SVM，贝叶斯公式，等等
5. 检验效果
   1. 混淆矩阵
   2. precision, recall, f1_score..

### Scikit-learn库

1. 算法包

   | 导入算法     | 代码                                               |
   | ------------ | -------------------------------------------------- |
   | 广义线性模型 | from sklearn.linear_model import LinearRegression  |
   | 逻辑回归     | rom sklearn.linear_model import LogisticRegression |
   | 支持向量机   | from sklearn.svm import SVC                        |
   | k近邻        | from sklearn.neighbors import KNeighborsClassifier |
   | 决策树       | from sklearn.tree import DecisionTreeClassifier    |
   | 朴素贝叶斯   | from sklearn.naive_bayes import MultinomialNB      |

2. 转换器（transformer）：用于数据预处理和数据转换

   1. transformer.fit
   2. transformer.transform
   3. transformer.fit_transform

3. 估计器（estimator）：用于分类和聚类

   1. estimator.fit
   2. estimator.predict

4. 特征提取（sklearn.feature_extraction）

   1. sklearn.feature_extraction.text.CountVectorizer：将文本转换为每个词出现的个数的向量（词典法）
   2. sklearn.feature_extraction.text.TfidfVectorizer：将文本转换为tfidf值的向量

5. 数据集分割（sklearn.cross_validation）

   1. train_test_split(X, y, test_size, random_state)

6. 效果评估（sklearn.metrics）

   1. confusion_matrix
   2. accuracy_score
   3. classification_report
   4. precision_recall_fscore_support

### sklearn文本特征提取

```python
from sklearn.feature_extraction.text import CountVectorizer

corpus = ["Hey lets get lunch :)",
           "Hey!!! I need a favor"]
vectorize = CountVectorizer()
vectorize.fit(corpus)
#构建 文档词频矩阵
dtm = vectorize.transform(corpus)
```

```python
from sklearn.feature_extraction.text import TfidfVectorizer

def createDTM(corpus):
    """构建文档词语矩阵"""
    vectorize = TfidfVectorizer()
    #注意fit_transform相当于fit之后又transform。
    dtm = vectorize.fit_transform(corpus)
    return dtm

corpus = ["Hey lets get lunch :)",
           "Hey!!! I need a favor"]
createDTM(corpus)
```

### sklearn文本分类

```python
import re
import pandas as pd
from sklearn.naive_bayes import MultinomialNB
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split

def normalize_numbers(s):
    """
    将数字特征统一替换为NUM
    """
    return re.sub(r'\b\d+\b', 'NUM', s)

dataset = pd.read_csv('data/20news-18828.csv', header=None, delimiter=',', names=['label', 'text'])

#依次导入X， y， test_size
X_train, X_test, y_train, y_test = train_test_split(dataset.text, dataset.label, test_size=0.3)

#CountVectorizer和TfidfVectorizer都有preprocessor参数，该参数传入预处理函数。
#也含有停用词处理参数stop_words
tv = TfidfVectorizer(preprocessor=normalize_numbers, stop_words='english')
X_train_tf = tv.fit_transform(X_train)
X_test_tf = tv.transform(X_test)

# 贝叶斯分类器
estimator2 = MultinomialNB()
estimator2.fit(X_train_tf, y_train)
predicted = estimator2.predict(X_test_tf)

print(classification_report(y_test, predicted, labels=dataset.label.unique()))

# 逻辑回归分类器
LR = LogisticRegression()
LR.fit(X_train_tf, y_train)
predicted = LR.predict(X_test_tf)
print(classification_report(y_test, predicted, labels=dataset.label.unique()))
```

### sklearn话题分析（聚类）

```python
import csv
import jieba
import re
from sklearn.cluster import KMeans
from sklearn.feature_extraction.text import TfidfVectorizer
import numpy as np
import pandas as pd

def read_data(file):
    """
    读取csv数据，返回含有字符串的列表数据。
    :param file:  csv文件
    :return: 列表数据（列表中每个组成部分是字符串文本）
    """

    df = pd.read_excel(file)
    #从dataframe中读取  news_content  列的所有新闻信息
    documents = df['news_content']
    return documents

def cluster_analysis(best_k, texts):
    """
    按照最佳聚类数k，将texts聚成k类。
    :param best_k: 聚类准确率最好的 k值
    :param texts: 列表数据（且列表中每个组成部分是字符串文本）
    :return:None

    """

    documents = [' '.join(jieba.lcut(text)) for text in texts]

    tfidf = TfidfVectorizer(max_df=0.5)
    # 学习tfidf矩阵数据中的分布规律
    tfidf.fit(documents)
    matrix = tfidf.transform(documents)
    print('文档向量化矩阵：')
    print(matrix.toarray())
    # c初始化Kmeans估计器
    estimator = KMeans(n_clusters=best_k)
    estimator.fit(matrix)

    #机器给所有的文档打上的标签
    #此处self.label_pred 为一维列表，列表长度等于 文档的数目
    label_pred = estimator.labels_

    #保存聚类算法的分类结果 到output文件夹中。
    with open('data/cluster_output.csv', 'a+', encoding='gbk', newline='') as csvf:
        writer = csv.writer(csvf)
        writer.writerow(('news_content', 'cluster'))
        #将新闻与 无kmeans聚类结果（cluster）写入csv文件
        for idx, text in enumerate(texts):
            try:
                writer.writerow((text, label_pred[idx]))
            except:
                continue
                
texts = read_data(file='data/百度新闻.xlsx')
cluster_analysis(best_k=19, texts=texts)                
```

### SVD计算消费者偏好

1. 推荐算法比较常见的实现方式是协同过滤（Collaberative Filtering）。数据涉及到用户，产品，和产品评价三种信息。
2. 有了用户评价矩阵，可以采用奇异值分解（SVD），获得降维后的用户向量，从而更准确的计算得到推荐信息
3. pip install scikit-surprise

```python
from surprise import Reader, Dataset
from surprise import SVD, evaluate

#定义数据格式
reader = Reader(line_format='user item rating timestamp', sep='\t')

#使用reader格式从u.data文件中读取数据
data = Dataset.load_from_file('data/u.data', reader=reader)
data.split(n_folds=5)

#相当于scikit的机器学习算法的初始化
svd = SVD()
#相当于scikit中的score，模型评估
evaluate(svd, data, measures=['RMSE', 'MAE'])

data = data.build_full_trainset() 
svd.fit(data)
svd.predict(186, 302, 4)
```

