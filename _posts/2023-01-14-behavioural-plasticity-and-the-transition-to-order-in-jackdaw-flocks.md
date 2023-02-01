---
title: 论文笔记：Behavioural plasticity and the transition to order in jackdaw flocks
tags: 
- Nature Communication 
- NC 
- 论文 
- 生物学 
- 笔记
---

> **「原文」**Behavioural plasticity and the transition to order in jackdaw flocks
> **「作者」**Hangjian Ling, Guillam E. Mclvor, Joseph Westley, Kasper van der Vaart, Richard T. Vaughan, Alex Thornton & Nicholas T. Ouellette

## 摘要

集体行为通常被认为是个体遵守特定规则的总和。本文的主要结论就是寒鸦在不同 context（环境、密度等外界条件）的情况下会改变交互策略（在欧氏空间或拓扑空间上）。随着集群密度增加，鸟群呈现了从无序到有序的急剧转变。这篇文章在方法上使用了 self-propelled particle model，这也将是我后续阅读的重点。

<!--more-->

## 简介

内容比较杂，大部分都没有太大意义，这里只摘取了对阅读文章比较有帮助的几点：

- 最近研究人员开始观察到，诸如大小、密度和极化等群体属性会随着外部条件的变化而变化，群体成员之间的异质性会影响群体动力学。
- 生物是否具有在 metric interactions 和 topological interactions 之间切换交互模型的能力还尚且未知。
- 多个模型表明随着鸟群密度增大，集体行为会从无序渐渐转移到有序行为。
- 结论是寒鸦确实会因为环境的不同而改变交互策略以及集体特征。

## 结论

### 1. Local interactions in jackdaw flocks.

文章分析了在两个生态环境的情况下，野生寒鸦鸟群的交互规则的变化。分别是：

1. Transit flocks：冬季傍晚，寒鸦形成大型迁徙鸟群返回栖息地（6次迁徙，集群中个体数量从 25 到 330 不等）；
   > ![]({{ site.url }}\assets\images\2023-01-14\ezgif-3-f2e7912c36.gif){:height="300px" .border}
   >
   > 图 1（原文 S. Movie 1）：其中一次迁徙追踪数据（S. 指 Supplementary，下文不再做注解）

2. Mobbing flocks：模拟「scolding call」（反捕食者召集呼喊）后的反捕食者的聚集过程（10次模拟，集群中个体数量从 4 到 120 不等）。

   > ![]({{ site.url }}\assets\images\2023-01-14\ezgif-1-5a3ded60c8.gif){:height="300px" .border} 
   >
   > 图 2（原文 S. Movie 2）：其中一次聚集追踪数据

   Mobbing flocks 的数据是首先组装了一个狐狸标本叼着类似寒鸦的鸟，然后让假狐狸突然出现的同时播放寒鸦的「scolding call」音频，从而迫使寒鸦形成聚集行为而得到的。

   > ![]({{ site.url }}\assets\images\2023-01-14\Snipaste_2023-01-14_12-59-42.png){:height="240px" .border} 
   >
   > 图 3（原文图 1a，1b）：迁徙数据和聚集数据示例

据文章所说，他们发现 transit flocks 的交互是 topological-based，而 mobbing flocks 是 metric-based（topological：拓扑距离；metric：度量距离）。由于鸟集群的互动一般都认为最终以 aligning 为结果，所以本文划分 topological 和 metric 的方法就是将对齐角度设为 topological rank $n$ 和欧氏距离 $r$ 的函数 $\theta=g(n,\ r)$，再分别查看 $n$ 和 $r$ 不变时，函数的相关性确定 $\theta$ 由 $n$， $r$ 中的哪一个确定。

