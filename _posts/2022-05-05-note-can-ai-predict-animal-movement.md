---
title: 论文笔记：Can AI predicts animal movements?
tags: 论文 强化学习 笔记
---

> 原文：Can AI predict animal movements? filling gaps in animal trajectories using inverse reinforcement learning

## 摘要

使用逆强化学习（Inverse Reinforce Leaning aka. IRL）框架，作者开发了一种新的间隙填充方法来预测动物最有可能行进的路线；再此框架内，算法从动物轨迹中学习奖励函数，以找到动物偏好的环境特征。作者将这种方法应用到了从「白额鸌」[bái é hù]（Streaked Shearwater）获得的 GPS 轨迹之上（下按语言习惯称为剪嘴鸥），并以此作为 IRL 相对于以往使用的插值方法更优秀的证据。其提出的优点如下：

1. 不需要关于控制运动的参数分布的假设；
2. 不需要动物关于地形的偏好和限制的假设；
3. 可以填补巨大的时空空白。

作者称这项工作展示了 IRL 如何增强填补动物轨迹空白以及在异质空间中构建 reward-space map 的能力。这个方法可以帮助用于针对具有生态或进化意义的现象的动作研究，比如栖息地的选择和迁移。

## 序言

除了对摘要信息的补充、过往方法的总结之外，在结尾部分作者表示利用了剪嘴鸥 GPS 数据的 80% 作为训练集（其实也不能称作训练集，强化学习往往不会像传统机器学习一样严谨的划分训练集和测试集），学习了一个奖励函数。并在剩余的 20% 的数据之中人为的制造了一些间隙供 IRL 算法预测，并在下文给出了对比的结果。

## 方法

这里概括的介绍了数据的获取来源以及获取时的一些问题，由于本人的研究方向不涉及实际的采样和数据获取，所以这部分暂略。

## 数据处理

- 数据选择：本文使用的数据包含了 106 条轨迹数据（53 条雄性轨迹以及 53 条雌性轨迹），这些轨迹数据为了保证训练和测试的性能，不含有大量缺失的部分。
- 环境选择：本文将纬度（37°300 N – 45°300 N）和经度（136°300 E – 148°300 E）分别转换为等间隔的 200 和 300 单元格。每个单元格对应一个边长大约 3 公里宽的正方形。由于 IRL 方法的局限性，精度的提升将带来巨大的计算成本。
- 数据形式：易知空间数据为 $(x, y)$ 单元格坐标，时间数据为了简化计算，将时间戳转化为从轨迹开始之后移动的次数（大于等于 1 的自增整型）。这是一个离散的时间步长，平均大约有 15 分钟。虽然选择的轨迹只含有小的缺失部分，但是这些缺失部分会形成连续的孤立点，为了解决这个问题，依然是用传统的线性插值的方法填补了这些缺失。最终的数据形式为：$(x, y, t)$。

## 强化学习的状态和动作

在本文所使用的强化学习模型下：状态 $s_t=(x_t,y_t,z_t)$ ，其中 $x_t$ 和 $y_t$ 是离散的空间坐标，$z_t$ 是离散的时间步长。虽然在某些类似的问题上（如行人预测），模型更接近于一种最短路径问题，从而在状态中不需要考虑经过时间 $z_t$ （智能体每次只考虑如何最短的时间内到达目标，从而无需再考虑到达目前状态时经过了多少时间），而在本文的任务中，由于剪嘴鸥（甚至所有的动物）在迁移过程中可能会采取间接路线（觅食、探险或其他原因），从而使问题异于简单的最短路径问题，所以在状态中添加了 $z_t$。有了这个状态空间定义，无论奖励值如何，代理都需要重复动作选择 $z_t$ 次才能达到目标，并且可以考虑间接路线。

除了状态 $s_t$ 之外，强化学习还要考虑每一步的动作 $a_t$，对于空间的转移，$a_{xy}\in\\{东, 南, 西, 北\\}$ ，而时间仅根据状态转换每次加一即可，所以 $a_t=(a_{xy}, 1)_t$ 。那么一个轨迹 $\zeta$ 可以定义为：$\zeta=\\{(s_0,a_0),(s_1,a_1),\ ...\\}$。一个轨迹的集合 $Z$ 则是：$Z=\\{\zeta_1,\zeta_2,...,\zeta_n\\}$。

为了基于 RL 框架来填补轨迹，本文定义一条缺失轨迹的始点为初始状态，结点为终止状态。此时智能体根据策略重复采取行动 $a$ ，并获得奖励值 $r$。$r$ 的累计 $R$ 表示行为的质量。RL 算法通过获得的累计奖励大小来选择最优的策略。显然，最优的策略取决于奖励函数的定义。然而，对于生态学实验，奖励函数的定义十分困难。因此，本文使用了 IRL 来从已有数据中学习奖励函数。

