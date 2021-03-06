# 第二章 线性代数

## 2.1 标量、向量、矩阵和张量

* 标量：一个单独的数
* 向量： 一列有序排列的数，如果该向量有n个属于实数集的元素，记为$R^n$
* 矩阵：二维数组。
* 张量：超过两维的数组。
* 转置操作：$(A^T)_{i,j}=A_{j,i}$ ， $(BA)^T=A^TB^T$

## 2.2 矩阵和向量相乘

* 矩阵乘法：$C=AB$，服从分配率和结合律。注意的是矩阵乘积不是对应元素的相乘
* 元素对应乘积：$C=A \bigodot B$

* 同维度向量的点积：$x^Ty$，满足交换律$x^Ty=yx^T$，点积的结果为标量

## 2.3 单位矩阵和逆矩阵

* 单位矩阵：所以沿着主对角线的元素都是1，而所以其他位置的元素都是0。记为$I_n$,$I_n \in R_{n*n}$
* 逆矩阵：$A^{-1}A=I_n$，

## 2.4 线性相关和生成子空间

* 线性组合：一组向量的线性组合是指每个向量乘以对应标量系数之后的和：$\sum_i c_iv^{(i)}$

* 生成子空间：一组向量的生成子空间是原始向量线性组合后所能抵达的点的集合。确定$Ax=b$是否有解相当于确定$b$是否在$A$列向量的生成子空间中。这个生成子空间成为$A$的列空间。
* 线性无关：一组向量中的任意一个向量都不能表示成其他向量的线性组合，这组向量是线性无关的，线性相关反之。

## 2.4 范数

范数用于衡量向量的大小

* $L_p$的范数定义：$||x||_p=(\sum_i |x_i|^p)^{1/p}$ 
* $L^2$范数：当$p=2$，在机器学习中使用频繁，经常简化为$||x||_p$
* 平方$L^2$范数：$x^Tx$
* $L^1$范数：$||x||_i=\sum_i |x_i|$，用于区分零的元素和非零但值很小的元素。
* $L^0$范数：非零元素个数，经常用$L^1$范数代替。
* $L^{\infty}$最大范数：$||x||_{\infty}=max_i |x_i|$
* F范数：$||A||_F=\sqrt{\sum_{i,j}{A^2_{i,j}}}$

## 2.5 特殊特性的矩阵和向量

* 对角矩阵：只在主对角线上含有非零元素，其他位置都是零。$diag(v)x=v\bigodot x$,$diag(v)^{-1}=diag([1/v_1,...,1/v_n])$
* 对称矩阵：$A=A^T$
* 单位向量：$||x||_2=1$ 
* 正交：若$x^Ty=0$，则$x$和$y$是正交的，若他们范数都是1，则为标准正交。
* 正交矩阵：行向量和列向量是分别标准正交的方阵：$A^{-1}=A^T$

## 2.6 特征分解

通过分解矩阵来发现不明显的函数性质。

