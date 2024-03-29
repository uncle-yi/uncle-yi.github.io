---
title: 论文笔记：Internal models direct dragonfly interception steering
tags: Nature 论文 生物学 笔记
---

> **「原文」**Internal models direct dragonfly interception steering<br/>
> **「作者」**Matteo Mischiati, Huai-Ti Lin, Paul Herold, Elliot Imler, Robert Olberg & Anthony Leonardo

## 摘要

目标：研究蜻蜓在捕捉猎物时，internal models 的作用会到什么程度。

通过同时跟踪蜻蜓头部以及身体在飞行过程中的位置以及方向，本文提供了蜻蜓的转向拦截被身体动力学的 forward and inverse models 和猎物运动的 models 驱动的证据。

蜻蜓头部的预测性旋转持续跟踪猎物的角位置。猎物追踪造成的头-身夹角似乎引导了蜻蜓系统性的旋转，以让身体和猎物的飞行轨迹相平行。所以，model 驱动的控制是转向拦截机动的基础，而视觉用于对猎物的意外行为做出反应。这些发现阐明了昆虫构造行为的计算复杂性。

<!--more-->

## 简介

在人类以及非人类灵长类动物上研究的工作已经证明了三种对 sensorimotor control 的有重要意义的 internal model：

1. physical models to predict properties of the world;
2. inverse models to generate the motor commands needed to attain desired sensory states;
3. forward models to predict the sensory consequences of self-movement.

现在普遍认为蜻蜓在捕食时的拦截策略是一种被称作为 parallel navigation（平行定位）的策略。在 parallel navigation 中，追捕者在保持方向不变的情况下缩小自身至猎物的距离向量的长度。当猎物产生的角速度旋转距离向量时，会引起追捕者的反作用力以保持距离向量方向。PN 的优势在于，它是时间最优的，计算简单，不需要身体或世界的 internal model。但是整个拦截动作涉及许多的因素，这意味着要么蜻蜓的反应经受过较强的训练，要么转向并非是纯粹的反射，其中依然有 internal model 的引导。

> ![]({{ site.url }}\assets\images\2022-07-10\Snipaste_2022-07-10_15-45-05.png){:height="300px" .border}
>
> 图 1：Parallel Navigation 示意图

## 题目 1 ：Reactive steering to prey angular velocity

### 数据

本文有两组数据集：

1. 蜻蜓拦截果蝇（猎物产生角速度影响距离向量方向）
2. 蜻蜓拦截人造猎物（constant-velocity 无加速度）

第一组数据集实验 PN 是否会在猎物扰动距离向量方向时出现，第二组数据集实验不扰动距离向量的情况下能多快实现 PN。

当距离向量及其导数呈完全负相关时（相关性=-1）进入 PN 状态。

### 结果

人造猎物的距离向量相关性的平均演化时间和果蝇类似，而且这些拦截都有接近 PN 后再偏离的特征。这些偏离一定是 Non-parallel-navigation 所造成的，因此可以确定蜻蜓的捕捉行为不是 100% 的 parallel-navigation。

> ![]({{ site.url }}\assets\images\2022-07-10\Snipaste_2022-07-10_22-10-39.png){:height="300px" .border}
>
> 图 2：距离向量的相关系数随时间变化的曲线

之后本文又去捕捉这些拦截发生时，果蝇和蜻蜓的较大转向发生的点，发现这些数据除了反应蜻蜓的视觉反应延迟之外，大概有 70% 的果蝇转向不导致蜻蜓的转向。且 75% 的蜻蜓转向也并无和果蝇转向的相关性。这样的结果和蜻蜓的捕捉行为是基于纯反应的结论矛盾。

> ![]({{ site.url }}\assets\images\2022-07-10\Snipaste_2022-07-10_22-11-21.png){:width="300px" .border}
>
> 图 3：某一次抓捕中蜻蜓和果蝇加速度变化的曲线

## 题目 2 ：Body orientation directs steering

接下来，本文开始研究那些和猎物角速度没有直接关联的蜻蜓转向事件。

虽然除了猎物角速度变化以外，蜻蜓转向也可能归因到猎物动作的一些其他特征，但是前者的转向并不一定要与猎物有关系，比如蜻蜓自己的身体方向也可能影响它飞行路径的结构。

同样地，蜻蜓眼睛的相对高敏感度的区域的朝向也会影响它对猎物动作的检测和反应。调整这些限制，要求蜻蜓依赖其头和身体的 internal model，它的作用可以呈现为反应性的或预测性的。为了验证这个 model 是否真能引导转向，我们设计了可以附着在蜻蜓头部以及身体上的逆反射标记（microscopic retroreflective markers）。它可以用来重建蜻蜓觅食时的头部以及身体方向。

> ![]({{ site.url }}\assets\images\2022-07-10\Snipaste_2022-07-10_22-12-38.png){:height="300px" .border}
>
> 图 4：重建的蜻蜓模型示意图

> ![]({{ site.url }}\assets\images\2022-07-10\Snipaste_2022-07-10_22-12-55.png){:height="300px" .border}
>
> 图 5：重建的某次蜻蜓抓捕的过程

为了验证蜻蜓的身体方向是否限制飞行路径，本文分析了这个方向在飞行过程中是如何变化的。匀速的人工猎物可以用来观察猎物无干扰（无加速度）动作的情况下蜻蜓身体方向的变化。本文观察到蜻蜓的系统性的旋转会让它的飞行路径与直线路径显著不同（直线路径是能最快缩短距离的飞行路径）。

