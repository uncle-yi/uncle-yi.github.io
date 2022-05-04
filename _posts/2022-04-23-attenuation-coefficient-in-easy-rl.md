---
title: 关于 Easy RL 中的衰减系数
tags: 记录 强化学习
---
最近在学习强化学习，买了一本《Easy RL 强化学习教程》来读。书是好书，虽然第一版有一些勘误，不过内容综合了很多国内机器学习大鳄的教程，还是值得一读的。大家感兴趣的话可以先到他们的 GitHub Page 页面看一下电子版：[EasyRL (datawhalechina.github.io)](https://datawhalechina.github.io/easy-rl/#/) 。

写这篇博客主要是因为在书中实现 QLearning 的时候，定义了一个衰减系数 $\epsilon$，但是没有该系数衰减公式的说明，所以我写了个可视化方便理解它是如何衰减的。源代码如下：

```python
class Qlearning(object):
    def __init__(self, state_dim, action_dim, cfg):
        self.action_dim = action_dim 
        self.lr = cfg.lr  # 学习率
        self.gamma = cfg.gamma  
        self.epsilon = 0 
        self.sample_count = 0  
        self.epsilon_start = cfg.epsilon_start
        self.epsilon_end = cfg.epsilon_end
        self.epsilon_decay = cfg.epsilon_decay
        self.Q_table = defaultdict(lambda: np.zeros(action_dim)) 

    def choose_action(self, state):
        self.sample_count += 1
        self.epsilon = self.epsilon_end + \  # 这里就是衰减系数的更新公式
            (self.epsilon_start - self.epsilon_end) * \
            math.exp(-1. * self.sample_count / self.epsilon_decay)
        
        if np.random.uniform(0, 1) > self.epsilon:
            action = np.argmax(self.Q_table[ str(state) ])
        else:
            action = np.random.choice(self.action_dim) 
        return action
    
    def update(self, state, action, reward, next_state, done):
        Q_predict = self.Q_table[ str(state) ][action]
        if done:
            Q_target = reward
        else:
            Q_target = reward + self.gamma * \
                np.max(self.Q_table[ str(next_state) ])
        self.Q_table[ str(state) ][action] += self.lr * (Q_target - Q_predict)
```

手写公式的话应该是这样的：

$$\epsilon = \epsilon_{end}+(\epsilon_{start}-\epsilon_{end}) e^{-(n/decay)}$$

$\epsilon_{start}$ 和 $\epsilon_{end}$ 分别代表一开始的和结束时的 $\epsilon$ ，n 代表回合数，decay 则是衰减率。可以看出来在其他参数是常数的情况下 $\epsilon$ 是关于 n 的单调递减函数。关于它的递减速度，我写的可视化脚本如下：

```python
# explain how epsilon drops:
import numpy as np
import matplotlib.pyplot as plt

epsilon = 0
epsilon_start = 0.95 # e-greedy策略中初始epsilon
epsilon_end = 0.01 # e-greedy策略中的终止epsilon
epsilon_decay = 300 # e-greedy策略中epsilon的衰减率

epsilon_lst = []
for sample_count in range(2000):
    epsilon = epsilon_end + \
        (epsilon_start - epsilon_end) * \
        np.exp(-1. * sample_count / epsilon_decay)
    epsilon_lst.append(epsilon)
f = plt.figure(figsize=(8, 4))
plt.grid('on')
plt.xlabel("回合次数") # 已导入中文字体，请自行操作。
plt.plot(range(2000), epsilon_lst, label='epsilon')
plt.scatter([0], [epsilon_start], label='epsilon_start', c='green')
plt.plot(range(2000), [epsilon_end] * 2000, dashes=[5, 5], label='epsilon_end')
plt.yticks(np.append((np.array(range(1, 9, 2)) / 10), ([0.95, 0.01])))
plt.legend()
plt.savefig("epsilon_decay.svg")
plt.show()
```

输出：

![]({{ site.url }}\assets\images\2022-04-23\epsilon_decay.svg)

可以看到大约在第 1500 个回合开始 $\epsilon$ 就已经接近 0.01 了。