* 特征分解：$Av=\lambda v$，$A$的特征分解可以记为：$A=Vdiag(\lambda）V^{-1}$ 。
* 不是每个矩阵都可以分解成特征值和特征向量。但每个实对称矩阵都可以分解成实特征向量和实特征值，实对称矩阵特征分解可能不唯一。

## 2.7 奇异值分解

每个实数矩阵都有一个奇异值分解，但是不一定有特征分解。

* $A$的奇异值分解：$A=UDV^T$

## 2.8 伪逆

伪逆求解线性方程众多可能解法的一种

* $A^+=VD^+U^T$

## 2.9 逆运算

* 逆运算返回的实矩阵对角元素的和：$Tr(A)=\sum_t A_{t,t}$
* 用逆运算表示表达式，可以使用很多有用的等式巧妙地处理表达式。

## 2.10 行列式

* 行列式，记为$det(A)$，是一个将方阵$A$映射到实数的函数。行列式等于矩阵特征值的乘积。 

# 第三章 概率和信息论

## 3.1 为什么要使用概率

这是因为机器学习通常必须处理不确定量，有时候也可能需要处理随机量。不确定行有三种：

* 被建模系统内在的随机性：例如纸牌洗成随机顺序。
* 不完全观测：即使是确定的系统，若我们无法观测到系统的所有变量，该系统也是呈现随机性的。
* 不完全建模：某些舍弃的信息导致预测的不确定性。

概率分类

* 频率派概率：直接与事件发生的概率相联系
* 贝叶斯概率：涉及到确定性水平

## 3.2 随机变量 

* 随机变量是可以随机地取不同值的变量

## 3.3 概率分布 

概率分布用来描述随机变量或一簇变量在每一个可能取到状态的可能性大小。我们描述概率分布的方式取决于概率是离线的还是连续的。

### 3.3.1 离散型变量和概率质量函数

* 离散型变量的概率分布可以用概率质量函数$P$来描述
* 概率质量函数可以同时作用与多个随机变量，这种多个变量的概率分布被称为联合概率分布$P(x,y)$

### 3.3.2 连续型变量和概率密度函数

* 若研究变量为连续性随机变量时，用概率密度函数$p$来描述，其必须满足下面的条件：
  * $p$的定义域必须时$x$所以可能状态的集合
  * $\int p(x)dx=1$

* 概率密度函数并没有给出特定状态的概率，它是给出某个区间的概率

## 3.4 边缘概率

* 边缘概率：给出一组变量的联合概率分布，某子集的概率分布为边缘概率分布
* 离散型：求和
* 连续型：积分

## 3.5 条件概率

* 条件概率：某件事情在给定其他事情发生时出现的概率：$P(Y=y|X=x)=\frac{P(Y=y,X=x)}{P(X=x)}$

## 3.6 条件概率链式法则 

* 任何多维随机变量的联合概率分布都可以分解成只有一个变量的条件概率相乘的形式$P(x^1,...,x^n)=P(x^1)\prod_{i=2}^nP(x^i|x^1,...,x^{i-1})$

## 3.7 独立性和条件独立性

* 相互独立：$P(X=x,Y=y)=P(X=x)P(Y=y)$
* 条件独立：$P(X=x,Y=y,Z=z)=P(X=x|Z=z)P(Y=y|Z=z)$

## 3.8 期望、方差和协方差

* 函数$f(x)$关于某分布$P(x)$的期望是指，当$x$由$p$产生后，$f$作用于$x$时，$f(x)$的平均值。对于离散变量，$E_{x~p}[f(x)]=\sum_xP(x)f(x)$。对于连续型随机变量，$E_{x~p}[f(x)]=\int p(x)f(x)$
* 期望是线性的：$E_x[af(x)+b(gx)]=aE_x[f(x)+bE_x(g(x))]$
* 方差衡量的是当我们对$x$依靠它的概率分布进行采样时，随机变量$x$的函数值呈现多大的差异：$Var(f(x))=E[(f(x)-E[f(x)])^2]$
* 协方差在某种意义上给出了了两个变量线性相关性的强度以及浙西变量的尺度：$Cov(f(x),g(x))=E[(f(x)-E[f(x)])(g(y)-E[g(y)])]$
* 协方差的绝对值如果很大就意味着变量值变化很大并且同时同时距离各自的均值很远。
* 协方差和相关性是有联系的，因为两个变量如果相互独立那么它们的协方差是零，如果两个变量的协方差不为零那么它们一定是相关的。
* 协方差矩阵：$Conv(x)_{i,j}=Cov(x_i,x_j)$

## 3.9 常用概率分布

### 3.9.1 Bernoulli分布

* Bernoulli分布是单个二值随机变量的分布。
* 性质
  * $P(x=1)=\phi$
  * $P(x=0)=1-\phi$
  * $P(X=x)=\phi^2(1-\phi)^{1-x}$
  * $E_x[x]=\phi$
  * $Var_x(x)=\phi(1-\phi)$

### 3.9.2 Multinoulli分布

* Multinoulli分布是指具有k个不同状态的单个离散型随机变量上的分布

### 3.9.3 高斯分布

* 高斯分布：$N(x;\mu,\sigma^2)=\sqrt{\frac{1}{2\pi}\sigma^2}exp(-\frac{1}{2\sigma^2}(x-\mu)^2)$，其中$\mu$是中心坐标，也是均值，$\sigma$是分布标准差，方差是$\sigma^2$
* 当缺乏关于某某分布的先验知识时候，正太分布是默认比较好的选择。
  * 很多分布都是接近正太分布
  * 在具有相同方差的所以可能的概率分布中，正太分布在实数上具有最大的不确定性。

### 3.9.4 指数分布和Laplace分布

* 经常需要在一个$x=0$点取得边界点的分布。为了实现这个目的，使用指数分布：$p(x;\lambda)=\lambda 1_{x>=0}exp(-\lambda x)$
* Laplace分布允许我们在任意一点$\mu$处设置概率质量的峰值：$L(X,\mu,\gamma)=\frac{1}{2\gamma}exp(-\frac{|x-\mu|}{\gamma})$

### 3.9.5 Dirac分布和经验分布

* 我们坑你希望概率分布的所以质量都集中在一个点上，这可以通过Dirac delta函数定义的概率密度函数来实现：$p(x)=\delta(x-\mu)$

### 3.9.6 分布的混合

* 通过组合一些简单分布得到新的概率分布，混合分布由那个组件分布组成，样本由哪个组件分布产生取决与从一个Multinoulli分布中的采样结果：$P(x)=\sum_iP(c=i)P(x|c=i)$

### 3.10 常用函数一些有用的性质

* logistic sigmoid函数常用来产生Bernoulli分布的参数$\phi$：$\sigma(x)=\frac{1}{1+exp(-x)}$
* softplus函数用来产生正态分布的$\mu,\sigma$：$\zeta(x)=log(x+exp(x))$

## 3.11 贝叶斯规则

* 需要在已知$P(y|x)$时计算$P(x|y)$，可以使用贝叶斯规则$P(x|y)=\frac{P(x)P(y|x)}{P(y)},P(y)=\sum_xP(y|x)P(x)$,

## 3.12 连续型变量的技术细节

* 测度论用严格的方式描述那些非常微小的点集，称为“零测度”。
* 如果某个性质几乎处处成立，那么它在整个空间中除了测度为0的集合以外都是成立的。
* 处理那种相互之间由确定性函数关系的连续型变量如$y=g(x)$，容易没考虑引入函数后的空间变形

## 3.13 信息论

* 信息论主要研究一个信号包含信息的多少进行量化。它的基本思想是一个不太可能的事件居然发生了，要比一个非常可能的请见发生能提供更多的信息。
* 自信息：$I(x)=-logP(x)$，单位是奈特，一奈特是以$1/e$的概率观测到一个事件时获得信息量。
* 自信息只是处理单个输出，我们可以用香浓熵对整个概率分布中不确定总量进行量化：$H(x)=E_{x~p}[I(x)]=-E_{x~p}[logP(x)]$
* 同一个随机变量x有两个单独的概率分布$P(x)，Q(x)$用KL散度来衡量这两个差异：$D_{KL}(P||Q)=E_{x~p}[logP(x)-logQ(x)]$
* 一个和KL散度密切连续的量是交叉熵，$H(P,Q)=H(P)+D_{KL}(P||Q)$

## 3.14 结构化概率模型

* 通常概率分布涉及到的直接相互作用都是介于非常少的变量之间。使用单个函数来描述整个联合概率分布是非常低效的，我们可以把概率分布分解成许多因子的乘积形式。这种分解极大地减少用来描述一个分布的参数变量。
* 用图来表示概率分布的分解，称为结构化概率模型

# 第四章数值计算

## 4.1 上溢和下溢

* 极具毁灭性的舍入误差是**下溢**，当接近零时候四舍五入为零发生下溢
* 极具破坏力的数值错误形式是**上溢**，当大量级的数近似无穷或者负无穷时候发生上溢
* 实现新算法时候应该保持数值稳定

## 4.2 病态条件

* 输入被微小扰动而迅速改变函数

## 4.3 基于梯度的优化方法

* 导数为0的点成为**临界点**。一个**局部极小值**意味着这个点的邻近值小于邻近点。一个**局部极大值**意味着这个点的邻近值大于邻近点。
* 有些临界点不是最小点也不是最大点，成为**鞍点**
* 在多维的情况下，临界点是梯度中所有元素为零的点
* 梯度下降更新的点为$x'=x-f(x)$

## 4.3.1 Jacobian和Hessian矩阵

* Jacobian矩阵：包含偏导数的矩阵，函数$f$的Jacobian矩阵为$J_{i,j}=\frac{\partial}{\partial x_j}f(x)_i$
* 二阶导数是对曲率的衡量，二阶导为负，曲线往下凹，二阶导为正，曲线往上凹
* 包含二阶导数的矩阵成为Hessian矩阵，Hessian矩阵$H(f)(x)$定义为：$H(f)(x)_{i,j}=\frac{\partial^2}{\partial x_i \partial x_j}f(x)$
* 任何二阶盘偏导数连续的点处都是可交换，也就是说$H_{i,j}=H_{j,i}$，因此Hessian矩阵的这些点都是对称的。
* 在深度学习背景下，Hession几乎处处对称，所以是实对称矩阵。
* 通过检测Hessian矩阵的特征值来判断该临界点是局部极大值(Hession所有特征值正定)，局部极小值(Hession所有特征值负)和鞍点(Hession特征值有正有负)
* 一阶优化算法：仅用梯度信息
* 二阶优化算法：使用Hession矩阵的优化算法

# 第五章 机器学习基础

## 5.1 学习算法

### 5.1.1任务$$T$$

* 机器学习可以解决很多任务，包括：分类，输入缺失分类，回归，转录，机器翻译，结构化输出，异常检测，合成和采样，缺失值填补，去噪，密度估计或概率质量函数估计。

### 5.1.2 性能度量$P$

* 准确率 错误率

### 5.1.3 经验$E$

* 无监督学习算法训练含有很多特征的数据集，然后学习处这个数据集上有用的性质。
* 监督学习算法训练含有很多特征的数据集，不过数据集中的样本都有标签。
* 无监督和有监督学习的界限是模糊的。

## 5.2 容量、过拟合和欠拟合

* 在未观测到的输入上表现良好的能力称为泛化
* 独立同分布假设：每个数据集中的样本都是彼此相互独立的，并且训练集和测试集都是同分布的，才样子相同的分布。
* 算法不好的因素
  * 降低了训练误差
  * 缩小训练误差和测试误差的差距
* 模型的容量是指其拟合各种函数的能力，容量低的模型可能很难拟合训练集，容量高的模型可能会过拟合。一种控制训练算法容量的方法是选择假设空间。
* 从预先知道真实分布$P(x,y)$预测而出现的误差被称为贝叶斯误差

### 5.2.1没有免费午餐定理

* 没有免费午餐定理：在所有可能的数据生成分布上平均之后，每个分类算法在未事先观测的点都有相同的错误率。这结论仅在考虑所有可能的数据生成分布时才成立

### 5.2.2 正则化

* 给代价函数添加正则项目：$w^Tw$

## 5.3 超参数和验证集

### 5.3.1 交叉验证

* 测试集太小很难判断算法好不好，但也有替代方法允许我们使用所有的样本估计平均测试误差，代价是增加了计算量

* k-折交叉验证过程：将数据集分成k个不重合的子集，测试误差可以估计k次计算后的平均测试误差，在第i次测试时，数据的第i个子集用于测试，其他数据用于训练

## 5.4 估计、偏差和方差

### 5.4.1 点估计

* 点估计试图为一些感兴趣的量提供单个"最优"预测
* 另{$x^{(1)},...,x^{m}$}是m个独立同分布的数据点，点估计是这些数据的任意函数：$\theta^{‘}_m=g(x^{(1)},...,x^{m})$
* 点估计也可以指输入和目标变量之间的关系的估计。

### 5.4.2 偏差

* 估计的偏差被定义为:$bias(\theta_m^{'}=E(\theta_m^{'})-\theta)$