对于 mobbing flocks，统计了在 topological rank 不变的情况下，对齐角度和欧氏距离的变化曲线（图 4i.）可以看到多个 topological rank 值对应的曲线几乎重叠，且角度都随距离平滑上升，曲线角度明显。这证明在 mobbing flocks 中 interaction 更倾向于 metric-based。有争议的点在于，观察集群模拟数据的轨迹，普遍认知可能是鸟群会围绕捕食者做圆周运动环绕飞行，这样即使没有交互，随着物理规律，距离大的个体就是会更不倾向于对齐。但是作者对 mobbing 的俯视角数据（图 4iii.）做了 probability density function 统计（图 4ii.），显示出鸟其实更倾向于通过捕食者的正上方点来回飞行，这就与“围绕捕食者做圆周运动”的假设相违背了。

> ![]({{ site.url }}\assets\images\2023-01-14\Snipaste_2023-01-14_15-12-26.png){:height="240px" .border} 
>
> 图 4（原文图 1c，S.1c，S.1b 重排）：Mobbing flocks 的 metric based interaction 证明

Transit flocks，相较于 mobbing flocks，$\theta$ 函数的曲线就没有反映出明显的上升趋势（图 4i.），这就说明起码相对于另一种 context 下的交互，起码迁徙中的寒鸦鸟群的交互是不那么基于欧氏距离的。不过在迁徙数据中，能得到较显著的速度波动（velocity fluctuation，图 5，「这似乎是流体力学的研究范畴，文中也没解释是怎么计算到的波动」）。作者倾向于相信这些速度波动是个体之间的 interaction 导致的。而这个数据反映的强对齐（strong alignment，总速度方向几乎全体一致）的结果则是「外界因素 + 个体交互」的结果。

> ![]({{ site.url }}\assets\images\2023-01-14\Snipaste_2023-01-14_16-22-59.png){:height="300px" .border} 
>
> 图 5（原文图 S.2）：Transit flocks 跟踪数据中的速度波动

那么「外界因素 + 个体交互」的重叠效应毫无疑问是难以分析的，所以后续将角度聚焦在了个体与其 neighbors 的关系上。接下来，对个体的空间分布上的各向异性（anisotropy，文中的解释是研究聚焦个体相邻的「邻居」的空间分布是否具有随机性）做了分析。介于 transit flocks 和 mobbing flocks 都具有和邻居 aligning 的现象，显然局部的 anistropy（如果真的存在的话）更倾向于受 local interaction 影响而非外部条件。为了衡量 anistropy，定义了一个 anistropy factor $y=f(n,\ r)$ 来衡量聚焦个体的邻居的分布。因子 $y$ 越大，邻居个体相对于聚焦个体的位置就越不随机。先前的一些研究已经表明 $y$ 随着邻居与聚焦个体的距离增加而减小，且定义了当 $y$ 缩小到其各向同性值（isotropic，代表此时的分布为完全随机），则此时对应的距离为个体的 interaction range。

在 mobbing flocks 的数据上，$y$ 随着 $r$ 增大而骤降，且不同 $n$ 的曲线依然几乎重合（图 6d），代表 $y$ 基本上是 $r$ 的函数。而 transit flocks 则是 $n$ 比 $r$ 更影响 $y$ 的趋势，代表 $y$ 的值更取决于 $n$ 而不是 $r$。据作者所说，**这个结果基本可以断定，在 mobbing ➔ transit 的过程中，寒鸦的 interaction 会从 metric-based 转移到 topological-based**。 这也是这个文章的核心结论。文章得到的 mobbing flocks 的交互距离大概是 5 m，大约 14 个身位，而 transit flocks 的交互邻居数量大概是 7-8 个。

> ![]({{ site.url }}\assets\images\2023-01-14\Snipaste_2023-01-14_18-46-14.png){:height="300px" .border} 
>
> 图 6（原文图 1d，1e）：Mobbing flocks 和 transit flocks 的 anistropy 因子和欧氏距离 $r$ 的曲线。

这个部分最后还有一段最后的基于 transit flocks 基于 topological rank 的佐证。但是我没有拓扑距离的背景知识，所以此处省略，未来可能会补充。

### 2. Relationship between group density and group order.

