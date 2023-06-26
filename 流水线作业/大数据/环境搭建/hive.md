# 一、Hive概述

hive是一个搭建再hadoop上的数据仓库，可以使用SQL来做到MapReduce大部分功能。

相对于传统的数据仓库，hive有以下特点

- Hive是部署再Hadoop上的
- 计算使用MapReduce
- 数据存储使用HDFS
- 类SQL语言查询
- hive主要用作大量数据统计分析的，而传统关系型数据库是面向实时数据更新和事务处理的

![image-20230412115335797](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412115335797.png)

## 1. Hive体系结构

![image-20230412120159732](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412120159732.png)

- MetaStore

  元数据信息存储再Mysql当中，其中数据相关的元数据DBS

  数据文件存放再HDFS当中

- Client: 为用户提供直接操作Hive的接口

  Hive Shell： hive提供的脚本命令
  HWL: hive提供的Web端

- Driver: 从HQL语句-> HiveJob（mapreduce）过程

![image-20230412120144151](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412120144151.png)

## 2. Hive的数据模型



---

# 二、作业

**实验要求：**

1. 完成教材9.6-Hive基本操作。
   - 创建数据库
   
   ![image-20230412195907510](C:/Users/janeonly/AppData/Roaming/Typora/typora-user-images/image-20230412195907510.png)
   
   - 创建表
   
   指定存储路径创建表
   
   
   
   ![image-20230412203628918](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412203628918.png)
   
   创建外部表
   
   ![image-20230412203745370](C:/Users/janeonly/AppData/Roaming/Typora/typora-user-images/image-20230412203745370.png)
   
   创建表，并且加上分区字段
   
   ![image-20230412204006457](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412204006457.png)
   
   复制表
   
   ![image-20230412204113372](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412204113372.png)
   
   
   
   
   
   - 创建视图
   
   创建视图little_usr，只包含表usr中id和age属性：
   
   ![image-20230412204456337](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412204456337.png)
   
   
   
   - 删除数据库
   
   - 删除表
   
   ![image-20230412204635004](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412204635004.png)
   
   - 删除视图
   
   ![image-20230412204622285](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412204622285.png)
   
   - 修改数据库、表、视图
   
   修改表名称
   
   ![image-20230412205125748](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412205125748.png)
   
   添加新的分区
   
   ![image-20230412205230916](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412205230916.png)
   
   删除分区
   
   ![image-20230412205352561](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412205352561.png)
   
   将name字段更改为username，并且放在id之后
   
   ![image-20230412205547557](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412205547557.png)
   
   新增sex1字段
   
   ![image-20230412205716341](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412205716341.png)
   
   ![image-20230412205816898](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412205816898.png)
   
   - show查看数据库
   
   
   
   ![image-20230412205859062](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412205859062.png)
   
   ![image-20230412210008835](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412210008835.png)
   
   
   
   
   
   - describe描述数据库、表、视图
   
   查看基本数据库基本信息
   
   ![image-20230412210058725](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412210058725.png)
   
    查看数据库详细信息
   
   ![image-20230412210434933](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412210434933.png)
   
   ![image-20230412210649669](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412210649669.png)
   
   
   
   - 查看表中数据

![image-20230412213852298](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412213852298.png).



1. Hive数据表emrusers改为自己姓名全拼接学号后四位，截图：查询数据表中有多少条数据结果，包含Hive命令。
2. 完成以下华为云三个实验，其中《Hive的数据统计》实验，最后一步多表统计中，两个表名接自己姓名全拼，截图HQL统计语句和执行结果。

二、华为云KooLabs实验

1. 《Hive创建数据仓库》https://lab.huaweicloud.com/testdetail_2019?ticket=ST-8592245-15562XdhIkmrjQtYVvSLKpqK-sso

   1. 创建数据库onlineLeaning

      ![image-20230412214255664](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412214255664.png)

   2. 查看数据库

   ![image-20230412214407649](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412214407649.png)

   3. 表的基本操作

  查看表;