### 5.4.3 方差和标准差

* 我们有时候会考虑估计量的变化程度是多少$Var(\theta^{’})$

### 5.4.4 权衡偏差和方差以最小化均方误差

* 均方误差：$MSE=E[(\theta_m^{’}-\theta)^2]=Bias(\theta_m^{’})^2+Var(\theta_m^{’})$

### 5.4.5 一致性

* 我们希望当数据集中的数量m增加时，点估计会手链到对应参数的真实值。
* 一致性保证了估计量的偏差会随着数据样本数量的增多而减少，但是，渐进无偏差并不意味着一致性。

## 5.5 最大似然估计

我们希望有一些准则可以让我们从不同的模型中得到特定函数作为好的估计。这个常用准则就是最大似然估计：$\theta_{ML}=argmax_{\theta}p_{model}(X;\theta)=argmax_{\theta}\prod p_{model}(x_{(i)};\theta)$

### 5.5.1 条件对数似然和均方误差

* 条件最大似然估计：$\theta_{ML}=argmax_{\theta}P(Y|X;\theta)=argmax_{\theta}\sum logP(y^i|x^i;\theta)$

### 5.5.2 最大似然的性质

* 具有一致性

## 5.6 贝叶斯统计

* $p(\theta|x^{(1)},...,x^{m})=\frac{p(x^{(1)},...,x^{m}|\theta)p(\theta)}{p(x^{(1)},...,x^{m})}$

