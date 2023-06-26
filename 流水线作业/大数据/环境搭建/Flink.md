一、阿里云-云起实验室-《使用PolarDB-X与Flink搭建实时数据大屏》： https://developer.aliyun.com/adc/scenario/884a36afe52d4850821b85f2c1e40691?spm=a2c6h.14164896.0.0.b5cc4096qLi5Yv ，或者在自己的虚机上安装配置Flink-1.13.6伪分布式。

**实验要求：**

1. 步骤3. 在PolarDB-X中准备订单表，给订单表orders中插入数据的SQL语句中姓名改为：自己姓名全拼1-3。

   ```sql
   CREATE TABLE `orders_` (
    `order_id` int(11) NOT NULL AUTO_INCREMENT,
    `order_date` datetime NOT NULL,
    `customer_name` varchar(255) NOT NULL,
    `price` decimal(10, 5) NOT NULL,
    `product_id` int(11) NOT NULL,
    `order_status` tinyint(1) NOT NULL,
    PRIMARY KEY (`order_id`)
   )AUTO_INCREMENT = 10001;
   
   INSERT INTO orders
   VALUES (default, '2020-07-30 10:08:22', 'lizaosong1', 50.50, 102, false),
          (default, '2020-07-30 10:11:09', 'lizaosong2', 15.00, 105, false),
          (default, '2020-07-30 12:00:30', 'lizaosong3', 25.25, 106, false);
   ```

   ![image-20230426120553617](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230426120553617.png)

   

   ![image-20230426120641093](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230426120641093.png)

   步骤运行Flink，最后一步将PolarDB-X的订单表orders的数据同步到Flink的订单表orders中，截图运行结果。

![image-20230426131259792](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230426131259792.png)

1. 步骤5. 启动压测脚本并实时获取GMV，最后一步显示Flink的实时计算结果截图。

![image-20230426131634877](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230426131634877.png)

![image-20230426131626054](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230426131626054.png)

1. 参考教材12.7.1，测试自带样例词频统计，截图命令及结果。

![image-20230426132351943](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230426132351943.png)

1. 简要说明本实验让你体验到了Flink的什么计算功能？

体验到了flink当中实时计算的功能。

二、包含Flink的实验资源

1. 阿里云实验《基于EMR离线数据分析》https://developer.aliyun.com/adc/scenario/exp/175735954e19429cbb753cd547c00b5a ，步骤2.登录集群，启动Flink。

  查看user表中

![image-20230426133224309](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230426133224309.png)

查看数据表中有多少数据

![image-20230426133309367](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230426133309367.png)

查询数据表中评级最高的三个电影

![image-20230426133338860](C:/Users/janeonly/AppData/Roaming/Typora/typora-user-images/image-20230426133338860.png)



1. 华为云实验《MRS基础入门实践》 https://lab.huaweicloud.com/testdetail_1970，创建集群，启动Flink。



三、简要回答“课堂考核”内容

