---
layout: post
title: "python machine learning and text processing (1)"
date: 2019-01-22 15:32
comments: true
categories: [programming, machine learning, NLP]
---

### 前言

近期在学习python和机器学习。其实本人大学时学过不少机器学习相关专业课，包括神经网络，计算视觉与图像处理，模式识别，自然语言处理（NLP）等。也做过相关研究，读了些paper。可以说有一定基础。只是工作之后没有特意往这个方向发展，不自觉走入了被各种业务需求埋没的境地。如今AI大热，自己也对这块兴趣盎然，因而特别想在理论和实践上弄通。

<!--more-->

若干年以前，因Ruby on Rails快速开发的特性，自己曾把ruby当作银弹，很痴迷于这门语言。也看了很多python与ruby谁更强大这方面的争论。当时觉得ruby的表达能力很强大且优美，近乎魔术，而python，光使用缩进语法这一点就觉得有点怪，因而并没有深入去学。虽然其解决问题有且只有一条最佳思路、还有make things explicit的理念听起来有些道理。然而世事难料，随着Rails渐趋冷静，ruby似乎也渐趋没落。而python，随着人工智能的兴起却在走向潮头。当然，自己现在来学python，已经没有了当初的门户之见，反倒很容易的见识到python简单易用，以及确实强大的方面。在大数据和人工智能的浪潮中，python能站在前沿，是有它的独到之处的。

本blog主要对python文本处理基础，以及机器学习相关的内容进行提纲挈领。其中文本处理包含了分词、自然语言统计、可视化、数据分析等python库。而机器学习，则包含其理论分野、实现步骤、基本机器学习算法和python库等。

以下为文本处理基础库相关内容。

### 分词

1. jieba 中文分词库

   1. cut, lcut
   2. load_userdict(file)
   3. jieba.posseg.cut

2. 正则词频统计

   1. `words = re.findall(r'[\u4e00-\u9fa5]+', test)` 匹配中文正则

```python
for word in wordset:
    if len(word)>1 :
        freq = wordlist.count(word)
        result.append((word, freq))         
#[(w1,freq1),(w2,freq2)....]
#对词频统计结果进行排序
new_result = sorted(result, key=lambda k:k[1], reverse=True)
```

### 自然语言统计

1. nltk 自然语言处理库

   1. nltk.text.Text类

      1. `text = Text(wordlist)` 
      2. `text.concordance(word='汪淼', width=20, lines=10)` 上下文
      3. `text.similar(word='叶文洁', num=10)`上下文最相关词
      4. `text.count(word='叶文洁')` 统计词频
      5. `text.dispersion_plot(words)` 离散图，横坐标表示词语的索引位置

   2. Ngrams的实现

      1. `nltk.util.bigrams(seq), trigrams(seq), ngrams(seq, n), skipgrams(seq, n, k)`

   3. nltk.probability

      1. FreqDist

         1. `fdist = FreqDist(wordlist)`
         2. `fdist.items()` 词频对
         3. `fdist.N()` 词语总数
         4. `fdist.max()` 词语总数
         5. `fdist.freq('三体')` 词频占比
         6. `fdist['三体']` 词频
         7. `fdist.plot(20,cumulative=True)` 前20个词画图

      2. ConditionFreqDist 带条件的频率分布类

         1. 

```python
cfd = ConditionalFreqDist(
         [(conditon1, event1),(conditon2, event1),(conditon1, event2)...(conditon_n, event_n)]
       )
```

         2. 
```python
cfd = ConditionalFreqDist(
         (file, word)
         for file in files     
         for word in text_split(file))

    #计算前10大词在各类文本中的频数
    cfd.tabulate(conditions=files, samples=words)
```

### 可视化

1. pyecharts - 动态交互可视化库

   1. Bar, Bar3D, Pie, Map, ...

```python
"""
kwargs为通用配置项
"""
add(name, x_axis, y_axis, data,
   grid3d_opacity=1,
   grid3d_shading='color', **kwargs) 

from pyecharts import Bar3D

bar3d = Bar3D("3D 柱状图示例", width=1200, height=600
             ...
             ...
bar3d.add(
   "",
   x_axis,
   y_axis,
   [[d[1], d[0], d[2]] for d in data],
   is_visualmap=True,
   visual_range=[0, 20],
   visual_range_color=range_color,
   grid3d_width=200,
   grid3d_depth=80,
)
bar3d.render('html/3D_Bar.html')
```

   1. WordCloud

```python
"""
name	str图例名称
attr	list属性名称
value	list属性所对应的值
"""
add(name, attr, value,
   shape="circle",
   word_gap=20,
   word_size_range=None,
   rotate_step=45)

from pyecharts import WordCloud
words = ['Python', '文本分析', '编程']
freqs = [100,200,50]
wordcloud = WordCloud(width=1300, height=620)
wordcloud.add("", words, freqs, word_size_range=[20, 100])
wordcloud.render('html/wordcloud.html')
```

   

### 数据分析库

1. pandas

   1. Series

      1. Series(list), Series(dict)
      2. notnull, s2>2, s2[s2>2]
      3. 数学运算
      4. value_counts(normalize=False)
      5. head, tail, sample
      6. plot(kind='bar')

   2. DataFrame

      1. 列向量和行向量组成的数据结构

      2. 
```python
 import numpy as np
 import pandas as pd
 df = pd.DataFrame(np.arange(40).reshape(8,5),
                  index=['one','two','three','four','five','six','seven','eight'],
                  columns=['a','b','c','d','e'])
 ```

      3. `df.drop('one', axis='rows', inplace=False)`

      4. `df['a']`, `df.a` - 索引列名称

      5. `df.index` - 行索引

      6. `df[:3]` - 行索引查询

      7. `df.loc[['two','three'], ['a','d']]` - 子DataFrame查询

      8. `df.loc[:, ['a','d']]`, `df[['a', 'd']]` - 所有行, a,d 列

      9. `df.loc[['two','three'], :]` - 所有列

      10. `df.describe()`

      11. `df['colname'] = 10009` - 新建或更新列

      12. 列方向进行**聚合运算**

```python
df.agg({'colname1': ['mean', diyfunc],
       'colname2': ['mean', 'sum']})
```

          1. `df.agg('mean')`- 针对所有列的聚合操作

      13. 针对值进行分组 - **groupby**

          1. `df.set_index('date').groupby('name')['ext price'].resample("M").sum()` - 针对name列的值进行分组
          2. `df.groupby(['name', pd.Grouper(key='date', freq='M')])['ext price'].sum()`
          3. `df.groupby('violation_raw').search_conducted.agg(['mean', 'count'])`

      14. `newdf=df+100`

      15. `newdf>120`

      16. `df[newdf>120]`

      17. **时间操作**

          1. pd.date_range() 生成一个时间段索引

             1. `pd.date_range('2017/10/11 10:00:01', '20181030', freq='3M') `

          2. pd.to_datetime(data) 将data序列数据转换为datetime类型Series

             1. 
```python
datatime = pd.to_datetime(data)
datatime.dt.year
datatime.dt.month.value_counts()
```

          3. 指定日期为索引

             1. 
```python
tm_rng = pd.date_range('2017-12-31 12:00:00',periods=20, freq='5H')
tm_series = pd.Series(np.random.randn(len(tm_rng)), index=tm_rng)
tm_series['2017']
```

          4. resample

             1. `df.set_index('date').resample('2M')['ext price'].sum()`

      18. 读取csv或excell文件为dataFrame

          1. `df = pd.read_excel('data/sample-salesv3.xlsx', encoding='gbk')`

### 