* 贝叶斯和似然估计的区别：
  * 贝叶斯使用$\theta$的全分布，最大似然估计使用$\theta$的点估计
  * 贝叶斯是使用现验知识

### 5.6.1 最大后验估计

* $\theta_{MAP}=argmax_{\theta}p(\theta|x)=argmax_{\theta}\ logp(x|\theta)+log\ p(\theta)$

### 5.7 监督学习算法

### 5.7.1 概率监督学习

### 5.7.2 支持向量机

* 类似于逻辑回归，支持向量机也是基于线性函数$W^Tx+b$，不同于逻辑回归的是，它不输出概率，指输出类别
* 使用核技巧，支持向量机使用如下的函数进行预测：$f(x)=b+\sum_i a_ik(x,x^{(i)})$

### 5.7.3 其他简单的监督学习算法 

* 决策树是将输入空间分成不同区域，每个区域有独立参数的算法。

### 5.8 无监督学习算法

### 5.8.1 主成分分析

* PCA学习一种比原始输入维度更低的表示，也学习一种元素之间彼此没有线性相关的表示。PCA学习一种线性投影，使得最大方差的方向与新空间的轴对齐。
* PCA是保留数据尽量多信息降维方法。

### 5.8.2 k-均值聚类

* k-均值聚类算法降训练集分成k个靠近彼此的不同样本聚类。

