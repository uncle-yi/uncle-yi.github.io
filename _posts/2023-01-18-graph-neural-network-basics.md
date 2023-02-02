---
title: 图神经网络基础
tags: 图神经网络 GNN 机器学习 记录
---
## 图数据结构

任何图都由若干个节点（vertices or nodes）以及若干个边（edges）组成，所以图可以被存储为节点集合和边集合。由于图的边可以是有向的也可以是无向的，所以图也分为有向图和无向图；根据图的边是否有权值，图也可以分为有权图和无权图。当图的边只有权值（weights）这一个信息时，权值就代表边存储的全部信息，当图的边承载的信息更复杂时，一般要想办法将其转化为向量的形式。

<!--more-->

```mermaid
flowchart TB

subgraph s1 ["无向 & 有权图"]
R1(( ))
R2(( ))
R3(( ))
R4(( ))
R5(( ))

R1--5---R2
R1--7---R3
R2--12---R3
R3--4---R4
R3--3---R5
end

subgraph s2 ["无向 & 无权图"]
L1(( ))
L2(( ))
L3(( ))
L4(( ))
L5(( ))

L1---L2
L1---L3
L2---L3
L3---L4
L3---L5
end

class s1,s2 noback;
classDef noback fill:#fff0, stroke:#fff0;
```

```mermaid
flowchart TB

subgraph s1 ["有向 & 有权图<"]
R1(( ))
R2(( ))
R3(( ))
R4(( ))
R5(( ))

R1--5-->R2
R1--7-->R3
R2--12-->R3
R3--4-->R4
R3--3-->R5
end

subgraph s2 ["有向 & 无权图"]
L1(( ))
L2(( ))
L3(( ))
L4(( ))
L5(( ))

L1-->L2
L1-->L3
L2-->L3
L3-->L4
L3-->L5
end

class s1,s2 noback;
classDef noback fill:#fff0, stroke:#fff0;
```

> 一个全连接图意味着如果是无向图则图中所有的节点之间都有一个边连接。如果是有向图，则总是存在一个边，这个边的两头为任意节点。

## 图的数学表示

```mermaid
flowchart TB

subgraph s1 [" "]
A2(("A"))
B2(("B"))
C2(("C"))
D2(("D"))
E2(("E"))
F2(("F"))
A2--12---B2
A2--10---C2
C2--15---E2
C2--1---F2
D2--7---F2
B2--8---D2
E2--12---F2
end

subgraph s2 [" "]
A1(("A"))
B1(("B"))
C1(("C"))
D1(("D"))
E1(("E"))
F1(("F"))
A1---B1
A1---C1
C1---E1
C1---F1
D1---F1
B1---D1
E1---F1
end

class s1,s2 noback;
classDef noback fill:#fff0, stroke:#fff0;
```

|  无向 & 无权图<br/>（左图，Un-Weighted & Undirected Graph）  |   无向 & 有权图<br/>（右图，Weighted & Undirected Graph）    |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| {::nomarkdown}<table>  <tr>    <th>节点的集合</th>    <th>边的集合</th>    </tr>  <tr>    <td>V={A,B,C,D,E,F}</td>  <td>E={(A,B),(A,C),(B,D),(C,E),(C,F),(D,F),(E,F)}</td>  </tr>  </table>{:/} | {::nomarkdown}<table>  <tr>    <th>节点的集合</th>    <th>边的集合</th>  </tr>  <tr>    <td>V={A,B,C,D,E,F}</td>  <td>E={(A,B,12), (A,C,10), (B,D,8), (C,E,15), (C,F,1), (D,F,7), (E,F,12)}</td>  </tr>  </table>{:/} |

邻居（Neighbors）：有边相连的两个节点互为邻居。例：B 和 C 是 A 的所有邻居。

### 边列表（Edge list）

```mermaid
flowchart TB

subgraph s1 [" "]
A2(("A"));
B2(("B"));
C2(("C"));
D2(("D"));
E2(("E"));
F2(("F"));
A2--12-->B2;
A2--10-->C2;
C2--15-->E2;
D2--7-->F2;
C2--1-->F2;
B2--8-->D2;
E2--12-->F2;
E2--23-->A2;
B2--34-->A2;
end

subgraph s2 [" "]
A1(("A"));
B1(("B"));
C1(("C"));
D1(("D"));
E1(("E"));
F1(("F"));
A1-->B1;
A1-->C1;
C1-->E1;
D1-->F1;
C1-->F1;
B1-->D1;
E1-->F1;
E1-->A1;
B1-->A1;
end

class s1,s2 noback;
classDef noback fill:#fff0, stroke:#fff0;
```

