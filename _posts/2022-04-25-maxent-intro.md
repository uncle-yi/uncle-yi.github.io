---
title: 最大熵模型概述——纽约出租车问题
tags: 数学 笔记
---

## 前言

最大熵方法的主要作用场景在于通过仅有的一部分数据 $K$ 来预测远远大于它的实际可能数据量 $N$。

$$ K\ll N $$

> 最大熵原理是一种选择随机变量统计特性最符合客观情况的准则，也称为最大信息原理。随机量的概率分布是很难测定的，一般只能测得其各种均值（如数学期望、方差等）或已知某些限定条件下的值（如峰值、取值个数等），符合测得这些值的分布可有多种、以至无穷多种，通常，其中有一种分布的熵最大。选用这种具有最大熵的分布作为该随机变量的分布，是一种有效的处理方法和准则。——百度百科

## 纽约出租车问题

问题：一个市民需要多长时间才能在纽约市叫到出租车？

针对这个问题，我们现在有一组观测 $T$（例：$T = \{6,3,4,6,2,3,2,6,4,4\}$），那么一种最直观的方法求 $p(x)$（需要 $x$ 分钟的可能性），就是求 $x$ 在 $T$ 中的频率（例：$p(6) = 0.3$）。那么这个模型或者说方法的幼稚之处在于整个系统有无限多种情况（甚至是只考虑离散的情况下也有许多可能性），而这些情况单纯只是未被观测到，模型的输出却认为这些可能性为 $0$，所以实际应用中是不可能这么做的。

那么这里为了不使用类似的天真模型，有人定义了一种方法——最大熵模型。它的目的是求出一个概率分布 $p$（我们把它称作 $P_{MaxEnt}$ 或 $P_{ME}$），它：

1. 满足我们已知的一些约束；
2. 在所有满足这些约束的概率分布中拥有最大的熵（信息熵）。

在这之中，约束总是以**期望**的形式出现（比如上述的平均等待时间），可以用如下公式表示：

$$\lt x\gt=\int_0^\infty P(x)x\ dx,$$

如果可离散化的话 $\lt x\gt=\sum _{x=0}^\infty P(x)x.$ 

对于类似的分布，如果需要讨论的话，还有：

$$\lt x^2\gt=\int_0^\infty p(x)x^2\ dx,$$

$$\lt f(x)\gt=\int_0^\infty p(x)f(x)\ dx.$$

那么回到出租车等待时间，这里我们需要限制等待时间的概率分布 $P_{ME}(x)$ 的期望值 $\lt x\gt_{P_{ME}} = 4\ min$ ，这个过程可以被称作：constraint step。

下一步，可以称为 entropy maximization step，选择熵最大的分布。信息熵的公式如下：

$$H(p)=-\sum_0^\infty P_{ME}(x)\ log\ P_{ME}(x).$$

它的意义仅在于，除了已知的几个均值类的限制条件之外，保持对于整个系统的最大不确定性。可以看出这个方法得到的分布是相当主观的。

## 最大熵模型

这里我所引用的 YouTube 视频使用了一种类似梯度下降法迭代求极值的方法演示了一遍如何求最大熵。那么对于普遍的情况，计算最值的指导公式为：

$$\frac{\partial f}{\partial \vec x}=\sum_{i=1}^N \lambda_i \frac{\partial g_i}{\partial \vec x}.$$

其中 $\lambda$ 被称为拉格朗日乘数（Lagrange multiplier）。

回到之前的出租车等待时间的例子，那么假设现在有两个限制条件（4 min 以及 100%），设 $S$ 为 分布 $p$ 的信息熵，$g_i$ 为 $p$ 的第 $i$ 个限制条件，那么这个公式可以写为：

$$\frac{\partial S}{\partial p_i}=\lambda_1 \frac{\partial g_1}{\partial p_i}+\lambda_2 \frac{\partial g_2}{\partial p_i}.$$

