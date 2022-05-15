---
title: DTW（Dynamic Time Warping）算法实现
tags: 实现 记录
---

## 前言

> In general, DTW (dynamic time warping) is a method that calculates an optimal match between two given sequences (e.g. time series) with certain restriction and rules:
>
> - Every index from the first sequence must be matched with one or more indices from the other sequence, and vice versa
> - The first index from the first sequence must be matched with the first index from the other sequence (but it does not have to be its only match)
> - The last index from the first sequence must be matched with the last index from the other sequence (but it does not have to be its only match)
> - The mapping of the indices from the first sequence to indices from the other sequence must be monotonically increasing, and vice versa, i.e. if j > i are indices from the first sequence, then there must not be two indices l > k in the other sequence, such that index i is matched with index l and index j is matched with index k and vice versa.
>
> ——Wikipedia

可以理解 DTW 为一个判断不同长的序列之间相似度的测量。DTW 的具体过程，可以查看这个视频：[DTW（动态时间规整）算法原理与应用_bilibili](https://www.bilibili.com/video/BV12r4y1A7mT?share_source=copy_web) ，应该是用中文解释的最好的视频了。这篇博客主要还是贴一下我的一个实现，以及整个过程中的一些困惑。

## 实现

```python
import numpy as np
import scipy.spatial # 主要是用他的距离公式；
import matplotlib.pyplot as plt # 绘图；

class DTW(object):
    def __init__(self, query, template, dist_metric='euclidean', cal=False): 
        if (type(query) != np.ndarray) or (type(template) != np.ndarray):
            raise TypeError
        # query和template是对比的两个轨迹；
        self.__query = np.atleast_2d(query).reshape(-1,1)
        self.__template = np.atleast_2d(template).reshape(-1,1)
        # dx和dy分别是cost matrix的两个维度大小；
        self.__d_x = max(self.__query.shape)
        self.__d_y = max(self.__template.shape)
        # dmap是cost matrix，distmap是距离矩阵，利用scipy计算后不需要循环计算距离，
        # 每步都变成取值操作。
        self.__d_map = np.empty(shape=(self.__d_x, self.__d_y))
        self.__dist_map = None
        # pathx和pathy就是最短路径
        self.__path_x = None
        self.__path_y = None
        # 每calculate一次，更新一次结果；
        self.__calculated = False
        # distmetric代表使用的距离，默认为欧氏距离
        self.__dist_metric = dist_metric
        
        if cal == True:
            self.calculate()
    
    def calculate(self):
        # calculate distance matrix. 利用scipy的距离函数计算距离矩阵；
        self.__dist_map = scipy.spatial.distance.cdist(self.__query, 
                                                       self.__template, 
                                                       metric=self.__dist_metric)
        # calculate the accumulated cost matrix. 再利用距离矩阵计算cost matrix；
        self.__d_map[0][0] = self.__dist_map[0][0]
        for i in range(1, self.__d_x):
            self.__d_map[i][0] = self.__dist_map[i][0] + \
                                 self.__d_map[i - 1][0]
        
        for i in range(1, self.__d_y):
            self.__d_map[0][i] = self.__dist_map[0][i] + \
                                 self.__d_map[0][i - 1]
            
        for i in range(1, self.__d_x):
            for j in range(1, self.__d_y):
                self.__d_map[i][j] = self.__dist_map[i][j] + min(self.__d_map[i - 1][j],
                                                                 self.__d_map[i][j - 1],
                                                                 self.__d_map[i - 1, j - 1])
        # calculate the path. 计算最短路径；
        temp_map = self.__d_map.copy() # 这里有一些疑惑，我目前看到的资料都没有说明
        temp_map[0, 1:] = np.inf       # 将cost matrix的第一列第一行除00以外的位置
        temp_map[1:, 0] = np.inf       # 都设为无穷大，但是两个已有的python包都是这么做的。而且没有这一步的话
        curr_x, curr_y = self.__d_x - 2, self.__d_y - 2   # 非等长序列可能找不到最短路径。
        path_x = np.array([curr_x])
        path_y = np.array([curr_y])

        while(curr_x > 0 or curr_y > 0):
            temp_lst = [temp_map[curr_x + 1][curr_y],
                        temp_map[curr_x][curr_y + 1],
                        temp_map[curr_x][curr_y]]
            
            i = np.argmin(temp_lst)
            if i == 0:
                curr_x, curr_y = curr_x, curr_y - 1
            elif i == 1:
                curr_x, curr_y = curr_x - 1, curr_y
            elif i == 2:
                curr_x, curr_y = curr_x - 1, curr_y - 1

            path_x = np.append(path_x, curr_x)
            path_y = np.append(path_y, curr_y)
        
        self.__path_x = path_x
        self.__path_y = path_y
        self.__calculated = True
        del temp_lst
        del temp_map
    
    def plot(self, size=(5, 5), hspace=0.7, wspace=0.7): # 绘图函数
        f = plt.figure(figsize=size)
        grid = plt.GridSpec(4, 4, hspace=hspace, wspace=wspace)

        main_ax = f.add_subplot(grid[:-1, 1:])
        main_ax.grid('on')

        y_hist = f.add_subplot(grid[:-1, 0], xticklabels=[], sharey=main_ax)
        x_hist = f.add_subplot(grid[-1, 1:], yticklabels=[], sharex=main_ax)
        
        y_hist.grid('oh')
        y_hist.set_ylabel("Template Sequence")
        x_hist.grid('oh')
        x_hist.set_xlabel("Query Sequence")
        
        main_ax.plot(self.__path_x, self.__path_y, color='red')
        y_hist.plot(self.__template, range(len(self.__template)), color='orange')
        x_hist.plot(self.__query, color='orange')
        if save:
            plt.savefig("plot.svg")
        plt.show()
    # 下面就是一些property函数；
    @property
    def query(self):
        return self.__query
    
    @query.setter
    def query(self, n_query):
        if (type(query) != np.ndarray):
            raise TypeError("query must be a numpy array.")
        n_query = np.atleast_2d(query).reshape(-1,1)
        self.__query = n_query
            
    @property
    def template(self):
        return self.__query
    
    @template.setter
    def template(self, n_template):
        if (type(template) != np.ndarray):
            raise TypeError("template must be a numpy array.")
        n_template = np.atleast_2d(template).reshape(-1,1)
        self.__template = n_template
    
    @property
    def dist_metric(self):
        return self.__dist_metric
    
    @dist_metric.setter
    def dist_metric(self, metric):
        if not isinstance(metric, str):
            raise ValueError("metric must be a string.")
        self.__dist_metric = metric
    
    @property
    def path(self):
        if self.__calculated:
            return path_x, path_y
        else:
            raise NotImplementedError("DTW not calculated, please run DTW.calculate() first.")
            
    @property
    def d_map(self):
        if self.__calculated:
            return self.__d_map
        else:
            raise NotImplementedError("DTW not calculated, please run DTW.calculate() first.")
    
    @property
    def min_distance(self):
        if self.__calculated:
            return self.__d_map[-1][-1]
        else:
            raise NotImplementedError("DTW not calculated, please run DTW.calculate() first.")
```

## 疑惑

正如注释所说我没有找到将 cost matrix 的第一列第一行除 $[0][0]$ 以外的位置都设为无穷大的理论根据是什么。这部分后面还需要研究，但是就实现看来目前没有什么问题。

## 测试

输入：（Jupyter Notebook 环境）

```python
idx = np.linspace(0,6.28,num=100)
query = np.sin(idx) + np.random.uniform(size=100)/10.0
template = np.append(np.cos(idx), np.sin(idx))

dtw = DTW(query, template, cal=True)
dtw.plot()
```

输出：

![]({{ site.url }}\assets\images\2022-05-09\plot.svg)

## 引用

[Dynamic time warping - Wikipedia](https://en.wikipedia.org/wiki/Dynamic_time_warping)

[DTW（动态时间规整）算法原理与应用_bilibili](https://www.bilibili.com/video/BV12r4y1A7mT?share_source=copy_web)