## 5.9 随机梯度下降

* $g=\frac{1}{m^{‘}} \nabla_\theta L(x^{(i)},y^{(i)},\theta)$,$\theta \longleftarrow \theta-\epsilon g$

## 5.10 构建机器学习算法

* 几乎所有的深度学习算法都可以描述为一个相当简单的配方：特点数据集，代价函数，优化过程和模型。

### 5.11 促使深度学习发展的挑战

* 维度灾难
* 局部不变性和平滑正则化
* 流程学习



# 第六章 深度前馈网络

## 6.1学习XOR

* 线性回归无法对XOR函数进行逼近
* 简单的深度前馈网络可以对XOR进行逼近

## 6.2 基于梯度的学习

### 6.2.1 代价函数

* 使用最大似然函数条件分布：$J(\theta)=-E_{x,y~P_{data}} log \ p_{model(y|x)}=1/2E_{x,y~P_{data}}||y-f(x,\theta)||^2+contant$

* 学习条件统计量：$f^{*}=argmin_f E_{x,y~P_{data}}||y-f(x)||^2=E_{y~Pdata(y|x)}[y]$

### 6.2.2 输出单元

* 用于高斯输出分布的线性单元：给定特征$h$,线性输出单元层产生一个向量$y'=W^Th+b$
* 用于Bernoulli输出分布的sigmoid单元：$y'=\sigma(w^{T}h+b)$
* 用于Multinoulli输出分布的softmax单元：$softmax(z)_i=\frac{exp(z_i)}{\sum_j exp(z_j)}$

## 6.3 隐藏单元

一些隐藏单元可能并不是在所有输入点上都是可微的，如rulu。但是它在深度学习中表现也很好，部分原因是神经网络训练算法通常不会达到代价函数的局部最小值，仅仅显著地减少它的值。代价函数的最小值对应于梯度未定义的点是可以接受的。

