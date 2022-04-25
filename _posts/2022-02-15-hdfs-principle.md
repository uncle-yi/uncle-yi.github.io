---
title: HDFS 概述
tags: Hadoop 笔记
article_header:
  type: cover
  image:
    src: /headers/2022-02-15/hadoop.jpg
---

## HDFS 简介

HDFS (Hadoop Distributed File System) ，意为：Hadoop分布式文件系统。

HDFS 作为文件系统有以下几种特性：

- 分布式运行（在多台计算机上运行的文件系统）
- 高度容错，海量存储
- 统一的访问接口，使得访问过程和普通单机的文件系统别无二致

## HDFS 的设计目标

1. 在大数据成百上千的服务器运行环境下，HDFS 必须具有**故障检测和自动快速恢复**的功能。
2. HDFS 被设计为用于批处理，而非用户交互式的操作，所以其应用设计以流式读取数据为主。
3. HDFS 可以支持大文件，它可以提供很高的聚合数据带宽。
4. "write-one-read-many"，一个文件一旦创建、写入、关闭后就不需要修改了，这一假设简化了一致性的问题。
5. 移动计算的代价比移动数据的代价低。
6. 易于移植到其他平台。

## HDFS 的重要特性

### 主从架构

- HDFS 集群是标准的 master/slave 主从架构集群。
- 一般由一个 Namenode 和多个 Datanode组成
- Namenode 是 HDFS 的主节点，Datanode是从节点，两者共同完成存储服务。

### 分块存储

- HDFS 的文件在物理上是分块 (block) 存储的，默认大小是128M (134217728)，不足则其自身是一个块。
- 块的大小通过 hdfs-default.xml中的：dfs.blocksize 参数来调整。

### 副本机制

- 所有 block 都会有副本。副本系数可以在创建文件时指定，也可以之后修改。
- 副本数由参数 dfs.replication 控制，默认值是3，也就是会额外再复制2份。

### 元数据管理

- 在 HDFS 中，Namenode 管理的元数据具有两种类型：
  - 文件自身属性信息：名称、权限、修改时间、大小、复制因子、块大小
  - 文件块位置映射信息：记录文件块和 Datanode 之间的映射信息，即那个快位于哪个节点上。

### Namespace

- HDFS 支持传统的层次型文件组织结构。用户可以对文件进行创建、删除、移动或重命名文件。
- Namenode 负责维护文件系统的 namespace 名称空间，任何对文件系统名称空间或属性的修改都将被 Namenode 记录下来。
- HDFS 会给客户端提供一个统一的抽象目录树，客户端通过路径来访问文件。

## HDFS 各个角色职责介绍

### 主角色：namenode

#### 职责：

- Namenode 是 Hadoop 分布式文件系统的核心，架构中的主角色
- Namenode 维护和管理文件系统元数据，包括名称空间目录树结构、文件和块的位置信息、访问权限等信息。
- 基于此，Namenode成为了访问 HDFS 的唯一入口。

> tips:
>
> 1. Namenode 只存储 HDFS 的元数据。
> 2. Namenode 知道 HDFS 中任何给定文件的块列表及其位置。
> 3. Namenode 不持久化存储每个文件中各块所在的 Datanode 的位置信息，这些信息会在系统启动时从 Datanode 重建。
> 4. Namenode 是 Hadoop 集群中的单点故障。
> 5. Namenode 所在机器通常会有配置大量内存 (RAM)。

#### 管理方式：

- Namenode内部通过内存和磁盘文件两种方式管理元数据。
- 其中磁盘上的元数据文件包括 Fsimaga 内存元数据镜像文件和 edits log (Journal) 编辑日志。

### 从角色：datanode

#### 职责：

- Datanode 是 Hadoop HDFS 中的从角色，负责具体的数据块存储。
- Datanode 的数量决定了 HDFS 集群的整体数据存储能力。通过和 Namenode 配合维护着数据块。

>  tips:
>
> 1. Datanode 负责最终数据块 block 的存储。是集群的从角色。
> 2. Datanode 启动时，会将自己注册到 Namenode 并汇报自己持有的块列表。
> 3. 当某个 Datanode 关闭时，不会影响数据的可用性。Namenode 将安排由其他 Datanode 管理的块进行副本复制。
> 4. Datanode 所在机器通常配置有大量的硬盘空间。

### 主角色辅助角色：secondarynamenode

#### 职责：

- Secondary Namenode 充当 Namenode 的辅助节点，但不能替代 Namenode。
- 主要是帮助主角色进行元数据文件的合并动作。