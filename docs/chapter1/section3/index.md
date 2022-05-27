# 集群启动

## 1 NameNode format

译为：格式化操作。

首次启动HDFS必须做的操作，本质上是初始化工作，进行HDFS清理和准备工作，如建立一些必要的工作目录和初始文件之类的。

```
hdfs namenode -format
```

敲击回车后，查看打印日志若含以下打印，则表示format成功：

```
storage directory /export/data/hadoop-3.1.4/dfs/name has been successfully formatted.
```

>该目录在集群环境搭建修改配置文件的第2步配置过。

该操作在启动后，不可重复执行，会造成数据丢失，导致hdfs集群主从角色互不识别。

若已经重复执行了，只有**删除**所有机器的hadoop.tmp.dir目录，然后**重新**format。

## 2 启动角色

角色启动完成后可以用java命令jps查看角色进程（因为这些都是java程序）。

### 2.1 手动启动

可以每次手动执行命令来启动或关闭一个角色进程: 精准控制某台机器的某个角色。

* HDFS集群
  ```
  ## start
  hdfs --daemon start namenode
  hdfs --daemon start datanode
  hdfs --daemon start secondarynamenode
  ## stop
  hdfs --daemon stop namenode
  hdfs --daemon stop datanode
  hdfs --daemon stop secondarynamenode
  ```
* YARN集群
  ```
  ## start
  yarn --daemon start resourcemanager
  yarn --daemon start nodemanager
  ## stop
  yarn --daemon stop resourcemanager
  yarn --daemon stop nodemanager
  ```

### 2.2 脚本启动

sbin目录下有一键启动的脚本。

>使用一键启动的脚本的前提：配置好机器之间的ssh免密登录和workers文件

* hdfs集群
  * start-dfs.sh
  * stop-dfs.sh
* yarn集群
  * start-yarn.sh
  * stop-yarn.sh
* hadoop集群=hdfs+yarn
  * start-all.sh
  * stop-all.sh

### 2.3 启动错误

查看日志，在安装目录下的logs目录中。

### 2.4 web UI

**hdfs集群**：http://namenodeHost:9870

namenodeHost: namenode所在机器的地址，即ip.
>windows可以通过配置hosts文件，让ip与主机名对应，使用主机名进行访问。linux不用，可直接通过主机名。

**yarn集群**：http://resourcemanagerHost:8088

resourcemanagerHost: resourcemanager所在机器的地址。

## 3 基准测试（benchmark）

又称：压力测试。