![image-20230412214804603](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412214804603.png)

查看表的详细信息

![image-20230412214706376](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412214706376.png)

  创建分区表

![image-20230412214926300](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412214926300.png)

  查看所有表

![image-20230412215016068](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412215016068.png)

删除分区表

![image-20230412215125485](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412215125485.png)

![image-20230412215149214](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412215149214.png)

1. 《Hive的数据查询》https://lab.huaweicloud.com/testdetail_2020?ticket=ST-8595305-qNAA4uEYy6gEdCzN5mcQ6l1C-sso

  准备初始的数据:

![image-20230412221408321](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412221408321.png)

  导入数据到相应的表

![image-20230412221558681](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412221558681.png)

![image-20230412222034400](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412222034400.png)

![image-20230412222044478](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412222044478.png)

查询数据表:

![image-20230412221948013](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412221948013.png)

![image-20230412222112290](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412222112290.png)

数据查询分为单表查询和多表查询，接下来依次进行实验。

![image-20230412222150500](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412222150500.png)

查询moc_score表中userid为1014200739的成绩数据

![image-20230412222258969](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412222258969.png)

select count(distinct(Alizaosong1834.userid)) from user_tag_value Alizaosong1834, (select userid,course_id, score from moc_score  where course_id = '8001' and score >= '90') as Blizaosong1834 where Alizaosong1834.userid=Blizaosong1834.userid and Alizaosong1834.district like '中国%';

![image-20230412224003264](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412224003264.png)

![image-20230412223952849](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230412223952849.png)



