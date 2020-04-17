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
原始数据和调用库函数形成的数据，本例中的模型为y=ax<sub>1</sub>+bx<sub>2</sub>+c，x<sub>1</sub>表示第一列数字，x<sub>2</sub>表示第二列数字，两者为自变量，y为因变量，表示最后一列数字<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/简单线性回归结果.png)<br>
**多元线性回归是用线性的关系来拟合一个事情的发生规律,找到这个规律的表达公式,将得到的数据带入公式以用来实现预测的目的,我们习惯将这类预测未来的问题称作回归问题.机器学习中按照目的不同可以分为两大类:回归和分类.逻辑回归就可以用来完成分类任务。**
## 逻辑回归
**首先要明确一点，逻辑回归不是用来做回归的，而是用回归的方法来做分类任务。**<br>
### 判定函数sigmod以及阈值的选择
按照多元线性回归的思路,我们可以先对这个任务进行线性回归,学习出这个事情结果的规律,比如根据人的饮食,作息,工作和生存环境等条件预测一个人"有"或者"没有"得恶性肿瘤,可以先通过回归任务来预测人体内肿瘤的大小,取一个平均值作为阈值,假如平均值为y,肿瘤大小超过y为恶性肿瘤,无肿瘤或大小小于y的,为非恶性.这样通过线性回归加设定阈值的办法,就可以完成一个简单的二分类任务.但是使用线性的函数来拟合规律后取阈值的办法是行不通的,行不通的原因在于拟合的函数太直,离群值(也叫异常值)对结果的影响过大。<br>
因此在逻辑回归中选用sigmod函数(又称为Logistic函数)![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/sigmod函数.png)作为判别函数,函数图像为![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/sigmod图像.png)该函数具有很强的鲁棒性(健壮和强壮的意思),并且将函数的输入范围(∞,-∞)映射到了输出的(0,1)之间且具有概率意义.`具有概率意义：将一个样本输入到我们学习到的函数中,输出0.7,意思就是这个样本有70%的概率是正例,1-70%就是30%的概率为负例.`<br>
**我们利用线性回归的办法来拟合然后设置阈值的办法容易受到离群值的影响,sigmod函数可以有效的帮助我们解决这一个问题,所以我们只要在拟合的时候把判定函数换成**![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/逻辑回归判定函数.png)，因为g(z)函数的特性,它输出的结果也不再是预测结果,而是一个值预测为正例的概率,预测为负例的概率就是1-g(z).<br>
在逻辑回归中，因为判定函数输出的是一个事件的概率，我们需要设定一个**阈值**来判断这个概率的时间属于哪一类事件，但是在设定阈值的过程中往往是根据实际情况来判断的，不能只是简单的取0.5，比如，我们做了一个肿瘤的良性恶性判断.选定阈值为0.5就意味着,如果一个患者得恶性肿瘤的概率为0.49,模型依旧认为他没有患恶性肿瘤,结果就是造成了严重的医疗事故.此类情况我们应该将阈值设置的小一些.阈值设置的小,加入0.3,一个人患恶性肿瘤的概率超过0.3我们的算法就会报警,造成的结果就是这个人做一个全面检查,比起医疗事故来讲,显然这个更容易接受.<br>
**综上所述，利用逻辑回归进行分类的主要思想是：根据现有数据对分类边界线建立回归公式，以此进行分类,而求解逻辑回归，就是找到一组系数theta(θ)令判定函数预测正确的概率最大**
### 交叉熵损失函数
根据上述判定函数可得每个单条样本预测正确概率的公式:![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/逻辑回归概率公式.png)，若想让预测出的结果全部正确的概率最大,根据[最大似然估计](https://blog.csdn.net/weixin_39445556/article/details/81416133),就是所有样本预测正确的概率相乘得到的P(总体正确)最大,此时我们令逻辑回归判定函数为![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/逻辑回归判定函数2.png)，则单条样本预测正确概率为![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/逻辑回归概率公式2.png)，为了简化计算，我们对L(θ)这个函数采取两边取log对数的方法，得到<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/逻辑回归概率公式3.png)<br>
l(θ)越大，证明我们得到的系数theta越好，因为在函数最优化时我们习惯求最小值，所以我们在l(θ)中加上符号，得到逻辑回归的损失函数，在这里称之为交叉熵损失函数<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/逻辑回归损失函数.png)<br>
### [求解梯度(对损失函数求偏导)](https://blog.csdn.net/weixin_39445556/article/details/83661219)
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/求解梯度函数.png)<br>
交叉熵损失函数的梯度和最小二乘的梯度形式上完全相同,区别在于,此时的拟合函数![](http://latex.codecogs.com/gif.latex?\\\h_{\theta}(x)=g(z)),而最小二乘的拟合函数是![](http://latex.codecogs.com/gif.latex?\\\h_{\theta}(x)=W^{T}X)
### 结语
使用线性模型进行分类第一个要面对的问题就是如何**降低离群值的影响**,而第二大问题就是,在正负例数据比例相差悬殊时预测效果不好.原因来自于逻辑回归交叉熵损失函数是通过最大似然估计来推导出的.使用最大似然估计来推导损失函数,那无疑,我们得到的结果就是所有样本被预测正确的最大概率.注意重点是我们得到的结果是预测正确率最大的结果<br>比如：100个样本预测正确90个和预测正确91个的两组w,我们会选正确91个的这一组.那么,当我们的业务场景是来预测垃圾邮件,预测黄色图片时,我们数据中99%的都是负例(不是垃圾邮件不是黄色图片),如果有两组w,第一组为所有的负例都预测正确,而正利预测错误,正确率为99%,第二组是正利预测正确了,但是负例只预测出了97个,正确率为98%.此时我们算法会认为第一组w是比较好的.但实际我们业务需要的是第二组,因为**正例检测结果才是业务的根本**. 此时我们需要对数据进行`欠采样/重采样`来让正负例保持一个差不多的平衡,或者使用`树型算法`来做分类.一般树型分类的算法对数据倾斜并不是很敏感,但我们在使用的时候还是要对数据进行欠采样/重采样来观察结果是不是有变好.
## 朴素贝叶斯
### 贝叶斯推断
条件概率是指事件A在另外一个事件B已经发生条件下的发生概率，记P(A|B)。![](http://latex.codecogs.com/gif.latex?\\\P(A|B)=\frac{P(AB)}{P(B)}=\frac{P(B|A)P(A)}{P(B)})，此外根据全概率公式，条件概率的另一种写法是![](http://latex.codecogs.com/gif.latex?\\\P(A|B)=\frac{P(B|A)P(A)}{P(B|A)P(A)+P(B|{A}')P{A}'}) 贝叶斯公式中，P(A)称为"先验概率"（Prior probability），即在B事件发生之前，对A事件概率的一个判断。P(A|B)称为"后验概率"（Posterior probability），即在B事件发生之后，对A事件概率的重新评估。P(B|A)/P(B)称为"可能性函数"（Likelyhood），这是一个调整因子，使得预估概率更接近真实概率。所以，条件概率可以理解成下面的式子：后验概率＝先验概率 ｘ 调整因子，这就是`贝叶斯推断`的含义。我们先预估一个"先验概率"，然后加入实验结果，看这个实验到底是增强还是削弱了"先验概率"，由此得到更接近事实的"后验概率"。因为在分类中，只需要找出可能性最大的那个选项，而不需要知道具体那个类别的概率是多少，所以为了减少计算量，全概率公式在实际编程中可以不使用。
### [朴素贝叶斯分类](https://blog.csdn.net/guoyunfei20/article/details/78911721?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1)
**朴素贝叶斯的思想基础是这样的：对于给出的待分类项，求解在此项出现的条件下各个类别出现的概率，哪个最大，就认为此待分类项属于哪个类别**<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/朴素贝叶斯分类.png)<br>
可以看到，整个朴素贝叶斯分类分成了三个阶段：
* 准备工作阶段，任务是为朴素贝叶斯分类做必要的准备，主要工作是根据具体情况确定特征属性，并对每个特征属性进行适当划分，然后由人工对一部分待分类项进行分类，形成训练样本集合。这一阶段的输入是所有待分类数据，输出是特征属性和训练样本。这一阶段是整个朴素贝叶斯分类中唯一需要人工完成的阶段，其质量对整个过程将有重要影响，分类器的质量很大程度上由特征属性、特征属性划分及训练样本质量决定。
* 分类器训练阶段，这个阶段的任务就是生成分类器，主要工作是**计算每个类别在训练样本中的出现频率及每个特征属性划分对每个类别的条件概率估计**，并将结果记录。其输入是特征属性和训练样本，输出是分类器。这一阶段是机械性阶段，根据前面讨论的公式可以由程序自动计算完成。
* 应用阶段，这个阶段的任务是使用分类器对待分类项进行分类，其输入是分类器和待分类项，输出是待分类项与类别的映射关系。这一阶段也是机械性阶段，由程序完成。
### 数据类型 [举例](https://blog.csdn.net/qiu_zhi_liao/article/details/90671932)
在进行朴素贝叶斯分类时，要注意数据时离散型还是连续型，当特征属性为离散值时，只要很方便的统计训练样本中各个划分在每个类别中出现的频率即可用来估计P(a|y)。当特征属性为连续值时，通常假定其值服从`高斯分布（也称正态分布）`。因此只要计算出训练样本中各个类别中此特征项划分的各均值和标准差，代入正态分布公式即可得到需要的估计值。另一个需要讨论的问题就是当P(a|y)=0怎么办，当某个类别下某个特征项划分没有出现时，就是产生这种现象，这会令分类器质量大大降低。为了解决这个问题，我们引入`Laplace校准`，它的思想非常简单，就是对每类别下所有划分的计数加1，这样如果训练样本集数量充分大时，并不会对结果产生影响，并且解决了上述频率为0的尴尬局面。
### 总结
朴素贝叶斯分类常用于文本分类，尤其是对于英文等语言来说，分类效果很好。它常用于垃圾文本过滤、情感预测、推荐系统等。<br>
实际应用方式：
- 若任务对预测速度要求较高，则对给定的训练集，可将朴素贝叶斯分类器涉及的所有概率估值事先计算好存储起来，这样在进行预测时只需要 “查表” 即可进行判别；
- 若任务数据更替频繁，则可采用 “懒惰学习” (lazy learning) 方式，先不进行任何训练，待收到预测请求时再根据当前数据集进行概率估值；
- 若数据不断增加，则可在现有估值的基础上，仅对新增样本的属性值所涉及的概率估值进行计数修正即可实现增量学习。