## 最大熵逆强化学习（Max Entropy Inverse Reinforcement Learning）

> 逆强化学习大体分为两个步骤，第一个步骤，通过已有的数据（或者说轨迹）学习奖励函数。奖励函数 $r(s;\theta)$ 是一个关于 $s$ 的函数，它的权重是 $\theta$，逆强化学习会将奖励函数分解为权重向量和特征向量的点积。特征向量是从状态 $s$ 中来的，它起到一个量化当前状态的作用。第一个步骤的学习，其实就是指学习 $\theta$ 向量，通过让智能体一次次的实验，不断地调整 $\theta$ 以匹配 已有的轨迹。第二个步骤就是传统强化学习的任务，利用已有的奖励函数让智能体在新的轨迹中实现预测功能。

本文使用的模型为逆强化学习（下称 MaxEnt IRL），它的贝尔曼方程如下所示：

$$Q(s,a)=r(s;\theta)+E_{p(s'|s,a)}[V(s')],$$

$$V(s)=softmax_aQ(s,a)=maxQ(s,a)+\log\left[1+\exp\{min\ Q(s,a)-max\ Q(s,a)\}\right].$$

（在强化学习中）Q 函数定义的是在某一个状态采取某一个动作，有可能得到的回报的一个期望。而状态价值函数 $V(s)$ 定义的是在某一个状态下有可能得到的回报的期望。在普通的强化学习中，Q 和 V 的贝尔曼方程如下：

$$Q(s,a)=r(s,a)+E_{p(s'|s,a)}[V(s')],$$

$$V(s)=max(Q(s,a)).$$

其中 Q 函数由即时回报 $r(s,a)$ 以及未来的累积回报期望组成。在逆强化学习中，奖励函数 r 被以下规则拆分：$r(s_t)=\theta^Tf(s_t)$。其中 $\theta$ 是权重向量， $f(s_t)$ 是状态 $s_t$ 的特征向量。$f(s_t)$ 可以从 $s_t$ 中推导而来，而 $\theta$ 则是逆强化学习推导奖励函数期间需要学习的权重。 综上所述，在 $\theta$ 确定的情况下，整条轨迹 $\zeta$ 的奖励为：

$$R(\zeta;\theta)=\sum_tr(s_t;\theta)=\sum_t\theta^Tf(s_t).$$

本文中一共使用了 12 张 feature map，每一张都量化了他们所处的物理环境，它们由以下类型组成：

1. 用于指示陆地和海洋的二进制图；
2. 与海洋、陆地和海岸线的指数距离图；
3. constant 常量图（空白）。

![]({{ site.url }}\assets\images\2022-05-05\feature_map.svg)

对于随机给定的策略 $\pi(a\|s;\theta)$，累积奖励的期望可以表示为：

$$E_{p_{\pi}(\zeta)}[R(\zeta;\theta)]=E_{p_{\pi}(\zeta)}[\sum_t\theta^Tf(s_t)]=\theta^TE_{p_{\pi}(\zeta)}[\sum_tf(s_t)].$$

MaxEnt IRL 的目的就在于找到一个能匹配剪嘴鸥行为的奖励函数。更正式的说法是，假定 $\overline \pi$ 是目前未知的，剪嘴鸥在轨迹 Z 中采取的策略，那么 MaxEnt IRL 的目的就是找到这个策略。这意味着 IRL 最终得到的特征向量的期望和观察到的剪嘴鸥的特征向量的均值相吻合，也就是：

$$E_{p_{\pi}(\zeta)}[\sum_tf(s_t)]=\frac{1}{\left|Z\right|}\sum_{\zeta \in Z}[\sum_tf(s_t)]=\overline f.$$

但是这个式子是 ill posed 的，是不可求的，所以作者选择用最大熵方法求出能让信息熵最大的策略 $\pi$。这里对应最大熵方法，限制条件有：

1. $\int_{\zeta}p_{\pi}(\zeta)d\zeta=1,\ p_{\pi}(\zeta)>0,$
2. $E_{p_{\pi}(\zeta)}[\sum_t f(s_t)]=\overline f.$（不可求不妨碍他可以当限制条件。）

最终得出的概率分布如下所示：（这里原文也没有给出过程，显然这是 (Jaynes 1957, Berger et al. 1996) 得出的一个结果，后面还得研究一下这篇论文。）