### 6.3.1 整流线性单元以及扩展

* relu：$g(x)=max{0,z}$
* 扩展：绝对值整流，渗漏整流线性单元，参数化整流线性单元

### 6.3.2 logistic sigmoid 与双曲正切函数

* logistic sigmoid：$g(z)=\sigma(z)$
* 双曲正切激活函数：$g(z)=tanh(z)$

## 6.4 架构设计

### 6.4.1 万能近似性质和深度

* 万能近似性质和深度：一个前馈神经网络如果具有线性输出层和至少一层具有一种“挤压”性质的激活函数的隐藏层，只要给予网络足够数量的隐藏单元，就可以以任何精度阿里近似任何一个有限维空间到另一个有限维空间的Borel可测函数。

### 6.4.2 其他架构上考虑

* 另一个关键点的考虑是如何将层与层之间连接起来

## 6.5 反向传播和其他微分算法

* 使用更精确的计算图来描述反向传播算法
* 微分中链式法则用于计算复合函数的导数

# 第七章 深度学习中的正则化

## 7.1 参数范数惩罚

* 通过给目标函数添加参数范数惩罚：$J'(\theta;X,y)=J(\theta;X,y)+a\Omega(\theta)$

* 当我们训练算法$J'$时候，它会降低原始目标$J$关于训练数据的误差并同时减小在某些衡量标准下参数$\theta$的规模

### 7.1.1 $L^2$参数正则化

* 权重衰减的$L^2$参数范数惩罚：$\Omega(\theta)=1/2||w||^2_2$使得权重更加接近原点
* $w\leftarrow(1-\epsilon a)w-\epsilon \nabla_wJ(w;X,y)$加入权重衰减后会引起学习规则的修改，梯度较之前收缩了权重向量

### 7.1.2 $L^1$参数正则化

* $L^1$正则化被定义：$\Omega(\theta)=||w||_1=\sum_t|w_i|$

* 相比于$L^2$正则化，$L^1$正则化会产生更加稀疏的解。

## 7.2 作为约束的范数惩罚

* 如果想约束$\Omega$小于某个常数$k$，可以构建广义拉格朗日函数：$L(\theta,a;X,y)=J(\theta;X,y)+a(\Omega(\theta)-k)$

## 7.3 正则化和欠约束问题

## 7.4 数据集增强

* 让机器学习模型泛化更好的办法是使用更多的数据进行训练
* 注意不要使用会改变类别的变换

## 7.5 噪声鲁棒性

* 对于某些模型而言，想输入添加方差极小的噪声等价于对权重施加范数惩罚。
* 在某些假设下，施加于权重的噪声可以被解释为于更传统的正则化形式等同，鼓励要学习的函数保持稳定。

## 7.6 半监督学习

* 半监督学习通常指的是学习一个表示$h=f(x)$。其目的是使得相同类中的样本有类似的表示。

## 7.7 多任务学习

* 多任务学习是通过合并几个任务中样例来提高泛化的一种方式。当模型的一部分被多个额外任务共享时，这部分将约束为良好的值。
* 不同的监督任务共享相同的输入$x$以及中间层$h^{share}$，能够学习的因素池。
* 模型通常可以分为两类相关参数
  * 具体任务参数
  * 所有无人共享参数

## 7.8 提前终止

* 当训练有足够的表达能力甚至会过拟合的大模型时候，训练误差逐渐减低但验证集误差再次上升。
* 提前终止需要在每次验证集有所改善后，保存模型参数副本，当验证集的误差在某个循环次数没有改善，就把算法终止。
* 提前终止为何具有正则化效果：可以将优化过程的参数空间限制在初始化参数值的小领域内。
* 提前终止可以单独使用或与其他正则化策略结合使用

## 7.9 参数绑定和参数共享

* CNN的参数共享

## 7.10 稀疏表示

* 除了权重衰减直接惩罚模型参数，另外一种策略是惩罚神经网络的激活单元，稀疏化激活单元。
* 表示的正则化可以使用参数正则化中的同种类型的机制实现，即添加惩罚$\Omega$

## 7.11 bagging和其他集成方法

