Hive
===
## 功能:
存储和etl(数据分析)。hive是基于hadoop的，用hdfs存储，mapreduce做计算/数据分析。
![hive架构图](https://raw.githubusercontent.com/Marvin-cece/Image-Hosting/master/hive/hive%E6%9E%B6%E6%9E%84%E5%9B%BE.png)

## hive架构组件：
![hive组成部分](https://raw.githubusercontent.com/Marvin-cece/Image-Hosting/master/hive/hive%E8%BF%87%E7%A8%8B.png)

**用户接口**：(cli shell命令行、jdbc/odbc代码、webgui浏览器)，用于输入sql。用户接口交互方式中生产常用：hive启动为服务，用beeline去连接 bin/beeline -u jdbc:hive2://mini1:10000 -n root或者 bin/beeline ! connect jdbc:hive2//mini1:10000

**元数据存储**：通常是在关系型数据库中，表名、列等。内置的derby启动简单，但是在不同路径启动会生成不同的元数据，无法共享。通常用第三方的mysql，第三方mysql只要连接同一个hive就可以共享元数据。

**解释器、编译器、优化器、执行器（跟hadoop交互）**：hql解析编译优化以及查询计划（mr程序）的生成（生成完存储在hdfs中），随后有yarn分配mapreduce调用执行。在查询计划的生成的过程中，即解析hql的过程中，是去元数据进行映射。


优点：不用用户直接写mr程序，hadoop一开始只接收mr程序，后来可以接收spark。


### hive命令行:
/export/server/hive/bin/hive
hive参数配置：配置文件（全局对所有hive进程有效），命令行参数（对hive启动实例有效），参数声明（对hive的连接session有效）。优先级递增

### hive配置
（用户自定义覆盖默认配置）覆盖Hadoop的配置

### 映射：
建表后，要映射出来要把映射文件放在路径下。建表跟已存在的结构化数据文件产生映射关系。对结构化文件的处理转化为对表的处理。跟user/hive/warehouse 下的文件夹对应，文件夹名字为表名。sql语句生成的查询计划就是映射hdfs中的结构化数据。

工具取代：离线大数据架构 离线数仓
实时数仓：lambda架构，在离线基础上加了加速层，用流处理技术完成。离线+实时。
kappa架构:全实时。

### 数仓和数据库和湖区别:
![数仓和数据库](https://raw.githubusercontent.com/Marvin-cece/Image-Hosting/master/hive/%E5%BA%93%E5%92%8C%E6%B9%96%E5%AF%B9%E6%AF%94.png)
存储：hive用hdfs，rdbms用本地文件系统。  
执行：默认执行引擎1.0mr/2.0spark，执行引擎innodb  
执行延迟：mr执行慢延迟高只适合离线。  
规模：hive取决于文件规模  

**数据湖**：作为一个集中的存储库，可以存储任意规模的所有结构化和非结构化数据的任意信息，存储未经处理的原始数据。仅在分析时再进行转换。存储数据之后定义schema

**数据仓库**：则是一种数据存储系统，它将来自不同来源的结构化数据聚合起来，是包含多种数据的存储库。是存储经过处理的和精炼的数据。通常从事务系统中提取，在将数据加载到数据仓库之前，会对数据进行清理与转换。用于月度报告等等支持决策。存储数据之前定义schema，意味着需要清理和规范化数据。

`数据仓库的数据是面向主题的`  
`数据仓库的数据是集成的`
```
数据大多来自于传统数据库的，但是数据通常不是直接入库，而且需要先进行数据清洗。因为事务性数据库中的数据通常会存在脏数据，这些脏数据会对基于数仓进行的分析和挖掘造成影响。
```
`数据仓库的数据是不可更新的`
```
用于支持决策，不可修改。  
```
`数据仓库的数据是随时间不断变化的`
```
数据仓库中的表通常会有生命周期，对于超过生命周期的分区数据，一般会删除或者改变数据存储结构单独存储。
```