|      有向 & 无权图<br>（左图，Directed & Un-weighted）       |        有向 & 有权图<br/>（右图，Directed & Weighed）        |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| {::nomarkdown}<table>  <tr>    <th>Node1</th>    <th>Node2</th>    </tr>  <tr>  <td>A</td>  <td>B</td>  </tr>  <tr>  <td>A</td>  <td>C</td>  </tr>  <tr>  <td>B</td>  <td>D</td>  </tr>  <tr>  <td>C</td>  <td>E</td>  </tr>  <tr>  <td>C</td>  <td>F</td>  </tr>  <tr>  <td>D</td>  <td>F</td>  </tr>  <tr>  <td>E</td>  <td>F</td>  </tr>  <tr>  <td>E</td>  <td>A</td>  </tr>  <tr>  <td>B</td>  <td>A</td>  </tr>  </table>{:/} | {::nomarkdown}<table>  <tr>    <th>Node1</th>    <th>Node2</th>    <th>Weight</th>    </tr>  <tr>  <td>A</td>  <td>B</td>  <td>12</td>  </tr>  <tr>  <td>A</td>  <td>C</td>  <td>10</td>  </tr>  <tr>  <td>B</td>  <td>D</td>  <td>8</td>  </tr>  <tr>  <td>C</td>  <td>E</td>  <td>15</td>  </tr>  <tr>  <td>C</td>  <td>F</td>  <td>1</td>  </tr>  <tr>  <td>D</td>  <td>F</td>  <td>7</td>  </tr>  <tr>  <td>E</td>  <td>F</td>  <td>12</td>  </tr>  <tr>  <td>E</td>  <td>A</td>  <td>23</td>  </tr>  <tr>  <td>B</td>  <td>A</td>  <td>34</td>  </tr>  </table>{:/} |

### 邻接矩阵（Adjacency matrix）

- 邻接矩阵是一个 2D 正方形矩阵；
- 每一个节点都占据两个维度的一个刻度；
- 无权图则矩阵值一般为 True/False 或 1/0；
- 权图则为权值，无权边可以为 -1。

一般表示为：$A=N\times N$

```mermaid
flowchart TB

subgraph s1 [" "]
A1(("A"))
B1(("B"))
C1(("C"))
D1(("D"))
E1(("E"))
F1(("F"))
A1-->B1
A1-->C1
C1-->E1
D1-->F1
C1-->F1
B1-->D1
E1-->F1
E1-->A1
B1-->A1
end

subgraph s2 [" "]
A2(("A"))
B2(("B"))
C2(("C"))
D2(("D"))
E2(("E"))
F2(("F"))
A2--12---B2
A2--10---C2
C2--15---E2
C2--1---F2
D2--7---F2
B2--8---D2
E2--12---F2
end

class s1,s2 noback;
classDef noback fill:#fff0, stroke:#fff0;
```

|                    无向 & 有权图（左图）                     |                    有向 & 无权图（右图）                     |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| {::nomarkdown}<table><tdead><tr><td></td><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td></tr></tdead><tbody><tr><td>A</td><td>-1</td><td>12</td><td>10</td><td>-1</td><td>-1</td><td>-1</td></tr><tr><td>B</td><td>-1</td><td>-1</td><td>-1</td><td>8</td><td>-1</td><td>-1</td></tr><tr><td>C</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td><td>15</td><td>1</td></tr><tr><td>D</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td><td>7</td></tr><tr><td>E</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td><td>12</td></tr><tr><td>F</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td></tr></tbody></table>{:/} | {::nomarkdown}<table><tdead><tr><td></td><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td></tr></tdead><tbody><tr><td>A</td><td>F</td><td>T</td><td>T</td><td>F</td><td>F</td><td>F</td></tr><tr><td>B</td><td>T</td><td>F</td><td>F</td><td>T</td><td>F</td><td>F</td></tr><tr><td>C</td><td>F</td><td>F</td><td>F</td><td>F</td><td>T</td><td>T</td></tr><tr><td>D</td><td>F</td><td>F</td><td>F</td><td>F</td><td>F</td><td>T</td></tr><tr><td>E</td><td>T</td><td>F</td><td>F</td><td>F</td><td>F</td><td>T</td></tr><tr><td>F</td><td>F</td><td>F</td><td>F</td><td>F</td><td>F</td><td>F</td></tr></tbody></table>{:/} |

> 严格意义上无向图的邻接矩阵应该是对称矩阵，但是有些时候为了方便存储或是其他原因，可能会表示为三角矩阵。