* Bagging是通过结合几个模型降低泛化误差的技术。主要想法是分别训练几个不同的模型然后让所有的模型表决测试样例的输出。这种策略称为模型平均。
* 模型平均奏效的原因是不同的模型通常不会在测试集上产生完全相同的误差。
* 通过公式推导，平均上，集成至少与它的任何成员表现得一样好，并且如果成员的误差是独立的，集成将显著地比其成员表现得更好。
* Bagging涉及构造k个不同的数据集，每个数据集从原始数据集中重复采样构成，和原始数据集具有相同数据量的样例。

## 7.12 Dropout

* Dropout提供了正则化一大类模型的方法，也可以被认为是集成大量深层神经网络的实用Bagging方法。
* 以某个概率丢弃神经元

## 7.13 对抗训练

* 我们可以通过对抗训练减少原来独立同分布的测试集的错误率--在对抗扰动的训练集样本上训练网络。

# 第八章 深度模型中的优化

## 8.1 学习和纯优化有什么不同

* 机器学习的优化通常是间接作用，通过减低代价函数来对优化性能度量。但是纯优化最小目标本身。

### 8.1.1 经验风险最小化

*  最小化经验风险：$E_{x,y~p_{data}[L(f(x;x),y)]}=1/m \sum_{i=1}^mL(f(x^{(i)}),y^{(i)}$

### 8.1.2 代理损失函数和提前终止

* 有时候真正关心的损失函数并不能被高效地优化，这时候用代理损失函数为原目标的代理，然后优化代理损失函数。

### 8.1.3 批量算法和小批量算法

* 使用整个训练集的优化算法称为批量梯度算法。
* 每次只是用单个样本的优化算法有时被称为随机算法。
* 使用一个以上又不是全部的训练样本的优化算法称为小批量算法，有时简单地称为随机方法

## 8.2 神经网络优化中的挑战

* Hessian矩阵的病态，体现在随机梯度下降会"卡"在某些情况，即使很小的更新步长也会增加代价函数。

* 局部极小值
* 高原 鞍点和其他平坦区域
* 悬崖和梯度爆炸
* 当计算图极深时候，优化算法会面临长期依赖的问题，由于变深的结构使得模型丧失了学习到先前信息的能力，让优化变得及其困难
* 非精确梯度
* 局部和全局结构间的弱对应

## 8.3 基本算法

### 8.3.1 随机梯度下降

* SGD算法的关键参数是学习率，在实践中，有必要随着时间的推移逐渐降低学习率。
* 在实践中，一般会线性衰减学习率直到第$\tau$次迭代：$\epsilon_k=(1-a) \epsilon_0 + a\epsilon_{\tau}$

### 8.3.2 动量

* 动量方法旨在加速学习，特别是处理高区率，小但一致的梯度或是带噪声的梯度。动量算法积累了之前梯度指数级衰减的移动平均，并且继续沿该方向移动。
* 更新：$v\leftarrow av-\epsilon\nabla_{\theta}(\frac{1}{m} \sum L(f(x^{(i)};\theta),y^{(i)}))， \ $$\theta \leftarrow \theta+v$

### 8.3.3 Nesterov 动量

* $v\leftarrow av-\epsilon\nabla_{\theta}[\frac{1}{m} \sum LL(f(x^{(i)};\theta+av),y^{(i)}))]， \ $$\theta \leftarrow \theta+v$

## 8.4 参数初始化策略

### 8.5 自适应学习率算法

### 8.5.1 AdaGrad算法

* 独立地适应所有模型参数的学习率，缩放每个参数反比于其所有梯度历史均方值总和的平方根
* 具有损失的最大导数有快速下降的学习率，小偏导的参数有学习率上有相对较小的下降。

### 8.5.2 RMSProp

* RMSProp算法修改Ada，改变梯度积累为指数加权的平均移动。

### 8.5.3 Adam

* 动量直接并入了梯度一阶距的估计。
* Adam包括了偏置的修正。

## # 第九章 卷积网络

## 9.1 卷积运算

* 卷积运算：$s(t)=fx(a)w(t-1)da=(x*w)(t)$,离散：$s(t)=\sum_{a=-\infty}^\infty$
* 二维卷积：$S(i,j)=(K*I)(i,j)=\sum_m\sum_nI(i-m,j-n)K(m,n)$