其中：$$\left\{
	\begin{array}{**lr**}
	g_1(\vec p)=\sum_{i=0}^\infty p_ii=4\ min, &\\
	g_2(\vec p)=\sum_{i=0}^\infty p_i=1.
	\end{array}
\right.$$根据信息熵公式：

$$S=-\sum_{i=0}^\infty p_ilog\ p_i,$$

$$\frac{\partial S}{\partial p_i}=-(log\ p_i+1).$$

求导很容易得到：$$\left\{
    \begin{array}{**lr**}
    \frac{\partial g_{1}}{\partial p_{i}}=i, &\\
    \frac{\partial g_{2}}{\partial p_{i}}=1.
	\end{array}
\right.$$则将等式 (7) 代入等式 (9) 可得等式 (10)：

$$-log\ p_i-1=\lambda_1 i+\lambda_2.$$

可以得到结果：$p_i=e^{-1-\lambda_1i-\lambda_2}$。设 $Z=e^{1+\lambda_2}$，则 $p_i=\frac{e^{-\lambda_1i}}{Z}$。接下来为了求解 $p$，就需要得到对应的 $\lambda_1$ 和 $Z$。根据 $g_2=1$ 可得等式 (11)：

$$Z=\sum_{i=0}^{\infty}e^{-\lambda_1i}.$$

则现在只要知道 $\lambda_1$ 就可以求出 $p_i$ ，再根据 $g_1= 4$ 可得等式 (12)：

$$\frac{1}{Z}\sum_{i=0}^\infty ie^{-\lambda_1i}=4.$$

那么这里我引用的 YouTube 视频略微有一些问题，因为他对等式 (11) 使用了几何级数的求和公式（$i\rightarrow \infty$）（要注意这里的 $i$ 指代的是无穷的概率分布 $p_i$ 的 $i$，而不是限制个数的 $i$），又对求和公式求极限，得到等式 $Z=\frac{1}{1-e^{-\lambda_1}}$。之所以有一些问题，是因为这里默认了 $\lambda_1\gt0$，所以这里要么有一些不严谨，要么有额外的原因能确保它确实大于 0。

对于等式 (11) 可以用几何级数公式求解，但是等式 (12) 成分比较复杂，但是这里若对 (11) 两侧求偏导数，可得 $\frac{\partial Z}{\partial \lambda_1}=-\sum_{i=0}^\infty ie^{-\lambda_1i}$ ，则 (12) 可改写为：

$$-\frac{1}{Z}\frac{\partial Z}{\partial \lambda_1}=4.$$

为了让 (11) 和 (13) 联系起来，再对 $Z$ 的分式形式两边求偏导数可得：

$$\frac{\partial Z}{\partial \lambda_1}=\frac{e^{-\lambda_1}}{(1-e^{-\lambda_1})^2}.$$

将 (14) 带入 (13) 可得：

$$-\frac{1}{Z}\frac{\partial Z}{\partial \lambda_1}=\frac{e^{-\lambda_1}}{(1-e^{-\lambda_1})}=4.$$

接下来只要计算等式 (15)，即可得 $\lambda_1$。这里我使用了 Numpy 的 log 函数求解得 $\lambda_1=0.22314355131420976$。代入 $p(x)=\frac{e^{-\lambda_1x}}{Z}$ ，最终结果为：$p(x) = \frac{(0.8)^x}{5}$。

求出的分布如下所示：

```python
import numpy as np
import matplotlib.pyplot as plt

p = 0.8 ** np.arange(0,60)/5

f = plt.figure(figsize=(8,4))
plt.grid('on')
plt.plot(range(60),p)
plt.savefig("taxi_q.svg")
plt.show()
```

![]({{ site.url }}\assets\images\2022-04-25\taxi_q.svg)

验证一下限制条件：

```python
print(f"验证分布的总和为：{np.sum(p)}（应接近1）\n验证分布的期望为：{np.sum(p*np.arange(60))}（应接近4）")
```

输出为：

```
验证分布的总和为：0.9999984675044594（应接近1）
验证分布的期望为：3.999901920285387（应接近4）
```

## 引用

1. [Maximum Entropy Methods Tutorial - YouTube](https://www.youtube.com/watch?v=6YEn9QRy3ks&list=PLF0b3ThojznT3olRuplp5x41wUp_LZxHL&index=1)
2. [最大熵原理_百度百科 (baidu.com)](https://baike.baidu.com/item/最大熵原理/9938383)