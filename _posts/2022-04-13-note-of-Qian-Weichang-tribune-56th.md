---
title: 钱伟长讲坛第56讲：从脑科学到类脑人工智能讲座笔记
tags: 笔记
---
## 脑科学目前发展的「一体两翼」模型
```mermaid
graph TB

S1(("脑疾病诊治"));
S2["临床和社区队列数据<br />和样本库"];
S3["认知相关重大脑疾病<br />早期诊断与干预"];
S4["脑健康和医疗产业"];
S1 --> S2; S1 --> S3; S2 --> S4; S3 --> S4;

L1(("脑功能<br />的神经基础"));
L2["脑智发育研究"];
L3["认知功能神经环路研究"];
L4["脑研究创新技术平台"];
L5["介观全脑神经连接图谱"];
L1 --> L2; L1 --> L3; L1 --> L4;
L2 --> L5; L3 --> L5; L4 --> L5;
L1 --> S1; S1 --> L1;

style L1 fill:#FFC0CB; 
style L2 fill:#FFC0CB;
style L3 fill:#FFC0CB; 
style L4 fill:#FFC0CB;
style L5 fill:#FFC0CB;

R1(("脑机智能技术"));
R2["类脑计算系统<br />类脑器件和智能体"];
R3["脑机接口<br />与脑调控技术"];
R4["类脑人工智能产业"];

style R1 fill:#ADD8E6;
style R2 fill:#ADD8E6;
style R3 fill:#ADD8E6;
style R4 fill:#ADD8E6;

R1 --> R2; R1 --> R3; R2 --> R4; R3 --> R4;
L1 --> R1; R1 --> L1;
```

<!--more-->

脑科学使用的各种模式动物：线虫 --> 斑马鱼 --> 果蝇 --> 小鼠 --> 猕猴 --> 人类

大脑结构：人类大脑之所以复杂，主要在于大脑皮层，其余的部分属于进化时的保守部分。

## 内容

### 大脑的神经网络

由近千亿神经细胞通过百万亿个突触联结组成神经网络，以特殊神经环路（回路）实现感知、运动、思维等功能。

### 可塑性是大脑最重要的特性

```mermaid
graph TB
L1["感觉/运动/认知行为相关的电活动"];
L2["神经元与突触功能和构造的修饰"];
L3["认知行为的改变"];
L1 --"学习（记忆形成）"-->L2;
L2 --> L3;
```

- 学习：神经网络处理和储存信息的过程；
- 记忆：储存在神经网络内可提取的信息。

### 赫伯学习法则

1. 同步的电活动可造成突触加强或稳固；
2. 不同步电活动可造成突触削弱或消失。

> 一起放电的神经元连接在一起。

- LTP-突触功能长期强化——>树突棘变大——>影响树突权重增强
- LTD-突触功能长期弱化——>树突棘变小——>影响树突权重减弱

### 人的聪明才智来自何处？

- 大脑的功能来自神经网络，而出生后的环境和经验引起的电活动是塑造网络的主要因素。
- 各种功能的网络形成在出生后都有一个关键期（如视觉系统1-3岁，语言系统2-7岁），关键期内电活动可以稳固或修剪神经连接，塑造出成熟的神经网络。
- 有限的网络可塑性在网络形成后还一直存在；这些可塑性也就是成熟大脑的记忆学习等各种认知功能的基础。
- 遗传基因是建立正常网络的必要条件，但形成不同网络（脑功能的基础）的因素来自后天不同经验。

### 图灵的「儿童机器」（child machine）

> 图灵：“与其尝试开发一个模拟成人大脑的算法程序，为什么不去模拟一个儿童的大脑？然后通过合适的教育过程，我们就可以获得大脑的功能。”

### 大脑认知的关键问题

信息捆绑问题（The binding problem）—各种信息如何组合为整体的感知？

**聚合模型**（convergence model）：感知=对各组合成分有反应的神经元的输出聚合到一个负责感知的神经元。

**同步放电模型**（correlated firing model）：感知=对各组合成分有反应的神经元的同步放电。

> 赫伯细胞群假说（Hebb's cell assembly hypothesis）
>
> - 同步电活动强化了细胞群之间突触连接 ——> 感知记忆的存储
> - 储存记忆的细胞群再次被启动 ——> 感知记忆的提取

### 「祖母」概念的形成和记忆

假说：

1. 代表概念各成分的神经元集群分布在各脑区；
2. 概念的形成：各集群间同步放电造成长程环路连接的强化；
3. 概念的提取：部分集群的放电引起整个概念集群的激活。

### 「新图灵测试」的建议

**语言与感知觉能力的整合**：要求机器人和一个真人各自操作一支机械手合作玩一个游戏，同时要求他对彼此的动作进行对话。与原版图灵测试一样，裁判需要区分机器人与真人。

**团队合作**：要求机器人参加一个团队，机器人需要能理解团队成员的喜好、分享信息、对不确定性的环境有对策（如参加打篮球或讨论会），裁判需要区分机器人与其他团队成员。