```mermaid
flowchart TB

subgraph s1 [" "]
A1(("A"))
B1(("B"))
C1(("C"))
D1(("D"))
E1(("E"))
F1(("F"))
A1--12-->B1
A1--10-->C1
C1--15-->E1
D1--7-->F1
C1--1-->F1
B1--8-->D1
E1--12-->F1
E1--23-->A1
B1--34-->A1
end

subgraph s2 [" "]
A2(("A"))
B2(("B"))
C2(("C"))
D2(("D"))
E2(("E"))
F2(("F"))
A2---B2
A2---C2
C2---E2
C2---F2
D2---F2
B2---D2
E2---F2
end

class s1,s2 noback;
classDef noback fill:#fff0, stroke:#fff0;
```

| 无向 & 无权图（左图）                                        | 有向 & 有权图（右图）                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| {::nomarkdown}<table><tdead><tr><td></td><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td></tr></tdead><tbody><tr><td>A</td><td>F</td><td>T</td><td>T</td><td>F</td><td>F</td><td>F</td></tr><tr><td>B</td><td>F</td><td>F</td><td>F</td><td>T</td><td>F</td><td>F</td></tr><tr><td>C</td><td>F</td><td>F</td><td>F</td><td>F</td><td>T</td><td>T</td></tr><tr><td>D</td><td>F</td><td>F</td><td>F</td><td>F</td><td>F</td><td>T</td></tr><tr><td>E</td><td>F</td><td>F</td><td>F</td><td>F</td><td>F</td><td>T</td></tr><tr><td>F</td><td>F</td><td>F</td><td>F</td><td>F</td><td>F</td><td>F</td></tr></tbody></table>{:/} | {::nomarkdown}<table><tdead><tr><td></td><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td></tr></tdead><tbody><tr><td>A</td><td>-1</td><td>12</td><td>10</td><td>-1</td><td>-1</td><td>-1</td></tr><tr><td>B</td><td>34</td><td>-1</td><td>-1</td><td>8</td><td>-1</td><td>-1</td></tr><tr><td>C</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td><td>15</td><td>1</td></tr><tr><td>D</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td><td>7</td></tr><tr><td>E</td><td>23</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td><td>12</td></tr><tr><td>F</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td><td>-1</td></tr></tbody></table>{:/} |

### 关联矩阵（Incidence matrix）

```mermaid
flowchart TB

subgraph s2 [" "]
A2(("A"))
B2(("B"))
C2(("C"))
D2(("D"))
E2(("E"))
F2(("F"))
A2-->B2
A2-->C2
C2-->E2
C2-->F2
D2-->F2
B2-->D2
E2-->F2
end

subgraph s1 [" "]
A1(("A"))
B1(("B"))
C1(("C"))
D1(("D"))
E1(("E"))
F1(("F"))
A1---B1
A1---C1
C1---E1
C1---F1
D1---F1
B1---D1
E1---F1
end

class s1,s2 noback;
classDef noback fill:#fff0, stroke:#fff0;
```

| 无向图（左图）                                               | 有向图（右图）                                               |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| {::nomarkdown}<table><tdead><tr><td></td><td>e1</td><td>e2</td><td>e3</td><td>e4</td><td>e5</td><td>e6</td><td>e7</td></tr></tdead><tbody><tr><td>A</td><td>1</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr><tr><td>B</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td></tr><tr><td>C</td><td>0</td><td>1</td><td>1</td><td>0</td><td>1</td><td>0</td><td>0</td></tr><tr><td>D</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>1</td></tr><tr><td>E</td><td>0</td><td>0</td><td>1</td><td>1</td><td>0</td><td>0</td><td>0</td></tr><tr><td>F</td><td>0</td><td>0</td><td>0</td><td>1</td><td>1</td><td>0</td><td>1</td></tr></tbody></table>{:/} | {::nomarkdown}<table><tdead><tr><td></td><td>e1</td><td>e2</td><td>e3</td><td>e4</td><td>e5</td><td>e6</td><td>e7</td></tr></tdead><tbody><tr><td>A</td><td>-1</td><td>-1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr><tr><td>B</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>-1</td><td>0</td></tr><tr><td>C</td><td>0</td><td>1</td><td>-1</td><td>0</td><td>-1</td><td>0</td><td>0</td></tr><tr><td>D</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>-1</td></tr><tr><td>E</td><td>0</td><td>0</td><td>1</td><td>-1</td><td>0</td><td>0</td><td>0</td></tr><tr><td>F</td><td>0</td><td>0</td><td>0</td><td>1</td><td>1</td><td>0</td><td>1</td></tr></tbody></table>{:/} |
| 如果节点和边相连，则值为 1，否则为 0。节点由行表示，而边相反。 | 如果节点和边相连，根据边的方向，值取 -1/1，不相连则取 0。节点由行表示，而边相反。 |

