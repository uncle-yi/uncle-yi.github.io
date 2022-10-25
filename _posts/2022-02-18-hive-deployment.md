---
title: Hive 部署记录
tags: Hadoop Hive 记录
---
## 缘由

昨天在常用的 Ubuntu 虚拟机上安装了 Hadoop 和 Hive，记录一下遇到的坑。
本来是按照《Hive 编程指南》上面的教程来安的，但是才发现这本书似乎已经很老了，四个已部署的环境的链接，三个已经挂了。不过为了学习一下还是没有用剩下那个，从头安一遍也能熟悉一下环境。

<!--more-->

## 部署过程
首先是 jdk，这里我的环境里已经有之前自动安装的 Open-jdk 了，但是奈何我按着书的版本安装的 Hive 0.9.0 不支持 1.7 以上版本的 jdk，所以这里得卸载后重新安装（可能换了环境变量以后不重装也行，但是为了保险起见我还是卸载了）。

Hadoop 和 Hive 的安装都很简单，只要解压源码包后设置环境变量即可，确保 `HADOOP_HOME`、`HIVE_HOME` 都指向正确的位置且 `$HADOOP_HOME/bin` 和 `$HIVE_HOME/bin` 都加入 `PATH` 即可。

你可以把环境变量设置到 /etc/profile 来让所有用户都可以访问，但如果你不想设置一堆权限问题的话，我推荐的配置方案是将所有软件安装到用户的home路径下，并在 $HOME/.bashrc 设置所有的环境变量。

如果你也在阅读《Hive 编程指南》，并想配置和书中匹配的版本的话，可以使用下面的链接。原书中 Hadoop 的链接已经挂了。  
Hadoop: https://archive.apache.org/dist/hadoop/core/hadoop-0.20.2/hadoop-0.20.2.tar.gz  
Hive: http://archive.apache.org/dist/hive/hive-0.9.0/hive-0.9.0-bin.tar.gz

另外如果你要是用本地配置运行 Hive 的话，那么 $HIVE_HOME/conf/hive-site.xml 的内容只要使用和书中一样即可，如下所示：
```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
 <property>
  <name>hive.metastore.warehouse.dir</name>
  <value>/home/username/Documents/hive/warehouse</value>
  <description>
  这个属性是你本地仓库的部署路径，设置好后就不会在运行hive时随意创建derby文件了
  </description>
 </property>
 <property>
  <name>hive.metastore.local</name>
  <value>true</value>
 </property>
 <property>
  <name>javax.jdo.option.ConnectionURL</name>
  <value>jdbc:derby:;databaseName=/home/username/Documents/hive/metastore_db;create=true</value>
  <description>
  这里是JDBC链接的元数据库路径链接，
  </description>
 </property>
</configuration>
```
如果你像我一样想使用 mysql 存储元数据的话，一定要记住 1.7 版本的 jdk 必须使用 5.1.x 版本的 mysql connector，我在这里卡了很长时间，Hive 一直报错，后来才发现是版本问题。我的 hive-site.xml 内容如下：
```xml
<?xml version="1.0"?>  
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>  
  
<configuration>  
<property>  
<name>hive.metastore.local</name>  
 <value>true</value>  
</property>  
  
<property>  
<name>javax.jdo.option.ConnectionURL</name>  
 <value>jdbc:mysql://localhost:3306/hive?createDatabaseIfNotExist=true&amp;useSSL=false</value>  
  <description>
  这个属性是你mysql的url路径，hive?的部分是你的数据库名称，如果是hive_db则更换成hive_db?
  </description>
</property>
  
<property>  
<name>javax.jdo.option.ConnectionDriverName</name>  
 <value>com.mysql.jdbc.Driver</value>  
</property>  
  
<property>   
 <name>javax.jdo.option.ConnectionUserName</name>   
 <value>root</value>      
  <description>
  这个属性是你登录mysql的账号，如果只是学习可以设置为root，若为生产环境则推荐设置为其他用户
  </description>
</property>
  
<property>   
 <name>javax.jdo.option.ConnectionPassword</name>   
 <value>passwd</value>     
  <description>
  这个属性是你登录mysql的账号对应的密码
  </description>
</property>
  
<property>   
 <name>datanucleus.fixedDatastore</name>   
 <value>false</value>   
</property>   
</configuration>
```

接下来就可以使用 Hive 了。