三、简要回答“课堂考核”内容
1.HiveQL语言是不是SQL语言？
HiveQL是一种SQL方言，它是用于Apache Hive的查询语言。因此，可以说HiveQL是SQL语言的变种或方言。HiveQL支持SQL的大部分语法和函数，并且可以通过类似于SQL的查询语句来查询和处理数据。然而，由于Hive是基于Hadoop的数据仓库，它还包括了一些特定于Hadoop生态系统的语法和函数。因此，尽管HiveQL非常相似于SQL，但仍然有一些区别。
2.Hive不指明数据库的话，创建表是哪个数据库的？
在Hive中，如果没有指定数据库，则默认使用"default"数据库。因此，如果您通过Hive CLI或Hue等客户端工具创建表，而没有指定要使用的数据库，则表将被创建在"default"数据库中。 
3.Hive怎么创建表？怎么创建分区表？
要创建Hive表，请按照以下步骤操作：
1. 以Hive CLI（命令行界面）的形式打开Hive。
2. 在Hive CLI中，使用CREATE TABLE命令创建表。例如，要创建一个名为“ mytable”的表，请使用以下命令：```CREATE TABLE mytable ( column1 STRING,column2 INT,column3 DOUBLE);```
3. 如果表需要分区，则在创建表时需要指定分区列。例如，要创建一个分区表，请使用以下命令：```CREATE TABLE mytable_partitioned (column1 STRING,column2 INT,column3 DOUBLE)PARTITIONED BY (partition_column STRING);```在此示例中，“ mytable_partitioned”是表的名称，“ partition_column”是分区列的名称。请注意，分区列必须是表架构中的一部分。
4. 如果您还需要指定表存储的位置，请使用LOCATION关键字。例如：```CREATE TABLE mytable (   column1 STRING, column2 INT,olumn3 DOUBLE)LOCATION '/path/to/table';```在此示例中，“ /path/to/table”是表数据存储的位置。
5. 要查看您创建的表，请使用SHOW TABLES命令。如果您创建了分区表，则可以使用以下命令向表中添加分区：```ALTER TABLE mytable_partitioned ADD PARTITION (partition_column='value');```在此示例中，“ mytable_partitioned”是表的名称，“ partition_column”是分区列的名称，“ value”是要添加到分区列的值。
4.Hive怎么加载数据？列是用什么符合分隔的？行是用什么符合分隔的？
Hive查询数据的方法如下：
- 打开Hive控制台
- 运行`USE`语句指定要查询的数据库
- 运行`SELECT`语句，指定要查询的列，例如：`SELECT column1, column2 FROM table_name;`
查询条件可以在`WHERE`子句中指定，例如：```SELECT column1, column2 FROM table_name WHERE column1 = 'value';```这个查询将只返回`column1`等于`value`的行。除此之外，还可以使用`GROUP BY`子句来分组数据，使用`ORDER BY`子句按特定列进行排序，使用`LIMIT`限制返回的行数等。总的来说，Hive提供了一系列的查询语句和操作，可以帮助用户高效地查询和处理大数据。
hive默认使用的行分隔符是'\n'分隔符 ，字段分隔符字段分隔符是^B
5.Hive查询数据怎么查询？查询条件是什么？
在Hive中，可以通过两种方式加载数据：本地加载和HDFS加载。
1. 本地加载：要将数据从本地加载到Hive中，可以使用`LOAD DATA LOCAL INPATH`命令，语法为：```LOAD DATA LOCAL INPATH 'path/to/data/files' INTO TABLE table_name;```其中，`path/to/data/files`是本地数据文件的路径。要确保数据文件可以被Hive所在的节点读取到。`table_name`是要将数据加载到的Hive表的名称。在本地加载数据时，列与行的分隔符需要在创建表时指定。
2. HDFS加载：要将数据从HDFS加载到Hive中，可以使用`LOAD DATA INPATH`命令，语法为：```LOAD DATA INPATH 'hdfs://path/to/data/files' INTO TABLE table_name;```其中，`hdfs://path/to/data/files`是HDFS上数据文件的路径。要确保数据文件可以被Hive所在的节点读取到。`table_name`是要将数据加载到的Hive表的名称。在HDFS加载数据时，与本地加载不同的是，Hive会使用HDFS的默认列分隔符（`\001`）和行分隔符（`\n`）。如果数据文件使用不同的分隔符，需要在创建表时指定。
6.Hive统计数据怎么统计？ HiveQL语句中统计的关键词是什么意思？
Hive是一个基于Hadoop的数据仓库工具，支持使用HiveQL(Hive Query Language)进行数据统计与分析。在HiveQL中，可以使用多种关键词对数据进行统计。以下是HiveQL中常用的统计关键词及其含义：
1. COUNT：用于统计表中某个列的行数。
2. SUM：用于计算表中某个列的总和。
3. AVG：用于计算表中某个列的平均值。
4. MIN：用于找出表中某个列的最小值。
5. MAX：用于找出表中某个列的最大值。
6. GROUP BY：用于对表中某个或某些列进行分组统计。
7. HAVING：用于过滤分组后的数据。
通过这些统计关键词，可以对Hive表中的数据进行聚合和分析，得到有用的信息和结论。
四、习题 9.8
1.试述在Hadoop生态系统中Hive与其组件之间的相互关系。
Hive是Hadoop生态系统中的一个组件，下面是Hive与其他组件之间的相互关系：
1. Hadoop HDFS：Hive需要依赖Hadoop HDFS来存储数据，因为Hive将数据存储在HDFS上，以便能够利用Hadoop的分布式存储和处理能力。
2. Hadoop YARN：Hive需要依赖Hadoop YARN来管理集群资源，因为Hive通过YARN来运行MapReduce作业。
3. MapReduce：Hive能够将HiveQL转换为MapReduce作业，在Hadoop集群上运行分布式查询。HiveQL语句被转换为MapReduce作业，然后在Hadoop集群上分布式执行。Hive还支持使用Tez引擎运行查询，Tez是一个基于YARN的数据处理框架，可以加速Hive查询。
4. Spark：Hive也可以使用Apache Spark作为数据处理引擎，这要归功于Spark SQL，它提供了类似于HiveQL的语法和功能。Hive可以通过Spark SQL将查询转换为Spark作业，并在Spark上执行查询。
5. ZooKeeper：Hive需要依赖ZooKeeper来协调元数据的访问。ZooKeeper是一个分布式协调服务，可以协调Hive集群中的元数据访问。
6. Oozie：Oozie是一个Hadoop作业调度引擎，可以用于调度Hive作业。Hive可以通过Oozie来调度复杂的作业流程。
综上所述，Hive与其他Hadoop组件之间有着紧密的联系和相互依赖关系。Hive扮演着数据仓库的角色，可以与其他组件协作，以实现数据存储、处理、调度、管理和查询等各种复杂任务。
2.请简述Hive与传统数据库的区别。
Hive主要是Hadoop生态系统中的一种数据仓库工具，而传统数据库则是在关系数据库理论基础上发展而来的，两者之间存在以下几点区别：
1.1 数据存储方式不同：Hive将数据存储在HDFS中，而传统数据库则是存储在本地磁盘中。
1.2 数据访问方式不同：Hive使用类SQL的HiveQL查询语句进行数据的访问，而传统数据库使用SQL语句访问数据。
1.3 数据处理方式不同：Hive是一种批处理方式的数据仓库，数据处理通常需要一定的时间，而传统数据库则更适合用于实时数据的处理。
1.4 查询性能不同：Hive在查询大批量数据时比较高效，而传统数据库在查询小批量数据时较高效。1.5 数据更新方式不同：Hive数据一旦被写入，通常是不会被修改或删除，而传统数据库则支持更为丰富的数据更新操作。
虽然Hive和传统数据库有着相应的区别，但是在大数据场景下，Hive仍然具有得天独厚的优势，适用于大批量的数据仓库处理，数据清洗和数据分析等场景。
3.请简述Hive的几种访问方式。
Hive可以通过不同的方式进行访问，主要包括以下几种方式：
1.1 Hive CLI命令行界面：Hive CLI是Hive自带的命令行界面，可以通过命令行交互式地访问Hive。用户可以使用HiveQL查询语句对数据进行查询、插入、更新、删除等操作，也可以使用Hive CLI来管理表、分区和存储等元数据。
1.2 Beeline命令行界面：Beeline是基于JDBC协议的Hive客户端，可以在任何支持JDBC的环境中使用。用户可以使用Beeline连接到Hive服务器，然后使用HiveQL查询语句进行数据访问和管理操作。
1.3 JDBC/ODBC接口：通过JDBC或ODBC接口，可以在其他编程语言和应用程序中访问Hive。开发人员可以使用Java编程语言或其他支持JDBC和ODBC接口的编程语言来编写程序，然后通过JDBC或ODBC接口连接到Hive服务器，对Hive中的数据进行查询和管理操作。
1.4 Web接口：Hive提供了Web接口，可以通过Web浏览器访问Hive。用户可以使用Hive WebUI来管理Hive元数据，创建、删除、修改表和分区等操作，也可以使用Hive WebUI来执行HiveQL查询语句。
1.5 编程语言接口：Hive支持在Java、Python、Scala、Ruby等编程语言中使用Hive的API进行数据访问和管理操作。开发人员可以使用这些编程语言编写程序，通过Hive的API来访问Hive中的数据。这种方式可以在程序中直接调用Hive的API，从而更好地控制和管理Hive中的数据和元数据。
综上所述，Hive提供了多种访问方式，以适应不同用户和应用程序的需求。用户可以使用不同的客户端工具和编程语言来访问Hive，对Hive中的数据进行查询和管理操作，从而更好地实现数据分析和处理。
4.请分别对Hive的几个注意组成模块进行简要介绍。
Hive是一个基于Hadoop的数据仓库解决方案，包括以下几个核心模块：
1. 元数据模块（Metastore）：Hive的元数据模块存储了所有关于表、分区、列、数据类型和存储位置等信息的元数据。这些元数据存储在关系型数据库中，例如MySQL、PostgreSQL等。元数据模块负责管理这些元数据，并且通过Hive CLI和其他接口来提供元数据查询和管理功能。
2. 查询编译器模块（Query Compiler）：Hive的查询编译器模块负责将HiveQL查询语句转换为MapReduce作业或者Tez作业。该模块负责执行各种查询优化，包括转换、聚合、过滤等。一旦查询编译器通过优化查询生成作业，该作业将在Hadoop集群上执行。
3. 执行引擎模块（Execution Engine）：Hive的执行引擎模块负责查询的执行。该模块负责在Hadoop集群上启动MapReduce或Tez作业，并监视它们的执行状态。执行引擎负责收集执行结果并将它们返回给客户端，以供查询分析和展示。
4. 驱动器模块（Driver）：Hive的驱动器模块是负责将元数据、查询编译器和执行引擎模块组合起来实现HiveQL查询的核心模块。Hive驱动器负责解释和处理HiveQL查询，并将结果返回给客户端。驱动器负责解析查询语句，从元数据模块中获取必要的信息，然后将查询提交给查询编译器模块处理。
综上所述，Hive的元数据模块、查询编译器模块、执行引擎模块和驱动器模块是Hive的核心模块。这些模块协同工作，将HiveQL查询语句转换为Hadoop作业并在Hadoop集群上执行，从而支持大规模数据仓库和数据分析工作。
5.请简述向Hive输入一条查询的具体执行过程。
向Hive输入一条查询的具体执行过程可以分为以下几个步骤：
1. 解析查询语句：首先Hive会解析查询语句，确定需要查询的表、字段以及查询条件等信息，并将这些信息传递给驱动器模块。
2. 获取元数据：接下来驱动器模块会根据查询语句从元数据模块中获取相关的表、分区和列等元数据信息，并将这些信息传递给查询编译器模块。
3. 编译查询：查询编译器模块会将查询语句转换为对应的MapReduce或Tez作业，并对作业进行各种优化，包括查询优化、数据倾斜处理等等。
4. 提交作业：执行引擎模块接收到编译器生成的作业后，将其提交给Hadoop集群执行。作业执行期间，执行引擎模块会监控作业状态，以便及时发现和处理错误。
5. 返回结果：一旦作业执行完毕，执行引擎模块会从作业输出中提取结果，并将其返回给驱动器模块。驱动器模块再将结果返回给客户端，完成整个查询过程。
以上是向Hive输入一条查询的基本流程，需要注意的是，Hive支持不同的输入源和存储格式，如Hive支持从Hive表、Hadoop文件、Hive视图和外部表等不同的输入源中检索数据，同时还支持不同的存储格式，如ORC、Parquet等。在查询过程中Hive会自动针对不同的输入源和存储格式进行优化，以提供更快速的查询性能。
6.请简述Hive HA原理。
Hive HA（高可用性）是为了保证在集群中的某个节点（例如Hive Metastore节点）出现故障时，Hive服务能够继续提供服务的机制。Hive HA主要通过以下两种机制实现：
1. 自动故障转移（Automatic Failover）：Hive HA通过在主节点和备用节点之间进行心跳检测，当主节点失效后，备用节点会自动接管主节点并提供服务，从而实现故障转移。在故障转移过程中，已经存在的客户端请求将被自动重定向到备用节点上。
2. 共享存储模型（Shared Storage Model）：Hive HA通过将元数据信息存储在共享存储设备上，使得备用节点能够快速接管主节点的元数据管理任务。在共享存储模型下，主节点和备用节点都能够访问共享存储设备上的同一份元数据信息。在主节点失效后，备用节点可以快速接管主节点的元数据管理任务，从而保持服务的连续性。
Hive HA的实现需要考虑以下几个因素：
1. 共享存储设备的可靠性和性能：由于元数据信息存储在共享存储设备中，因此，共享存储设备的可靠性和性能对于Hive HA非常重要。通常情况下，Hive HA会使用分布式文件系统（如HDFS）作为共享存储设备。
2. 心跳检测机制的准确性和效率：Hive HA的心跳检测机制需要快速、准确地检测出主节点的故障，并将备用节点自动接管主节点的任务。在设计心跳检测机制时，需要兼顾准确性和效率，并考虑到网络延迟等因素对检测效率的影响。
3. 数据一致性和完整性：在自动故障转移过程中，需要保证数据的一致性和完整性。为此，Hive HA通常会使用事务一致性协议（如ZooKeeper）来确保故障转移过程中数据的一致性和完整性。
7.请简述Impalad进程的主要作用。
Impalad进程是Impala中最重要的进程之一，它在Impala集群中扮演着以下几个角色：
1. 查询处理器(Query Processor)：Impalad进程是Impala的核心查询处理器，它负责接收来自客户端的查询请求，并将查询请求转换为对应的执行计划。在查询执行过程中，Impalad进程会进行各种优化，包括查询重写、列剪枝、数据分片等，以提高查询性能。
2. 数据管理器(Data Manager)：Impalad进程负责对底层数据进行管理，包括数据的分片、存储、读取和写入等。在数据管理过程中，Impalad进程会利用底层数据存储系统（如HDFS、HBase等）提供的数据管理API，以实现高效的数据管理和操作。
3. 元数据管理器(Metadata Manager)：Impalad进程负责管理Impala元数据信息，包括表结构、列信息、表分区信息、数据位置等。在元数据管理过程中，Impalad进程会与Metastore服务进行交互，从Metastore读取和更新元数据信息。
4. 通信协调器(Communication Coordinator)：Impalad进程负责维护Impala集群中各个节点之间的通信，包括查询请求的转发、数据块的传输、查询状态的同步等。在通信协调过程中，Impalad进程会利用Impala自身的通信协议和网络传输协议，以实现高效的通信。
总之，Impalad进程是Impala集群中的核心组件之一，它负责处理来自客户端的查询请求，管理底层数据存储系统，管理Impala元数据信息，以及维护集群各个节点之间的通信，是Impala的重要执行引擎。
8.请比较Hive与Impala的异同点。
Hive和Impala是Hadoop生态系统中两个常见的SQL查询工具，它们都提供了面向大规模数据的SQL查询能力。它们之间的主要区别在于以下几个方面：
1. 查询性能：Impala是一个基于内存的分布式查询引擎，它使用了MPP（Massively Parallel Processing）技术，能够实现高速数据处理和查询能力。而Hive则是一个基于MapReduce的查询引擎，性能相对较慢。因此，在处理大数据集和复杂查询时，Impala比Hive更快。
2. 数据存储：Hive支持多种底层存储系统，包括HDFS、HBase等，而Impala仅能够在HDFS上运行。因此，如果需要使用其他数据存储系统进行查询，Hive是更好的选择。
3. 数据类型：Hive支持更多的数据类型，包括日期、时间、二进制等，而Impala仅支持常见的数据类型。因此，在需要使用特殊数据类型的查询时，Hive可能更适合。
4. 实时性：Impala能够提供更快的查询响应时间，因此更适合实时数据分析和查询。而Hive则更适合离线批处理任务。
5. 适用场合：Hive更适合用于批处理任务，如ETL、数据清洗，而Impala更适合实时数据分析任务，如交互式查询和数据探索。
总之，两者都有自己的优势和适用场合，选择哪一个取决于具体的使用场景和需求。
9.请简述State Store的作用。
State Store是Apache Flink中的一个关键组件，它的主要作用是存储和管理Flink作业中的元数据信息，并为作业提供高可用性的状态管理服务。在Flink作业中，State Store会保存一些关键的元数据信息，如状态信息、检查点信息、作业配置信息等。这些信息是对于作业运行过程中状态的维护和管理非常重要的。State Store提供了高可用性的状态管理，它通常会运行在分布式环境中，并使用ZooKeeper等分布式协调服务来实现高可用性和一致性。如果状态管理服务节点失效，State Store会自动切换到备用节点上，以保证作业的正常运行。同时，State Store还为Flink作业提供了一些状态管理功能，如容错、检查点、状态恢复等。在Flink作业中，State Store会将作业的状态信息存储在内存中，并定期将状态信息持久化到外部存储系统中，以保证状态信息的安全性和可靠性。总之，State Store是Apache Flink中的一个非常重要的组件，它为Flink作业提供了高可用性的状态管理服务，以及状态管理功能，帮助Flink作业实现高效的数据处理和分析。
10.请简述Impala执行一条查询的具体过程。
当Impala执行一条查询时，它会经历如下的具体流程：
1. 语法解析：首先，Impala会读取查询语句，并进行语法解析，以确定查询语句的正确性和结构。如果查询语句存在错误，Impala会返回查询错误信息。
2. 元数据检索：接下来，Impala会检索元数据信息，包括表结构、数据位置等信息。这些元数据信息会被缓存，以提高查询性能和减轻元数据服务的负载。
3. 查询计划生成：根据查询语句和元数据信息，Impala会生成查询计划，该计划包括查询的逻辑执行流程、数据读取方式、数据过滤方式等。
4. 查询优化：Impala会对查询计划进行优化，以提高查询性能。优化的过程包括谓词下推、列裁剪、分区剪裁、谓词合并、连接重排序等。
5. 执行查询：最后，Impala会执行查询计划，并返回查询结果。在查询执行的过程中，Impala会利用多核CPU、内存和磁盘等资源，以实现高效的数据处理和查询能力。
总之，Impala执行一条查询的过程包括语法解析、元数据检索、查询计划生成、查询优化和查询执行等步骤，其中每个步骤都具有重要的作用，以保证Impala能够快速、高效地处理和查询数据。
11.请列举Hive中的列所支持的3种集合数据类型。
在Hive中，以下是三种支持的集合数据类型：
1. Array：表示一组具有相同类型的有序元素的集合。数组中的每个元素都可以通过其索引号进行访问，索引号从0开始。数组中的元素可以是任意的Hive数据类型，包括其他集合类型。
2. Map：表示一组键值对的无序集合。Map中的键和值可以是任意的Hive数据类型，包括其他集合类型。键的名称必须是字符串。
3. Struct：表示具有命名字段的记录或结构体。结构体中的每个字段都具有名称和类型，字段的类型可以是任何Hive数据类型，包括其他集合类型。
这三种集合数据类型在Hive中都有广泛的应用，可用于存储和处理复杂的数据结构，如JSON、XML、Log等。同时，这些数据类型的使用也可以极大地简化Hive查询中的语法和表达式，提高查询的可读性和可维护性。
12.请举例几个Hive的常用操作及基本语法。
以下是Hive中的几个常用操作及其基本语法：

1. 创建表格：
  ```
   CREATE TABLE table_name (
    column1_name column1_type,
    column2_name column2_type,
    ...
    ) [ROW FORMAT row_format]
    [STORED AS file_format]
  ```
2. 插入数据：
 ```
   INSERT INTO table_name VALUES (value1, value2, ...)
 ```

3. 查询数据：
   ```
   SELECT column1_name, column2_name, ...
   FROM table_name
   [WHERE condition]
   [ORDER BY column_name [ASC | DESC]];
   ```
4. 更新数据：
   ```
    UPDATE table_name
   SET column_name = new_value
   [WHERE condition];
   ```
5. 删除数据：
   ```
   DELETE FROM table_name
   [WHERE condition];
   ```
6. 创建视图：
   ```
   CREATE VIEW view_name AS
   SELECT column1_name, column2_name, ...
   FROM table_name
   [WHERE condition];
   ```
7. 加载数据：
   ```
   LOAD DATA [LOCAL] INPATH 'filepath'
   [OVERWRITE] INTO TABLE table_name;
   ```
   以上是Hive中常用的操作及其基本语法，还有其他的操作，如ALTER TABLE、DROP TABLE等等
