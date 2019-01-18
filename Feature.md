# 特征工程

## 特征工程概述

"数据决定了机器学习的上限，而算法只是尽可能逼近这个上限" \
数据指的就是经过特征工程得到的数据。

特征工程指的是把原始数据转变为模型的训练数据的过程，它的目的就是获取更好的训练数据特征，使得机器学习模型逼近这个上限。

一般认为括特征构建、特征提取、特征选择三个部分
- - -

### 一、特征构建

特征构建是指从原始数据中人工的找出一些具有物理意义的特征。需要花时间去观察原始数据，思考问题的潜在形式和数据结构，对数据敏感性和机器学习实战经验能帮助特征构建。除此之外，属性分割和结合是特征构建时常使用的方法。结构性的表格数据，可以尝试组合二个、三个不同的属性构造新的特征，如果存在时间相关属性，可以划出不同的时间窗口，得到同一属性在不同时间下的特征值，也可以把一个属性分解或切分，例如将数据中的日期字段按照季度和周期后者一天的上午、下午和晚上去构建特征。总之特征构建是个非常麻烦的问题，书里面也很少提到具体的方法，需要对问题有比较深入的理解。

### 二、特征提取 (_降维_)
PCA、ICA、SOM、MDS、ISOMAP、LLE……

**1、PCA主成分分析**
* 协方差原理

    样本X和样本Y的协方差(Covariance)：\
    ![[协方差公式]](https://images2017.cnblogs.com/blog/984656/201708/984656-20170830164626405-1764657020.png)        
    协方差为正时说明X和Y是正相关关系，协方差为负时X和Y是负相关关系，协方差为0时X和Y相互独立。
    
    
    
* SVD分解原理
* PCA原理及实现

**2、LDA线性判别分析**

**3、ICA独立分析**

**4、因子分析**

### 三、特征选择