1. 1.Flink客户端是什么？

   Flink客户端是Apache Flink分布式流处理框架的一个组件，它是用户与Flink集群交互的主要方式。通过Flink客户端，用户可以提交、启动、监控和取消Flink集群上运行的作业。Flink客户端一般运行在用户的本地计算机上，它通过与Flink集群中的JobManager进行通信，来管理分布式作业的运行。

   Flink客户端可以使用Flink提供的命令行工具，如bin/flink、bin/flink run等来操作。除此之外，Flink还提供了REST API、Web UI等多种方式来管理Flink集群上运行的作业。

   2.怎样创建Flink表？实验中怎么导入数据？

   可以使用Flink的SQL API来创建表。

   对于实验中的数据导入，一般来说，可以通过以下几种方式导入数据：

   使用LOAD DATA语句从文件中导入数据。

   使用INSERT INTO语句将数据插入到表中。

   使用COPY命令从其他数据库或者数据源中导入数据。

   导入数据时需要注意数据格式和路径等问题。

    

   3.Flink表怎么查询数据？怎么实时统计数据？

   Flink表的查询数据有多种方式，可以使用Table API、DataStream API和SQL API进行查询。对于实时统计数据，可以使用Flink提供的窗口机制进行实现。比如使用滑动窗口来统计指定时间段内的数据。此外，还可以使用Flink提供的连接外部存储的API实现维表查询，可以使用LookupableTableSource接口实现维表查询。

    

   4.压测脚本是干什么的？

   压测脚本是一种用来测试软件、应用程序或者系统性能的工具，可以通过模拟用户的访问和操作来测试目标系统的性能和稳定性。压测脚本通常会在一定负载下进行测试，并且会记录下系统在不同压力下的表现，比如响应时间、吞吐量、错误率等指标。

   通过进行压力测试可以找到系统的性能瓶颈，发现哪些部分需要优化和改进，从而提高系统的性能和稳定性。压测脚本可以模拟多种场景，比如并发用户数、请求频率、请求类型等，以验证系统在不同压力下的表现。

   压测脚本的应用范围非常广泛，比如在互联网应用、电商平台、金融行业、游戏行业等领域都有着重要的应用。利用压测脚本可以有效避免系统上线后因无法承受流量而导致的系统崩溃和服务中断等问题，保证系统的可靠性和稳定性。

   5.Flink怎么做批处理计算的？

   Flink是一个面向流式计算的分布式计算框架，但是它同样也支持批处理计算。Flink的批处理计算采用DataSet API进行编程，可以使用类似于Java 8的Stream API的编程模型来对数据集进行操作。

   四、12.9 习题

   1. 请阐述传统的数据处理架构的局限性。

   传统的数据处理架构主要采用批处理的方式对数据进行处理和分析，存在以下局限性：

   实时性差：传统的数据处理架构需要等待一定时间的数据积累后再进行批量处理，因此实时性较差，在需要快速地获取实时数据信息时无法满足需求。

   处理效率低下：传统的数据处理架构只能采用串行处理方式，不能并行计算，因此处理效率较低，难以满足大规模数据的处理需求。

   数据不准确：传统的数据处理架构需要经过多个环节的处理和转换，容易出现数据丢失、处理错误等问题，导致数据不准确，影响业务决策的准确性。

   缺乏灵活性：传统的数据处理架构的计算流程在设计时需要提前确定，难以适应不同场景下的数据分析和处理需求，缺乏灵活性。

   成本高昂：传统的数据处理架构需要专门的硬件设备和复杂的软件系统进行支持，成本较高，难以适应企业规模化大数据处理的需求。

   综上所述，传统的数据处理架构存在实时性差、处理效率低下、数据不准确、缺乏灵活性和成本高昂等局限性，在当前大数据快速发展的背景下已经不适应企业对于数据处理需求的要求。

    

   1. 请阐述大数据Lambda架构的优点和局限性。

   大数据Lambda架构是一种结合了批处理和流式处理的数据处理架构，主要由批处理层、速度层和查询层三部分组成。其优点和局限性如下：

   优点：

   适应性强：Lambda架构既可以实现高效的离线批处理，也可以支持实时数据流处理，能够满足不同场景下对数据处理和分析的需求。

   容错性好：Lambda架构通过将数据进行多份备份，从而提高了容错性，即使数据处理出现异常或者某些服务器宕机，也不会影响系统正常运转。

   实时性强：Lambda架构中的速度层可以实现强大的实时数据处理能力，能够快速地对大量实时数据进行处理并输出结果。

   可伸缩性好：Lambda架构具有很好的可扩展性，可以通过加减计算节点的方式进行横向扩展，从而适应高速增长的数据流和计算需求。

   维护成本低：Lambda架构采用开源技术，避免了传统数据处理架构的高成本和依赖性，同时还允许用户根据自身需求进行优化和调整，从而提高了系统的维护成本。

   局限性：

   代码复杂度高：Lambda架构需要在两个不同的API中对同样的业务逻辑进行两次编程，增加了系统的代码复杂度和维护成本。

   数据一致性问题：在Lambda架构中，批处理层和速度层是分离的，而实时处理的数据可能会与离线处理得到的结果不同步，导致数据一致性问题。

   实时性与准确性权衡：速度层具有强大的实时处理能力，但是由于采用了近似计算的方法，可能会存在部分数据计算不准确的情况。

   运维的难度较大：由于Lambda架构需要管理多个计算系统和处理流程，因此部署和管理成本相对较高，需要更完善的运维体系。

   综上所述，Lambda架构能够适应不同场景下的数据处理和分析需求，具有很好的可扩展性和容错性，但是也存在一定的局限性，需要根据具体业务场景进行优化和调整。

    

   1. 请阐述与传统的数据处理架构和Lambda架构相比，流处理架构具有什么优点。

   与传统的数据处理架构和Lambda架构相比，流处理架构具有以下优点：

   实时性更强：流处理架构可以实现对数据的实时处理和分析，通过实时的数据流处理能力，可以更加及时地获取数据的价值，满足对数据实时性的需求。

   灵活性更高：流处理架构采用基于事件驱动的处理方式，在处理过程中动态地选择、调整或者过滤数据，灵活性更高，可以根据业务需求进行灵活配置。

   精确度更高：流处理架构可以快速地对大量实时数据进行分析和处理，并且可以实现精确的数据处理和计算，避免了批量处理带来的近似误差。

   成本更低：与传统的数据处理架构和Lambda架构相比，流处理架构不需要对数据进行离线存储，因此可以减少存储成本，同时也可以避免数据不一致问题。

   可扩展性更好：流处理架构支持水平扩展，可以通过增加节点的方式扩展集群规模，从而更好地适应数据流量的增长和变化。

   综上所述，流处理架构在实时性、灵活性、精确度、成本和可扩展性方面具有优势，适合于处理实时性要求较高的数据场景，如物联网、金融、广告等领域。

    

   1. 请举例说明Flink在企业中的应用场景。

   Flink是一个快速、可扩展且分布式的流处理框架，广泛应用于企业中的实时数据处理和分析场景。以下是Flink在企业中的一些典型应用场景：

   实时数据分析和处理：Flink可以实现对实时流数据的处理和分析，在电商、金融等领域应用广泛。例如，通过Flink实现用户行为数据的实时统计和分析，可以帮助企业做出更优秀的推荐系统和精准广告。

   金融风控：Flink可通过对海量交易数据的实时处理，实现风险预测和风险控制，帮助企业对金融市场进行有效的监测和控制。

   物联网数据处理：Flink可以帮助企业快速处理和分析物联网设备产生的数据，实现对设备状态的监测和管理，提升设备运行效率和品质。

   舆情分析：Flink可通过对社交网络等大规模数据的实时处理和分析，实现对舆论态势的监控和分析，帮助企业了解公众的反馈和意见，进行品牌维护和营销。

   游戏数据处理：Flink可以实现对游戏中产生的海量数据进行实时处理和分析，实现游戏运营的数据驱动决策。例如，通过对玩家行为数据的实时分析，优化游戏体验和盈利策略。

   综上所述，Flink在企业中的应用场景多种多样，能够帮助企业快速实现对实时数据的处理和分析，提升企业的数据驱动决策能力。

    

   1. 请阐述Flink核心组件栈包含哪些层次以及每个层次具体包含哪些内容。

   Flink的核心组件栈分为三个层次，包括物理部署层、Runtime核心层和API&Libraries层。

   物理部署层：Flink的底层是物理部署层，这一层可以采用Local模式运行，启动单个JVM，也可以采用Standalone集群模式运行，还可以采用YARN、Mesos或Kubernetes等分布式集群管理器进行部署。该层包括以下组件：

   Standalone Mode：单机模式

   Local Mode：本地模式

   Cluster：集群模式

   Runtime核心层：Runtime核心层是Flink的计算引擎，包括运行时环境、执行引擎、数据传输和资源调度等组件。该层包括以下组件：

   JobManager：任务管理器

   TaskManager：任务执行器

   DataStream API：基于流的高级编程接口

   DataSet API：基于批处理的编程接口

   Distributed Coordination：分布式协调服务

   Shuffle Service：数据划分服务

   API&Libraries层：API&Libraries层是Flink提供的丰富的高级API和库，包括了针对不同数据处理场景的API和对应的库，如：

   Batch Processing：批处理

   Stream Processing：流处理

   Complex Event Processing：复杂事件处理

   Graph Processing：图像处理

   Machine Learning Libraries：机器学习库

   SQL / Query Processing：SQL支持

   综上所述，Flink的核心组件栈包含着物理部署层、Runtime核心层和API&Libraries层，每个层次都包含了丰富的组件和功能，为企业提供了快速、可扩展且分布式的流处理框架。

   1. 请阐述Flink的JobManager和TaskManager具体有哪些功能。

   Flink的JobManager和TaskManager是Flink Runtime核心层的两个重要组件，它们分别担负着不同的任务并共同完成Flink作业处理。

   JobManager功能：

   作业提交和调度：JobManager负责接收用户提交的Flink作业并进行作业调度。它会将作业分配给TaskManager进行执行，并监控任务的执行状态，以及协调任务之间的依赖关系。

   Checkpoints管理：JobManager负责协调和管理Checkpoint的生成、存储和恢复。Checkpoint是一种容错机制，用于在任务执行过程中定期保存任务状态，保证任务出现故障时可恢复执行。

   故障转移：JobManager会在任务出现故障时自动从之前保存的Checkpoints中恢复任务状态，并重新调度任务执行。通过这种方式，JobManager保证了任务的高可用性和抗故障能力。

   TaskManager功能：

   任务执行：TaskManager根据JobManager的调度指令启动任务处理并执行具体的计算任务，处理流数据或者批量数据，可以从网络或本地文件系统中读取数据，输出结果到网络或本地文件系统。

   内存管理：TaskManager负责管理内存使用，包括堆内存和堆外内存，以及内存的分配、释放和回收等操作。它根据需要动态调整内存的分配策略，确保任务的高效执行。

   Task Slot管理：TaskManager在启动时会注册Task Slot，TaskManager可以同时执行多个任务，每个Task占用一个Task Slot。TaskManager会根据JobManager的调度指令将Task分配到对应的Task Slot上执行。

   综上所述，JobManager和TaskManager分别担负着Flink作业处理中不同的任务，共同完成了整个作业的调度、执行、容错和恢复等。这两个组件的合理协同运作，是Flink实现高效流处理和批处理的重要保障。

    

   1. 请阐述Flink编程模式的层次结构。

   Flink编程模式的层次结构可以理解为Flink的体系结构。Flink编程模式的层次结构包括以下三个层次：

   应用程序层

   应用程序层主要是指Flink应用程序开发的具体业务逻辑实现层，开发人员可以使用Java或Scala等编程语言，利用Flink提供的API进行具体业务逻辑的实现。

   流处理层和批处理层

   Flink提供了流处理和批处理两种不同的处理方式，两者共享同一套API，但是在底层实现上有所不同。

   流处理层：流处理层主要针对实时数据流进行处理，支持事件驱动、流式计算，并提供窗口、状态管理、CEP、表格等高级功能。

   批处理层：批处理层主要针对有限数据集进行处理，支持各种数据源的输入和输出，并提供了丰富的转换算子、聚合操作和多种优化策略，以及对Hadoop生态系统的无缝集成。

   运行时层

   运行时层是Flink的核心部分，它包括JobManager和TaskManager两个组件，负责任务的提交、调度、执行，以及资源管理和任务容错等操作。

   JobManager：JobManager是Flink运行时层的控制中心，负责接受和处理用户提交的任务，协调和管理JobGraph的执行，检查点管理等。

   TaskManager：TaskManager是Flink运行时层的执行引擎，负责实际的任务执行，数据的处理和结果的输出，以及与JobManager的通信、资源管理等。

   Flink编程模式的层次结构将不同的功能分离到不同的层次中，使得Flink可以支持实时流处理和批处理两种使用场景，并提供了一整套高效的运行时机制来支持各种复杂的业务需求。

   1. 请对 Spark、Flink和Storm 进行对比分析。

   Spark、Flink和Storm 都是大数据处理领域比较常用的流式计算框架，它们在数据处理模型、应用场景、性能表现等方面都有所差异。

   数据处理模型

   Spark：Spark最初是一个基于离线批处理的MapReduce并行计算框架，后来引入了Spark Streaming组件，支持流处理和批处理两种处理方式。Spark Streaming采用微批处理的方式对流式数据进行处理，将数据流分割成一段段小的时间间隔（batch），再对每个时间间隔内的数据进行处理。

   Flink：Flink致力于实现连续流式计算，提供了完整的事件驱动模型，支持真正意义上的流式处理，可以同时处理批量数据和无限数据流，并且支持状态管理和容错恢复等特性。

   Storm：Storm是一个分布式实时计算系统，主要面向实时流数据处理，采用分布式流处理模型，支持可靠消息传递、分组聚合、窗口计算等高级功能。

   应用场景

   Spark：Spark的主要应用场景是离线批处理处理和流处理，适用于数据仓库、ETL、机器学习、图计算等领域。

   Flink：Flink的主要应用场景是实时流式计算，适用于金融、物联网、网络安全等需要实时处理数据的领域。

   Storm：Storm的主要应用场景是实时流式计算，适用于互联网广告、社交网络、运营商等需要实时处理数据的领域。

   性能表现

   Spark：Spark的批处理性能表现优秀，处理速度快，可以充分利用内存和磁盘资源，但是对于流数据处理，由于使用了微批处理，数据延迟较大。

   Flink：Flink的流数据处理性能表现优异，能够实现几乎接近于实时的数据处理，同时具备强大的容错和状态管理能力，不需要进行checkpoint操作，避免了数据丢失的风险。

   Storm：Storm的实时流数据处理性能表现极佳，数据延迟在毫秒级别，适合对数据实时性要求较高的场景；但在数据处理中，可能存在数据的重复或丢失情况，需要进行手动的幂等和去重处理，增加了代码实现的复杂性。

   综上所述，Spark、Flink和Storm都有其各自的优势和适用场景，采用不同的数据处理模型和技术架构，企图满足用户不同的需求，开发者在选择框架时应充分考虑自己的应用场景、数据处理模型和性能表现要求。





```c
void strait_insert(SqlList *L){
    
    int i,j;
    for(i=2;i<L->length;i++){
        
        L->R[0]=L->R[i];j=i-1;
        while(LT(L->R[0].key,L->R[j].key)){
            
            L->R[j+1] = L->R[j];
            j--;
        }
        L->R[j+1] = L->R[0];
    }
}
```



  0  1 2 3 4 5

0 0 1 5 2 0 0

1 1 0 3 0 7 0  

2 5 3 0 0 0 6

3 2 0 0 0 0 8

4 0 7 0 0 0 4 

5 0 0 6 8 4 0





```c
void EnQueue(linkQueue *Q,Datatype x){
    QueueNode *p = new QueueNode;
    p->data=x;p->next=Q->next;
    Q->rear->next=P;
    Q->rear = p;
}
```

```C
int fact(int n){
    if(n<=0) return 1;
    
}
```