### 度矩阵（Degree Matrix）

如果不区分入度和出度的话，任意有向图的度矩阵和将它的边的方向删掉得到的无向图的度矩阵是一样的。

```mermaid
flowchart LR
A(("A"))
B(("B"))
C(("C"))
D(("D"))
E(("E"))
F(("F"))
A---B
A---C
C---E
C---F
D---F
B---D
E---F
B---B
F---F
```

> {::nomarkdown}<table><tdead><tr><td></td><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td></tr></tdead><tbody><tr><td>A</td><td>2</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr><tr><td>B</td><td>0</td><td>4</td><td>0</td><td>0</td><td>0</td><td>0</td></tr><tr><td>C</td><td>0</td><td>0</td><td>3</td><td>0</td><td>0</td><td>0</td></tr><tr><td>D</td><td>0</td><td>0</td><td>0</td><td>2</td><td>0</td><td>0</td></tr><tr><td>E</td><td>0</td><td>0</td><td>0</td><td>0</td><td>2</td><td>0</td></tr><tr><td>F</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>5</td></tr></tbody></table>{:/}

> 度矩阵是一个对角矩阵。

### 拉普拉斯矩阵（Laplacian Matrix）

- 测量矩阵的平滑性；
- $L=Degree Matrix - Adjacency Matrix$
- $L=\{D-A\}$

```mermaid
flowchart LR
A(("A"))
B(("B"))
C(("C"))
D(("D"))
E(("E"))
F(("F"))
A---B
A---C
C---E
C---F
D---F
B---D
E---F
B---B
F---F
```

> | $D\ {\rm(Degree Matrix)}$                                    | $A\ {\rm(Adjacency Matrix)}$                                 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| {::nomarkdown}<table><tdead><tr><td></td><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td></tr></tdead><tbody><tr><td>A</td><td>2</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr><tr><td>B</td><td>0</td><td>4</td><td>0</td><td>0</td><td>0</td><td>0</td></tr><tr><td>C</td><td>0</td><td>0</td><td>3</td><td>0</td><td>0</td><td>0</td></tr><tr><td>D</td><td>0</td><td>0</td><td>0</td><td>2</td><td>0</td><td>0</td></tr><tr><td>E</td><td>0</td><td>0</td><td>0</td><td>0</td><td>2</td><td>0</td></tr><tr><td>F</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>5</td></tr></tbody></table>{:/} | {::nomarkdown}<table><tdead><tr><td></td><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td></tr></tdead><tbody><tr><td>A</td><td>0</td><td>1</td><td>1</td><td>0</td><td>0</td><td>0</td></tr><tr><td>B</td><td>1</td><td>2</td><td>0</td><td>1</td><td>0</td><td>0</td></tr><tr><td>C</td><td>1</td><td>0</td><td>0</td><td>0</td><td>1</td><td>1</td></tr><tr><td>D</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>1</td></tr><tr><td>E</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>1</td></tr><tr><td>F</td><td>0</td><td>0</td><td>1</td><td>1</td><td>1</td><td>2</td></tr></tbody></table>{:/} |

$L=\{D-A\}:$

> {::nomarkdown}<table><tdead><tr><td></td><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td></tr></tdead><tbody><tr><td>A</td><td>2</td><td>-1</td><td>-1</td><td>0</td><td>0</td><td>0</td></tr><tr><td>B</td><td>-1</td><td>2</td><td>0</td><td>-1</td><td>0</td><td>0</td></tr><tr><td>C</td><td>-1</td><td>0</td><td>3</td><td>0</td><td>-1</td><td>-1</td></tr><tr><td>D</td><td>0</td><td>-1</td><td>0</td><td>2</td><td>0</td><td>-1</td></tr><tr><td>E</td><td>0</td><td>0</td><td>-1</td><td>0</td><td>2</td><td>-1</td></tr><tr><td>F</td><td>0</td><td>0</td><td>-1</td><td>-1</td><td>-1</td><td>3</td></tr></tbody></table>{:/}

## 图神经网络（Graph Neural Network or GNN）

- 一种可以对图数据结构应用的神经网络；
- GNN 通常可以提供以下单位上的分类/预测任务：
  - 节点级别
  - 边级别
  - 图级别
- 主要分类：
  - Recurrent GNN
  - Spatial Convolutional Network
  - Spectral Convolutional Network
- GNN 能够帮助我们：
  - 分类节点
  - 预测 边/连接
  - 分类图

### GNN 数据模式（Batch vs Single Mode）

**Single Mode:** 包含大量节点的单个图

**Batch Mode:** 包含一个或多个节点的图集合