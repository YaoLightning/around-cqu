- ##### 大数据应用的软件

  - Hadoop

    - HDFS分布式文件系统

      4_Hadoop概述+HDFS

    - YARN资源调度

    - MapReduce计算框架

      5_Hadoop（YARN+MapReduce）

  - Hbase

    6_分布式数据库      P 52

  - spark

    7_spark系统

- 配置完成后，我们切换到hadoop用户下：

  ```
  su hadoop   # 注意，不要使用root用户，以下全部切换到hadoop用户下操作
  ```

  

  ⚠️ 如非特殊说明，**接下来所有命令都是Hadoop用户（不用使用root用户）下完成**

- 退出Hadoop为logout+exit

- 启动pyspark

  ```python
  cd /usr/local/spark
  bin/pyspark
  ```

- 命令 `sudo reboot`重启

-  *在master节点上* 停止集群：

  ```
  cd /usr/local/hadoop  # 切换到你的hadoop目录下
  sbin/stop-all.sh      # 关闭集群
  ```

- 启动集群

  ```
  cd /usr/local/hadoop
  sbin/start-all.sh
  ```

- 启动master节点

  ```
  cd /usr/local/spark/
  sbin/start-master.sh
  ```

- 启动所有slave节点

  ```
  sbin/start-slaves.sh
  ```

- **单独启动master**

  - sbin/start-slave.sh spark://121.37.177.146:7077  #单独启动master

- **重启方法**：重启下集群，停止后内存占用会很快减轻，然后再启动集群跑一次**可能**就可以了。

  ```
  cd /usr/local/spark
  sbin/stop-master.sh 
  sbin/stop-workers.sh
  
  cd /usr/local/hadoop
  sbin/stop-all.sh
  ```

  

  然后我们重新启动一下集群。

  ```
  cd /usr/local/hadoop  
  sbin/start-all.sh 
  
  cd /usr/local/spark
  sbin/start-master.sh
  sbin/start-slaves.sh
  sbin/start-slave.sh spark://121.37.177.146:7077
  ```

  

  重新执行一下任务。

- **在进行对hdfs的操作时出现错误：**
  `Zero blocklocations for /chn/train.csv. Name node is in safe mode.`
  解决方法：因为namenode节点当前处于安全模式，强制退出安全模式即可。
  （1） 进入hadoop下：`cd /usr/local/hadoop`
  （2） 解除安全模式：`dfsadmin -safemode leave`

- 提交代码

  ```
  bin/spark-submit /home/hadoop/project/001.py
  ```

- 为避免权限问题，请最好先给予权限：

```
sudo chmod -R 777 /home/hadoop/Experiment/Ex4/Ex4_CustomerForecast/
```

- 上传集群运行任务

- 提交代码：


```
cd /usr/local/spark
bin/spark-submit --master spark://master:7077 --executor-memory 4G /home/hadoop/Experiment/Ex4/Ex4_CustomerForecast/forecast.py
bin/spark-submit --master spark://master:7077 --executor-memory 11G /home/hadoop/project/DTM1.py

```

- HDFS操作


1. 创建文件夹

   ```
   cd /usr/local/hadoop
   bin/hadoop fs -mkdir -p /ex/ex3dataset  # -p 参数可用于创建多级目录
   ```

2. 上传数据集

将本地文件`iris.data` 上传到 `hdfs://master:9000/ex/ex3dataset/iris.data` 。

​     以下上传的`hdfs` 路径可简写为：`/ex/ex3dataset`

```
bin/hadoop fs -put /home/hadoop/Experiment/Ex3/Ex3_Kmeans/src/iris.data /ex/ex3dataset
```

HDFS文件查看的命令：

```
hdfs dfs -ls /

/home/hadoop/project
```

