


#### 本页目录

* [有监督学习](#有监督学习)
    * [线性回归和逻辑回归比较](#线性回归和逻辑回归比较)
    * [回归算法]()
        * [Linear regression - 线性回归 ](#Linear%20regression%20-%20线性回归)
            * [线性回归参数求解方法一：解析法 ](#解析法)
            * [线性回归参数求解方法二：梯度下降 ](#梯度下降)
    * [分类算法 - 预测的变量是离散的]()
        * [Logistic regression - 逻辑回归 ](#Logistic%20regression%20-%20逻辑回归)
            * [逻辑回归 - 简介](#简介)
            * [逻辑回归的 cost function](#逻辑回归的%20cost%20function)
            * [线性回归参数求解方法二：梯度下降 ](#梯度下降)
            * [逻辑回归参数求解 （最小化 cost function, 或 最大化 极大似然函数)](#逻辑回归参数求解%20（最小化%20cost%20function,%20或%20最大化%20极大似然函数）)
                * [方法一：梯度下降](#方法一：梯度下降)
                    * [梯度下井推导过程](#梯度下井推导过程)
                    * [梯度下降的可行性 解释](#梯度下降的可行性%20解释)
                * [方法二：牛顿法](#方法二：牛顿法)
                    * [牛顿法推导](#牛顿法推导)
                * [求解正则化](#求解正则化)
                    * [L1正则化](#L1正则化)
                    * [L2正则化](#L1正则化)
                * [并行化 求解 梯度下降或牛顿法](#并行化求解梯度下降或牛顿法)
                    * [计算步骤](#计算步骤)
        * [Softmax regression （SMR） - 可用于多分类](#Logistic%20regression%20-%20逻辑回归)
            * [SMR 一般步骤](#一般步骤)
            * [Softmax定义](#Softmax定义)
            * [Softmax模型定义](#Softmax模型定义)
            * [Softmax cost function - 定义为 交叉熵](#Softmax%20cost%20function%20-%20定义为%20交叉熵)
            * [Softmax Regression 多分类例子](#Softmax%20Regression%20多分类例子)
            * [Softmax Regression 与 logistic regression 的联系](#Softmax%20Regression%20与%20logistic%20regression%20的联系)
            * [Softmax Regression 过拟合问题 - 处理方法为减少特征数量或正则化](#Softmax%20Regression%20过拟合问题%20-%20处理方法为%20`减少特征数量`%20或%20`正则化`)










----

## 有监督学习
* 回归算法 - 预测的变量是连续的
    * linear regression - 线性回归

* 分类算法 - 预测的变量是离散的
    * logistic regression - 逻辑回归 - （统计方法）
    * Softmax regression - 可用于多分类
    * Decision Tree -  决策树
    * SVM - 支持向量 - （几何方法）




### 线性回归和逻辑回归比较

* 线性回归只能用于回归问题，逻辑回归虽然名字叫回归，但是更多用于分类问题
* 线性回归要求因变量是`连续性数值变量`，而逻辑回归要求因变量是`离散的变量`
* 线性回归要求自变量和因变量呈线性关系，而逻辑回归不要求自变量和因变量呈线性关系
* 线性回归可以直观的表达`自变量和因变量之间的关系`，逻辑回归则无法表达变量之间的关系


### Linear regression - 线性回归 

* 给定数据集D={(x1, y1), (x2, y2), ...}，我们试图从此数据集中学习得到一个线性模型，这个模型尽可能准确地反应x(i)和y(i)的对应关系。这里的线性模型，就是属性(x)的线性组合的函数，可表示为：

![linear regression](ML_img/3.png)

* 通俗的理解：x(i)就是一个个属性（例如西瓜书中的色泽，根蒂；Andrew ng示例中的房屋面积，卧室数量等），theta(或者w/b)，就是对应属性的参数（或者权重），我们根据已有数据集来求得属性的参数（相当于求得函数的参数），然后根据模型来对于新的输入或者旧的输入来进行预测（或者评估）。

* 从下图来直观理解一下线性回归优化的目标——图中线段距离（平方）的平均值，也就是最小化到分割面的距离和。

* 也就是说我们尽量使得f(xi)接近于yi，那么问题来了，我们如何衡量二者的差别？常用的方法是均方误差，也就是（均方误差的几何意义就是欧氏距离）最小二乘法。最小二乘法即为线性回归的损失函数 （loss function）。

![linear regression 2](ML_img/6.png)

* 这里，将上式中的 b 也向量化，吸纳到 w 中。

![linear regression 2](ML_img/linear_regression_2.png)

* 此时，线性回归的目标就是求得

![linear regression 4](ML_img/4.png)

* 这里，argmin是指求得最小值时的w,b的取值

* 这里可以采用两种方式完成求解，当维度不高时可以采用解析法。当处理高纬度数据时可以使用梯度下降求解。
    * #### 解析法
    ![linear regression 5](ML_img/5.png)
        * E是关于 (w,b)的凸函数，只有一个最小值。此时，所求即为E为最小是的 (w,b) 取值。
            ![tu function](ML_img/tu_func.png)
        * 对于凸函数E关于w,b导数都为零时，就得到了最优解。
        * 若是一维数据，可分别对w,b求导，令等式为0，即可计算。
        * 若是多维数据，则使用矩阵。
        * 此时，解析解为：
            ![linear regression 7](ML_img/7.png)
    
    ![linear regression 8](ML_img/8.png)

* 以下是多元线性回归 最小二乘法 推导过程
    ![linear formula](ML_img/linear_formula.png)

    * #### 梯度下降
        * 由上一步，根据最小二乘法得到 cost function (损失函数)，这里使用梯度下降方法求解损失函数最小值时的参数值。（最优化问题）
        * 梯度下降有不同的方法，分别为
            * batch gradient descent - 批梯度下降: 权重的更新通过计算全部训练集的数据
            * Stochastic Gradient Descent, （SGD） - 随机梯度下降: 当数据量特别大时，`在计算最快下降方向时，随机选择一个数据进行计算，而不是扫描所有的训练数据`，这样就`加快了迭代速度`。 随机梯度下降并不是沿着 cost function 下降最快的方向进行，是以震荡的方式趋向极小点。
            *  批量梯度下降
                ![Gradient Descent 3](ML_img/gradient_descent_3.png)
                ![Gradient Descent 4](ML_img/gradient_descent_4.png)
            * 随机梯度下降
                ![Gradient Descent 5](ML_img/gradient_descent_5.png)
    
     ![Gradient Descent 1](ML_img/gradient_descent.png)
     ![Gradient Descent 2](ML_img/gradient_descent_2.png)
        




### Logistic regression - 逻辑回归

* Logistic 回归的本质是：假设数据服从这个分布，然后使用极大似然估计做参数的估计。
* 逻辑回归的思路是，先拟合决策边界(不局限于线性，还可以是多项式)，再建立这个边界与分类的概率联系，从而得到了二分类情况下的概率

* 它输出一个 0 到 1 之间的离散二值结果。简单来说，它的结果不是 1 就是 0。
* Logistic 回归通过使用其固有的 logistic 函数估计概率，来衡量因变量（我们想要预测的标签）与一个或多个自变量（特征）之间的关系。


![Logistic regression 1](ML_img/logistic_regression_1.png)

* 逻辑回归`优点`
    * 直接对分类的概率建模，无需实现假设数据分布，从而避免了假设分布不准确带来的问题；
    * 不仅可预测出类别，还能得到该预测的概率，这对一些利用概率辅助决策的任务很有用；
    * 对数几率函数是任意阶可导的凸函数，有许多数值优化算法都可以求出最优解。
    * 实现简单，广泛的应用于工业问题上；
    * 分类时计算量非常小，速度很快，存储资源低；
    * 便利的观测样本概率分数；
    * 对逻辑回归而言，多重共线性并不是问题，它可以结合L2正则化来解决该问题；
    * 计算代价不高，易于理解和实现；

* `缺点`
    * 当特征空间很大时，逻辑回归的性能不是很好；
    * 容易`欠拟合`，一般准确度不太高
    * 不能很好地处理大量多类特征或变量；
    * 只能处理两分类问题（在此基础上衍生出来的softmax可以用于多分类），且`必须线性可分`；
    * 对于非线性特征，需要进行转换；

![Logistic regression 2](ML_img/logistic_regression_2.png)

* #### 简介
![Logistic regression 22](ML_img/logistic_regression_22.png)

![Logistic regression 3](ML_img/logistic_regression_3.png)

![Logistic regression 4](ML_img/logistic_regression_4.png)

#### 逻辑回归的 cost function

![Logistic regression 29](ML_img/logistic_regression_29.png)

![Logistic regression 30](ML_img/logistic_regression_30.png)

![Logistic regression 23](ML_img/logistic_regression_23.png)

![Logistic regression 24](ML_img/logistic_regression_24.png)

![Logistic regression 5](ML_img/logistic_regression_5.png)

![Logistic regression 31](ML_img/logistic_regression_31.png)


#### 逻辑回归参数求解 （最小化 cost function, 或 最大化 极大似然函数）

![Logistic regression 6](ML_img/logistic_regression_6.png)

* ##### 方法一：梯度下降

    * ![Logistic regression 7](ML_img/logistic_regression_7.png)
    
    * ###### 梯度下井推导过程
    ![Logistic regression 25](ML_img/logistic_regression_25.png)
    ![Logistic regression 26](ML_img/logistic_regression_26.png)
    ![Logistic regression 27](ML_img/logistic_regression_27.png)
    
    * ###### 梯度下降的可行性 解释
    ![Logistic regression 28](ML_img/logistic_regression_28.png)


* ##### 方法二：牛顿法

    ![Logistic regression 8](ML_img/logistic_regression_8.png)

    * ###### 牛顿法推导

    ![Logistic regression 32](ML_img/logistic_regression_32.png)



#### 求解正则化

* 正则化是一个通用的算法和思想，所以会产生过拟合现象的算法都可以使用正则化来避免过拟合。

![Logistic regression 9](ML_img/logistic_regression_9.png)

* ##### L1正则化
    
    ![Logistic regression 10](ML_img/logistic_regression_10.png)
    
    * 拉普拉斯分布

    ![Logistic regression 33](ML_img/logistic_regression_33.png)


* ##### L2正则化

    ![Logistic regression 11](ML_img/logistic_regression_11.png)

    * 正态分布
    
    ![Logistic regression 34](ML_img/logistic_regression_34.png)

![Logistic regression 12](ML_img/logistic_regression_12.png)

![Logistic regression 13](ML_img/logistic_regression_13.png)

![Logistic regression 14](ML_img/logistic_regression_14.png)

![Logistic regression 15](ML_img/logistic_regression_15.png)

![Logistic regression 16](ML_img/logistic_regression_16.png)

#### 并行化求解梯度下降或牛顿法

* 逻辑回归的并行化最主要的就是对目标函数梯度计算的并行化。

* 并行 LR 实际上就是在求解损失函数最优解的过程中，针对寻找损失函数下降方向中的梯度方向计算作了并行化处理，而在利用梯度确定下降方向的过程中也可以采用并行化。

* 我们看到目标函数的梯度向量计算中只需要进行向量间的点乘和相加，可以很容易将每个迭代过程拆分成相互独立的计算步骤，由不同的节点进行独立计算，然后归并计算结果。

* 样本矩阵按行划分，将样本特征向量分布到不同的计算节点，由各计算节点完成自己所负责样本的点乘与求和计算，然后将计算结果进行归并，则实现了按行并行的 LR。按行并行的 LR 解决了样本数量的问题，但是实际情况中会存在针对高维特征向量进行逻辑回归的场景，仅仅按行进行并行处理，无法满足这类场景的需求，因此还需要按列将高维的特征向量拆分成若干小的向量进行求解。

![Logistic regression 17](ML_img/logistic_regression_17.png)

![Logistic regression 18](ML_img/logistic_regression_18.png)

* #### 计算步骤

    * 并行计算总共会被分为两个并行化计算步骤和两个结果归并步骤

![Logistic regression 19](ML_img/logistic_regression_19.png)

![Logistic regression 20](ML_img/logistic_regression_20.png)

![Logistic regression 21](ML_img/logistic_regression_21.png)




-----


### Softmax regression （SMR） - 可用于多分类

* 使用对数线性模型
* softmax 回归是逻辑回归的一般形式,当类别数为 2 时，softmax 回归退化为逻辑回归
* softmax用于多分类过程中，它将多个神经元的输出，映射到（0,1）区间内，可以看成概率来理解，从而来进行多分类！
* 在神经网络中的最后一层隐含层和输出层就可以看成是logistic回归或softmax回归模型，之前的层只是从原始输入数据从学习特征，然后把学习得到的特征交给logistic回归或softmax回归处理。
* Softmax Regression是一个简单的模型，很适合用来处理得到一个待分类对象在多个类别上的概率分布。


* #### 一般步骤
    * Step 1: add up the evidence of our input being in certain classes; 
    * Step 2: convert that evidence into probabilities.

* #### Softmax定义

![Softmax regression 20](ML_img/sr_20.png)

![Softmax regression 1](ML_img/sr_1.png)

![Softmax regression 4](ML_img/sr_4.png)

![Softmax regression 5](ML_img/sr_5.png)


* #### Softmax模型定义
    * 假设有 K 个分类 

![Softmax regression 1](ML_img/sr_1.png)

![Softmax regression 2](ML_img/sr_2.png)

![Softmax regression 3](ML_img/sr_3.png)


* #### Softmax cost function - 定义为 交叉熵

    ![Softmax regression 6](ML_img/sr_6.png)

    * 得到costfunction后的步骤可用梯度下降完成, 根据学习率，进行参数求解
        
        ![Softmax regression 10](ML_img/sr_10.png)
        
         ![Softmax regression 11](ML_img/sr_11.png)
         
    ![Softmax regression 12](ML_img/sr_12.png)

* #### Softmax Regression 多分类例子

![Softmax regression 14](ML_img/sr_14.png)

![Softmax regression 15](ML_img/sr_15.png)

![Softmax regression 16](ML_img/sr_16.png)

![Softmax regression 17](ML_img/sr_17.png)

![Softmax regression 18](ML_img/sr_18.png)

![Softmax regression 19](ML_img/sr_19.png)

原文地址https://www.kdnuggets.com/2016/07/softmax-regression-related-logistic-regression.html 


* #### Softmax Regression 与 logistic regression 的联系

![Softmax regression 7](ML_img/sr_7.png)

![Softmax regression 8](ML_img/sr_8.png)

![Softmax regression 13](ML_img/sr_13.png)


* #### Softmax Regression 过拟合问题 - 处理方法为 `减少特征数量` 或 `正则化`

![Softmax regression 9](ML_img/sr_9.png)


---
















