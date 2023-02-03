---
title: 论文笔记：Efficient collective swimming by harnessing vortices through deep reinforcement learning
tags: 
- PNAS 
- 论文 
- 生物学 
- 笔记
- 强化学习
---

> **「原文」**Efficient collective swimming by harnessing vortices through deep reinforcement learning<br/>
> **「作者」**Siddhartha Verma, Guido Novati, and Petros Koumoutsakos
>
> 注：该文章有较多的流体力学内容，这些部分对我的研究暂时没有太大用处，所以下文会省略大量内容，注意审阅。

## 摘要

作者通过深度强化学习算法建模了鱼的游泳模型，通过不同模型的比对表明了鱼类可以通过将自己放置在其他游泳者尾流中的适当位置提高自身的推进效率。

<!--more-->

> ![]({{ site.url }}\assets\images\2023-02-03\ezgif-5-0c3c6310fa.gif){:height="120px" .border} ![]({{ site.url }}\assets\images\2023-02-03\ezgif-5-73bf296214.gif){:height="120px" .border} ![]({{ site.url }}\assets\images\2023-02-03\ezgif-5-49ec5fe688.gif){:height="120px" .border}
>
> 图 1（原文 S. Movie 1, 2, 3）：三种不同情况的仿真示例，透明流状物是仿真可视化的涡旋（S. 指 Supplementary，下文不再做注解）

利用强化学习训练的“smart swimmer”在仿真环境中模拟的结果表明，以跟随为目标的行为序列不会给鱼带来能量效益，但是通过在 leader 的周围，通过摆动身体对其产生的涡流作对应的同步，则能稳定的节省能量。

## 简介

> 一些对鱼群聚集原因探索相关的研究，此处省略。

本文的主要工作是建立了四组实验组，其中涉及两种个体：Interactive Swimmer 和 Solitary Swimmers。

1. $IS_\eta$：能在领导鱼周围学习到最高效游行效率的智能体；
2. $IS_d$：只以减少和领导鱼的 lateral deviation（横向偏差，$\Delta y$）为目标的智能体；
3. $SS_\eta$：复制 $IS_\eta$ 动作序列的对照组；
4. $SS_d$：复制 $IS_\eta$ 动作序列的对照组。

> ![]({{ site.url }}\assets\images\2023-02-03\Snipaste_2023-02-03_15-20-14.png){:height="300px" .border}
>
> 图 2（原文图 2B 加标注）文中所说的横向和纵向的具体方向

## 结果

### Efficient Autonomous Swimmers

$IS_\eta$ 的奖励函数就是游行效率，而 $IS_d$ 的奖励函数是 $R_d = 1 − \lvert\Delta y\rvert/L$ （$L$ 是鱼的长度）。后者收敛后，符合预期的，它和领导鱼的纵向偏差 $\Delta x$ 维持在了 $2.2L$ 左右。令人惊讶的是，$IS_\eta$ 的结果也是分别在 $\Delta x=1.5L$ 以及 $\Delta x=2.2L$ 的距离上保持稳定（图 3E），即使它不会因为相对位置而获得任何的奖励。这两个点也和实际上的全局最优的两个峰值十分接近。两个距离相差的 $0.7L$ 也和领导鱼造成的涡旋之间的距离吻合。从训练数据来看，$IS_\eta$ 在游行过程中的摆动幅度也比 $IS_d$ 明显要小（图 2F）。这也很好理解，因为后者的奖励函数对游行效率不敏感，事实证明它的能量损失也严重大于前者。这说明盲目的无策略的跟随领导鱼反而会降低游行的效率。

> ![]({{ site.url }}\assets\images\2023-02-03\Snipaste_2023-02-03_15-56-18.png){:height="300px" .border}
>
> 图 3（原文图 2）学习游泳策略的结果

### Intercepting Vortices for Efficient Swimming

和对照组的两个 $SS$ 作比较，$IS_\eta$ 的游行效率要大大优于 $SS_\eta$，而它们之间的差距只有一个稳定的领导鱼造成的稳定涡流。而 $IS_\eta$ 和 $SS_\eta$ 又都优于 $IS_d$ 和 $SS_d$。

后续内容就是效率的实际分析了，涉及过多流体力学的内容，请有需求的读者自行阅读原文。

## 引用

1. [Efficient collective swimming by harnessing vortices through deep reinforcement learning - PNAS](https://www.pnas.org/doi/suppl/10.1073/pnas.1800923115)

