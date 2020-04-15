# Sklearn-Algorithm
## 线性回归
**简单线性回归(simple linear regression)** 简单线性回归通常就是包含一个自变量x和一个因变量y，这两个变量可以用一条直线来模拟。如果包含两个以上的自变量就叫做**多元回归(multiple regresseion)** 被用来描述因变量y和自变量x以及偏差error之间关系的方程叫做回归模型<br>
**线性回归的目的是要得到输出向量Y和输入特征X之间的线性关系，求出`线性回归系数`。为了得到线性回归系数θ，我们需要定义一个`损失函数`，一个极小化损失函数的优化方法，以及一个验证算法的方法。损失函数的不同，损失函数的优化方法的不同，验证方法的不同，就形成了不同的线性回归算法。**
### 损失函数
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0.png) <br>
对于这个损失函数，一般有梯度下降法和最小二乘法两种极小化损失函数的优化方法，sklearn中提供的LinearRegression用的是最小二乘法，因为最小二乘法在概率论，统计学等课中学过，所以在这里详细讲解梯度下降法。<br>
### [梯度下降算法](https://www.jianshu.com/p/386645b7e03a)
梯度下降法是一种在学习算法及统计学常用的最优化算法，其思路是对theta取一随机初始值，可以是全零的向量，然后不断迭代改变θ的值使其代价函数J（θ）根据梯度下降的方向减小，直到收敛求出某θ值使得J（θ）最小或者局部最小。其更新规则为：
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/%E6%A2%AF%E5%BA%A6%E4%B8%8B%E9%99%8D.png)<br>
其中alpha为学习率，控制梯度的下降速度，一般取0.01，0.03，0.1，0.3，…, J(θ)对θ的偏导决定了梯度下降的方向，将J(θ)带入更新规则中得到：
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/梯度下降2.png)
- 为什么梯度下降可以逐步减小代价函数
 - 假设函数`f(x)`
 - 泰勒展开：`f(x+△x)=f(x)+f'(x)*△x+o(△x)`
 - 令：`△x=-α*f'(x)`   ,即负梯度方向乘以一个很小的步长`α`
 - 将`△x`代入泰勒展开式中：`f(x+△x)=f(x)-α*[f'(x)]²+o(△x)`
 - 可以看出，`α`是取得很小的正数，`[f'(x)]²`也是正数，所以可以得出：`f(x+△x)<=f(x)`
 - 所以沿着**负梯度**放下，函数在减小，多维情况一样。
 根据本案例中的数据得到梯度下降(代价随迭代次数的变化)的结果为![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/梯度下降结果.png)<br>
 ### [数据归一化(Normalization)](https://www.jianshu.com/p/95a8f035c86c)
 - 目的是使数据都缩放到一个范围内，便于使用梯度下降算法
 - 归一化有很多种方法，其中最常见的是**Min-Max归一化，z-score,平均归一化***
 - 归一化是把数据变成（0，1）或者（1，1）之间的小数。主要是为了数据处理方便提出来的，把数据映射到0～1范围之内处理，更加便捷快速。归一化还可以把有量纲表达式变成无量纲表达式，便于不同单位或量级的指标能够进行比较和加权。归一化是一种简化计算的方式，即将有量纲的表达式，经过变换，化为无量纲的表达式，成为纯量。
### 结果
原始数据和调用库函数形成的数据，本例中的模型为y=ax<sub>1<sub>+bx<sub>2<sub>+c，x<sub>1<sub>表示第一列数字，x<sub>2<sub>表示第二列数字，两者为自变量，y为因变量，表示最后一列数字
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/简单线性回归结果.png)







Sklearn机器学习中的主要算法原理以及实现