$$p_{\pi}(\zeta|\theta)=\frac{\exp(\sum_t \theta^Tf(s_t))}{Z(\theta)}=\frac{\exp(\sum_t \theta^Tf(s_t))}{\int_\zeta \exp(\sum_t \theta^Tf(s_t'))d\zeta'}.$$

易知 $Z(\theta)=\int_\zeta \exp(\sum_t \theta^Tf(s_t'))d\zeta'.$ 它是最大熵模型中的一个中间函数，和本文中的路径集合 $Z$ 不是一个东西。

而参数 $\theta$ 是最大化对数似然来估计的（$\hat\theta$ 代表最优的 $\theta$）：

$$\hat \theta=argmax_\theta L(\theta)=argmax_{\theta}\frac{1}{\left|Z\right|}\sum_{\zeta \in Z}\log p_{\pi}(\zeta|\theta).$$

具体的方法是通过指数梯度法迭代求 $\theta$：

$$\theta=\theta e^{\lambda \nabla L(\theta)}.$$ 

其中，

$$\nabla L(\theta)=\frac{1}{\left|Z\right|}\sum_{\zeta \in Z}\log p_{\pi}(\zeta|\theta)=\overline f-E_{p_{\pi}(\zeta'|\theta)}[\sum_t f(s_t')].$$

（推导过程见原论文）

其中，式 (11) 的第二个项是特征向量在 $\pi$ 策略下的期望，此时 $\theta$ 是常量，则期望仅仅是 $\zeta$ 的期望，则其可以写成：

$$E_{p_{\pi}(\zeta'|\theta)}[\sum_t f(s_t)]=\frac{1}{\left|Z\right|}\sum_{\zeta \in Z}\sum_tf(s_t)D_\zeta(s_t).$$

> *$D_\zeta(s_t)$ is an expected state visitation count that expresses the probability of being in a certain state $s_t$ of trajectory $\zeta$ and is efﬁciently computed by backward and forward passes (Ziebart et al. 2008, Kitani et al. 2012)*. ——原文

这里 $D$ 函数是一个统计状态访问次数的函数，通过前向传播和反向传播直至收敛，即可得到最优的 $\pi(a\|s;\theta)$ 。

在前向传播中，$D$ 的传播公式为：

$$D_\zeta^{k+1}(s)=\sum_{s',a}D_\zeta^k(s')\pi(a|s;\theta)p(s'|s,a).$$

收敛后，$D_\zeta(s)$ 的值是整个迭代的过程值的总和：$D_\zeta(s)=\sum_kD_\zeta^k(s).$ 通过不断地迭代 $D$，再更新 $\theta$ 就可以得到最优的 $\hat \theta$。

## 填补的完整过程

给定一个缺失的部分，首先设置这个轨迹的始点和终点为 $s_0$ 和 $s_g$。然后再通过反向传播得到的 $\hat \theta$ 估计最优策略 $\pi(a\|s;\theta)$。反向传播通过 $s_g$ 计算 $Q(s,a)$ 和 $V(s)$。然后再用 $Q$ 和 $V$ 计算随机策略 $\pi(a\|s;\theta)$。我们对于填补的轨迹的目标，是每次选择 $argmax_a\pi(a\|s;\theta)$ 的状态 $s'$。从 $s_0$ 到 $s_g$ 都保持这个策略来选择动作。要注意的是通过特定策略生成的轨迹在$s_0$ 和 $s_g$ 相同的情况下应该是完全一样的；但是，可以通过估计的策略来进行概率插值（probabilistically interpolate trajectories）。

## 评估模型

为了评估 IRL 方法，本文使用了留出法（Holdout method）保留了一部分雄性雌性数据。正如前文所讲，使用了 80% 的数据估计权重向量，剩下的用于评估模型。具体的评估方法是，在测试轨迹的前半部分和后半部分随机选择两个点。选择的点被设置为轨迹缺失部分的开始和结束。为了确认这个插值方法对大缺失部分的效果，仅使用了沿整个轨迹至少缺失 50% 的样本。

对于定量评估，本文将 IRL 的插值方法和生态学中非常普遍且广泛使用的线性插值方法进行了比较。具体使用的方法是镜柜改进的 Hausdorff 距离 (MHD; Dubuisson and Jain 1994)。这个方法后期会更新博文，暂略。

评估的结果简要来说就是远远胜于已有的方法，详见原文。

## 引用

1. [T. Hirakawa et al., "Can AI predict animal movements? filling gaps in animal trajectories using inverse reinforcement learning", *Ecosphere*, vol. 9, no. 10, 2018.](https://esajournals.onlinelibrary.wiley.com/doi/10.1002/ecs2.2447)