在拦截飞行过程中，蜻蜓会调整它的方位角朝向来和身体的朝向对齐。在实验过程中，由于每只蜻蜓的初始位置是随机的，所以它们改变朝向去拦截猎物必然涉及一次转向。转向的目标是蜻蜓将自身身体朝向和猎物飞行方向相平行，并且蜻蜓在这个过程中会保持 30 度左右的仰角。这一种固定模式表明蜻蜓在捕捉猎物时的身体方向上有偏好。所以以此为基础，可以把捕获过程简化为在有各个方向的角度限制的情况下缩小与猎物的垂直距离的问题（这一组实验均是将蜻蜓放置在猎物下方的）。如图 6,7 所示，蜻蜓的初始角度随机，但捕捉完成时，方位角和仰角都会有一定的趋势。

> ![]({{ site.url }}\assets\images\2022-07-10\Snipaste_2022-07-10_22-15-41.png){:height="300px" .border}
>
> 图 6：若干个捕捉过程的方位角视角

> ![]({{ site.url }}\assets\images\2022-07-10\Snipaste_2022-07-10_22-16-40.png){:height="300px" .border}
>
> 图 7：若干个捕捉过程的仰角视角

当蜻蜓开始捕捉有速度变化的猎物时，它的飞行规律和捕捉匀速的猎物时并没有显著区别。这意味着针对猎物机动的视觉反应作为一种微调，被整合在正在发生的转向机制上。而后者的重点则是身体的对齐情况以及猎物的角度位置（angular position）。也就是说身体方向确实会限制飞行方向。

## 题目 3 ：Head Orientation tracks prey position

蜻蜓的身体旋转会导致猎物在视线内的移动，从而加大检测猎物角位置的难度。在实际中，蜻蜓自身旋转造成的角位置漂移可能远远大于猎物移动造成的角位置漂移。需要分辨猎物和蜻蜓造成的漂移是不使用固定转向策略（比如 PN）的代价。解决这个问题可以通过神经系统来调节并取消明显的运动，或是利用头部的移动来抵消位移。

为了知道蜻蜓是如何解决辨别漂移的问题的，本文使用了动作捕捉的系统来估计蜻蜓眼睛上出现的猎物图像漂移。首先检测了蜻蜓头部的方向，然后再将猎物图像在蜻蜓复眼上的角位置分离出来。

本文分别量化了三个潜在的猎物图像漂移来源：

1. rotation of the dragonfly’s body;
2. relative translation of the dragonfly and prey;
3. rotation of the dragonfly’s head relative to the body.

> ![]({{ site.url }}\assets\images\2022-07-10\Snipaste_2022-07-10_22-29-01.png){:.border}
>
> 图 8：猎物图像的漂移轨迹以及头部旋转校正的示意图

结果发现了猎物图像稳定的保持在蜻蜓的视觉焦点中（这种维持聚焦的行为在下文中被称为 foveation，为了突出 foveal——中心凹）。猎物图像在头部的漂移平均要比身体旋转造成的偏移少 50%，说明极大程度上它们都被中和掉了。综上所述，蜻蜓是利用头部的移动来抵消的漂移。

## 题目 4 ：Predictive foveation

补偿性的头部旋转可能是基于猎物图像，被反应性地驱动，也可能是基于和漂移相关的 internal model，被预测性地驱动。为了判断是哪种可能性，本文检查了「蜻蜓 foveation 扰动的累加」以及「头部旋转校正」之间的延迟。已知昆虫肌肉收缩大约需要 5 ms 来产生力，并且依靠视觉的反应性行为往往有更大的延迟。相反地，如果蜻蜓能预知 foveal drift，那么头部旋转就可以做到没有延迟，以此做到最优抵消，尤其是在捕捉匀速猎物时。最后处理得到的平均延迟是在 4 ms 左右，几乎等同于肌肉即刻收缩的延迟，这就意味着整个校正过程是预测性的机制。

> ![]({{ site.url }}\assets\images\2022-07-10\Snipaste_2022-07-09_21-45-53.png){:height="300px" .border}
>
> 图 9：「头部转向校正」信号和「身体旋转+猎物相对移动（造成的漂移)」信号之间的互相关系数随延迟变化的曲线

之后利用「头部旋转校正」信号和「身体旋转的漂移」信号相加得到的信号再和「猎物相对移动」信号做互相关，也得到了类似的结果。这意味着无论是身体旋转导致的漂移还是猎物相对移动造成的漂移都是被预测性的机制抵消掉的。

> ![]({{ site.url }}\assets\images\2022-07-10\Snipaste_2022-07-10_22-30-08.png){:height="300px" .border}
>
> 图 10：「头部旋转校正」+「身体旋转的漂移」的信号和「猎物相对移动」信号之间的互相关系数随延迟变化的曲线


这些观测可以表明，在拦截匀速猎物时，「头部旋转校正」完全是预测性的。而拦截变速猎物时（未符合预测的行为会发生的情况下），由于 foveation 依然能够保持，说明当意外的猎物运动发生时，蜻蜓头部依然能反应性的补偿产生的图像漂移。可以说蜻蜓头部的旋转校正机制无缝的结合了预测和反应控制。

## 引用

1. [Internal models direct dragonfly interception steering - Nature](https://www.nature.com/articles/nature14045)



