这个部分就是计算了 mobbing flocks 数据中子集群的有序性（这里的 order 指的不是顺序，是有序性），通过比对不同密度下鸟群速度方向的一致性，可以得到密度和有序性之间的关系。结果是集群的数量越多，有序性显著提高（图 7）。这里的有序参数 $\phi_t$ 就是计算了 $t$ 时刻所有个体的速度方向对应单位向量的平均（反应方向的重合度，也就是有序性）。

> ![]({{ site.url }}\assets\images\2023-01-14\Snipaste_2023-01-14_19-34-00.png){:height="300px" .border} 
>
> 图 7（原文图 2）：3个代表性集群的飞行轨迹以及它们有序参数 $\phi_t$ 随时间变化的曲线。

并且观察到了有序参数 $\phi$（$\phi_t$ 的平均）和密度参数 $\rho$ 的 0.37 次方呈线性相关（图 8），和 Vicsek 的 self-propelled particle model 是一致的。这代表 mobbing flock 有自组织性且有序参数 $\phi$ 是从 local interaction 中逐渐涌现的属性。但是 transit flocks 并没有呈现出这种规律，可能是因为 topological interactions 和外部条件重叠作用的结果。

> ![]({{ site.url }}\assets\images\2023-01-14\Snipaste_2023-01-14_19-37-09.png){:height="300px" .border} 
>
> 图 8（原文图 3）：有序参数 $\phi$ 和密度参数 $\rho^{0.37}$ 的曲线

Transit flocks 的 topological interactions 对于保持整体的一致性和反应性都有相当高效的效果，而 mobbing flocks 的 metric interactions 则可能是为了让个体更能集中注意力到捕食者而不是追踪同类身上。

### 3. Self-propelled particle model.

这里声明的 Self-propelled particle model 就是参照了 Vicsek 早期的工作。在文中使用的模型，粒子都做速度大小为 $U_0$ 的匀速运动，并且方向会和它固定距离内所有个体的速度向量的平均对齐。作者也设置了一个互斥区/距离（repulsion zone），避免粒子结成一个团。这里比较重要的是他设置的速度规则，每个粒子一帧的速度遵循以下公式：

$$v_i(t+\Delta t)=U_0·\Omega_\eta\{\Theta[\sum_{j\in S_i}{\rm v}_j(t)-\beta {\rm x}_i(t)]\}$$

- $\sum_{j\in S_i}{\rm v}_j(t)$：$S_i$ 是围绕第 $i$ 个粒子的半径为 $r_0$ 的球体，这个累加是计算 $t$ 时刻球体内除 $i$ 之外粒子的速度总和；
- $\beta {\rm x}_i(t)$：是一个谐和力，减去它会将粒子 $i$ 往他的原位置拉回（为什么要有这个我还暂不清楚）；
- $\Theta$：是归一化算子，将向量缩放到单位向量；
- $\Omega_\eta$：是噪声算子，按照某个均匀分布旋转向量一定角度；
- $U_0$：每个粒子的速度大小，是常数。

使用这个 Self-propelled particle model 的原因在于，观察到了 Mobbing flocks 的有序参数 $\phi$ 和密度 $\rho^{0.37}$ 的曲线在密度到达接近 0 的时候依然保持了这个趋势（图 8）。文中意思是这个叫做 critical density，也就是这种聚集模式转变时的极限密度。而使用这个粒子模型，得到了几乎一致的结果。这就意味着 order-density 的这种规则就是 interaction 所导致的。为了测试这个有序性是 metric-based local interaction 导致的而非捕食者的出现而造成的，将粒子模型的交互模式更改为 topological-based 的规则。最终得到了不同的结果（密度和有序性无关），这就说明有序性的转变和捕食者没有关系。

## 展望

- 群体的动态机制和形态学即使在相同物种中都会因为环境和外部条件的变化而变化。生物会为了最大化收益，调整和邻居个体的交互规则。
- 未来的研究在建模和预测新的动物运动时，应该考虑环境和外部条件，必须预先假设动物的感官和认知机制的可塑性。

## 引用

1. [Behavioural plasticity and the transition to order in jackdaw flocks - Nature Communications](https://www.nature.com/articles/s41467-019-13281-4)
