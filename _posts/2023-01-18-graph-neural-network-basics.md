---
title: 图神经网络基础
tags: 图神经网络 GNN 机器学习 记录
---
## 图数据结构

任何图都由若干个节点（vertices or nodes）以及若干个边（edges）组成，所以图可以被存储为节点集合和边集合。由于图的边可以是有向的也可以是无向的，所以图也分为有向图和无向图；根据图的边是否有权值，图也可以分为有权图和无权图。当图的边只有权值（weights）这一个信息时，权值就代表边存储的全部信息，当图的边承载的信息更复杂时，一般要想办法将其转化为向量的形式。

<!--more-->

```mermaid
flowchart TB

subgraph Undirected & Weighted
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

subgraph Undirected & Unweighted
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
```

```mermaid
flowchart TB

subgraph Directed & Weighted
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

subgraph Directed & Unweighted
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
```

> 一个全连接图意味着如果是无向图则图中所有的节点之间都有一个边连接。如果是有向图，则总是存在一个边，这个边的两头为任意节点。

## 图的数学表示

<table>
<tr><th>无向 & 无权图<br/>（Un-Weighted & Undirected Graph）</th><th>无向 & 有权图<br/>（Weighted & Undirected Graph）</th></tr>
<tr><td>

| 节点的集合                                    | 边的集合                                                     | 边的集合（另一种格式）                                       |
| --------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| $V=\{$<br/>$A, B, $<br/>$C, D, $<br/>$E, F\}$ | $E=\{$<br/>$AB, AC,$ <br/>$BD, CE,$ <br/>$CF, DF,$ <br/>$EF\}$ | $E=\{$<br/>$(A,B),(A,C),$ <br/>$(B,D),(C,E),$ <br/>$(C,F),(D,F),$ <br/>$(E,F)\}$ |

</td><td>

|b|1|2|3| 
|--|--|--|--|
|a|s|d|f|

</td></tr> </table>

|      |      |
| ---- | ---- |

```mermaid
graph TB

subgraph Weighted & Undirected
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

subgraph Un-Weighted & Undirected 
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
```

| 节点的集合：<br>$V=\{A, B, C, D, E, F\}$                     | 节点的集合：<br>$V=\{A, B, C, D, E, F\}$                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **边的集合：**<br/>$E=\{AB, AC, BD, CE, CF, DF, EF\}$<br/>$E=\{(A,B), (A,C), (B,D), (C,E),$<br/> $(C,F), (D,F), (E,F)\}$ | **边的集合：**<br/>$E=\{AB, AC, BD, CE, CF, DF, EF\}$<br/>$E=\{(A,B,12), (A,C,10), (B,D,8),$<br>$ (C,E,15), (C,F,1), (D,F,7), (E,F,12)\}$ |

邻居（Neighbors）：有边相连的两个节点互为邻居。例：B 和 C 是 A 的所有邻居。

### 边列表（Edge list）

| 有向 & 无权图<br>（Directed & Un-weighted） | 有向 & 有权图<br/>（Directed & Weighed） |
| -------------------- | ------------------------ |
|<占位符>|<占位符>|

```mermaid
graph TB

subgraph Directed & Weighed 
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

subgraph Directed & Un-weighted
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
```

| Node1 | Node2 |<占位符>| Node1 | Node2 |Weight|
| ----- | ----- |-------| ----- | ----- | ------ |
|   A   |   B   |<占位符>|   A   |   B   |   12   |
|   A   |   C   |<占位符>|   A   |   C   |   10   |
|   B   |   D   |<占位符>|   B   |   D   |   8   |
|   C   |   E   |<占位符>|   C   |   E   |   15   |
|   C   |   F   |<占位符>|   C   |   F   |   1   |
|   D   |   F   |<占位符>|   D   |   F   |   7   |
|   E   |   F   |<占位符>|   E   |   F   |   12   |
|   E   |   A   |<占位符>|   E   |   A   |   23   |
|   B   |   A   |<占位符>|   B   |   A   |   34   |
