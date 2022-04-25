---
title: hadoop fs 命令概述
tags: Hadoop 笔记
---

## 介绍

Hadoop 提供了一个在终端运行的 shell 命令行客户端：hadoop fs [generic option]

```shell
hadoop fs -ls <directory> # 显示 directory 下的所有文件
# 若 directory 未指定前缀（系统），则其读取环境变量中的 fs. defaultFS 属性，以该属性值作为默认文件系统。

hadoop fs -mkdir [-p] <path> # 创建目录 path
# path 为待创建的目录
# -p 选项的行为和 Linux 的-p 非常相似，会沿着路径创建父目录。

hadoop fs -ls [-h] [-R] [<path> ...] # 查看指定目录下的内容
# path 指定目标路径
# -h 人性化显示文件 size
# -R 递归查看指定目录及其子目录

hadoop fs -put [-f] [-p] <localsrc> ... <dst> # 上传文件到 HDFS 指定目录下
# -f 覆盖目标文件
# -p 保留访问和修改时间，所有权和权限
# localsrc 本地文件系统（客户端所在机器）
# dst 目标文件系统 (HDFS) 

hadoop fs -cat <src> ... # 查看 HDFS 文件内容
# 其他的某些 linux 读取命令如 tail 也可以使用。

hadoop fs -get [-f] [-p] <src> ... <localdst> # 下载 HDFS 文件
# localdst 必须是目录
# -f 覆盖目标文件
# -p 保留访问和修改时间，所有权和权限

hadoop fs -appendToFile<localsrc> ... <dst> # 追加数据到 HDFS 文件中
# 将所有给定本地文件的内容追加到给定 dst 文件
# 若 dst 文件不存在，则创建该文件。
# 如果<localSrc>为-，则输入为从标准输入中读取。

hadoop fs -mv <src> ... <dst>
# 移动文件到指定文件夹下
# 可以使用该命令移动数据，重命名文件的名称
```

 