## 9.2 动机

* 卷积运算的三个思想：稀疏交互，参数共享，等变表示

## 9.3 池化

* 不管采用什么样的池化函数，当输入做出少量平移实，池化能够帮助输入的表示近似不变
* 使用池化可以看做是增加了一个无限强的先验：这一层学的函数必须对少量平移的不变性

## 9.4 卷积与池化作为一种无限强的先验

* 我们可以把卷积的使用当作是对网络中一层的参数引入了一种无限强的先验概率分布，这个先验说明了该层应该学得的参数只包含了局部连接关系并且对平移具有等变型。
* 使用池化也是一种无限强的先验：每一个单元都具有对少量平移的不变性。

## 9.5 基本卷积函数的变体

## 9.6 结构化输出

* 卷积神经网络可以用于输出高维的结构化对象，而不仅仅是预测分类任务的类标签或回归任务的实数值。

## 9.8 高效的卷积算法

* 卷积等效于使用傅里叶变换将输入和核都转到频域、执行两个信号的逐点相乘，再使用傅里叶逆变换转换回时域。
* 当一个d位可以表示成d个向量的外积时候，该核被称为可分离的，当核可分离时候，用朴素的卷积是低效的。

## 9.9 随机或无监督的特征

* 减少卷积网络训练成本的一种方式是使用那些不是由监督方式训练得到的特征。
* 由三种基本策略可以不容过监督训练得到卷积核。
  * 随机初始化
  * 手动设计
  * 使用无监督的标准来学习核。

# 第十章 序列建模：循环和递归网络

## 10.1 展开计算图

![](https://gitee.com/weifagan/MyPic/raw/master/img/RNN4.PNG)

$h^{t}=f(h^{t-1},x^t,\theta)$

## 10.2 循环神经网络

RNN结构图：

![](https://gitee.com/weifagan/MyPic/raw/master/img/RNN2.PNG)

RNN设计模式包括以下几种：

* 每个时间步都有输出，并且隐藏单元之间有循环连接的循环网络

* 每个时间步都产生一个输出，只有当前时刻的输出到下一个时刻的隐藏单元之间有循环连接的循环网络
* 隐藏单元之间存在循环连接，但读取整个序列后产生单个输出的循环网络。

前向传播更新公式：

$a^{t}=b+Wh^{t-1}+Ux$

$h^{t}=tanh(a^{(t)})$

$o^{t}=c+Wh^{t}$

$y^t=softmax(o^t)$

### 10.6 递归神经网络

递归神经网络代表循环网络的另一种扩展，被构造为神的树状结构而不是RNN的链状结构，如下图所示：

![](https://gitee.com/weifagan/MyPic/raw/master/img/tigui.PNG)

递归网络的明显优势是对于具有相同长度$\tau$的序列，深度可以急剧地从$\tau$减少为$O(log\tau)$,有助于解决长期依赖问题。

## 10.7 长期依赖的挑战

* 长期依赖的根本问题是，经过许多阶段传播后梯度倾向于消失或爆炸，即使假设循环网络的参数是稳定的，但是长期依赖的困难来自比短期相互作用的指数小的权重。

* $h^{(t)}=W^Th^{t-1}=Q^T\Lambda^tQh^{0}$，特征值提升到t次后，导致赋值不到一的特征值衰减到0，大于一的就会暴增。

## 10.10 长短期记忆和其他门控RNN

### 10.10.1 LSTM

LSTM的“细胞“框图如下所示：

![](https://gitee.com/weifagan/MyPic/raw/master/img/LSTM9.PNG)

## 10.11 优化长期依赖

### 10.11.1 截断梯度

* 目标函数可能存在“悬崖”的“地形”，这种情况下的解决方案是截断梯度
  * 参数更新前，逐个元素地截断小批量产生的参数梯度
  * 在参数更新前截断梯度g的范数：$g\leftarrow \frac{g^v}{||g||}$

### 10.11.2 引导信息流的正则化

* 梯度截断有助于处理爆炸的梯度，但它无助于消失的梯度。
* 为了解决梯度问题并更好地捕获长期依赖，进行正则化或约束参数，以引导“信息流”