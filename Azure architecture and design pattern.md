# 1 Azure application architecture fundamentals

## [1-1 Introduction]((https://docs.microsoft.com/en-us/azure/architecture/guide/))

This library of content presents a structured approach for designing applications on Azure that are scalable, secure, resilient, and highly available. The guidance is based on proven practices that we have learned from customer engagements.

### Introduction

The cloud is changing how applications are designed and secured. Instead of monoliths, applications are decomposed into smaller, decentralized services. These services communicate through APIs or by using asynchronous messaging or eventing. Applications scale horizontally, adding new instances as demand requires.

These trends bring new challenges. Application states are distributed. Operations are done in parallel and synchronously. Applications must be resilient when failures occur. Malicious actors continuously target applications. Deployments must be automated and predictable. Monitoring and telemetry are critical for gaining insight into the system. This guide is designed to help you navigate these changes.

| **Traditional on-premises**          | **Modern cloud**                                   |
| ------------------------------------ | -------------------------------------------------- |
| Monolithic                           | Decomposed                                         |
| Designed for predictable scalability | Designed for elastic scale                         |
| Relational database                  | Polyglot persistence (mix of storage technologies) |
| Synchronized processing              | Asynchronous processing                            |
| Design to avoid failures (MTBF)      | Design for failure (MTTR)                          |
| Occasional large updates             | Frequent small updates                             |
| Manual management                    | Automated self-management                          |
| Snowflake servers                    | Immutable infrastructure                           |

### How this guidance is structured

The Azure application architecture fundamentals guidance is organized as a series of steps, from the architecture and design to implementation. For each step, there is supporting guidance that will help you with the design of your application architecture.

![](https://docs.microsoft.com/en-us/azure/architecture/guide/images/a3g.svg)

### Architecture styles

The first decision point is the most fundamental. What kind of architecture are you building? It might be a microservices architecture, a more traditional N-tier application, or a big data solution. We have identified several distinct architecture styles. There are benefits and challenges to each.

### Technology choices

Knowing the type of architecture you are building, now you can start to choose the main technology pieces for the architecture. The following technology choices are critical:

* *Compute* refers to the hosting model for the computing resources that your applications run on. 
* *Data stores* include databases but also storage for message queues, caches, logs, and anything else that an application might persist to storage.
* *Messaging* technologies enable asynchronous messages between components of the system.

You will probably have to make additional technology choices along the way, but these three elements (compute, data, and messaging) are central to most cloud applications and will determine many aspects of your design.

### Design the architecture

Once you have chosen the architecture style and the major technology components, you are ready to tackle the specific design of your application. Every application is different, but the following resources can help you along the way:

#### Reference architectures

Depending on your scenario, one of our reference architectures may be a good starting point. Each reference architecture includes recommended practices, along with considerations for scalability, availability, security, resilience, and other aspects of the design. Most also include a deployable solution or reference implementaion.

#### Design principles

We have identified 10 high-level design principles that will make your application more scalable, resilient, and manageable. These design principles apply to any architecture style. Throughout the design process, keep these 10 high-level design principles in mind.

#### Design patterns

Software design patterns are repeatable patterns that are proven to solve specific problems. Our catalog of Cloud design patterns addresses specific challenges in distributed systems. They address aspects such as availability, high availability, operational excellence, resiliency, performance, and security.

#### Best practices

Our best practices articles cover various design considerations including API design, autoscaling, data partitioning, caching, and so forth. Review these and apply the best practices that are appropriate for your application.

#### Security best practices

Our security best practices describe how to ensure that the confidentiality, integrity, and availability of your application aren't compromised by malicious actors.

### Quality pillars

A successful cloud application will focus on five pillars of software quality: Cost optimization, Operational excellence, Performance efficiency, Reliability, and Security.

Leverage the Microsoft Azure Well-Architected Framework to assess your architecture across these five pillars.

## 1-2 Architecture styles

### [1-2-1 Overview]((https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/))

An architecture style is a family of architectures that share certain characteristics. For example, N-tier is a common architecture style. More recently, microservice architectures have started to gain favor. Architecture styles don't require the use of particular technologies, but some technologies are well-suited for certain architectures. For example, containers are a natural fit for microservices.

We have identified a set of architecture styles that are commonly found in cloud applications. The article for each style includes:

* A description and logical diagram of the style.
* Recommendations for when to choose this style.
* Benefits, challenges, and best practices.
* A recommended deployment using relevant Azure services.

#### A quick tour of the styles

This section gives a quick tour of the architecture styles that we've identified, along with some high-level considerations for their use. Read more details in the linked topics.

##### N-tier

N-tier is a traditional architecture for enterprise applications. Dependencies are managed by dividing the application into *layers* that perform logical functions, such as presentation, business logic, and data access. A layer can only call into layers that sit below it. However, this horizontal layering can be a liability. It can be hard to introduce changes in one part of the application without touching the rest of the application. That makes frequent updates a challenge, limiting how quickly new features can be added.

N-tier is a natural fit for migrating existing applications that already use a layered architecture. For that reason, N-tier is most often seen in infrastructure as a service (IaaS) solutions, or application that use a mix of IaaS and managed services.

##### Web-Queue-Worker

For a purely PaaS solution, consider a Web-Queue-Worker architecture. In this style, the application has a web front end that handles HTTP requests and a back-end worker that performs CPU-intensive tasks or long-running operations. The front end communicates  to the worker through an asynchronous message queue.

Web-queue-worker is suitable for relatively simple domains with some resource-intensive tasks. Like N-tier, the architecture is easy to understand. The use of managed services simplifies deployment and operations. But with complex domains, it can be hard to manage dependencies. The front end and the worker can easily become large, monolithic components that are hard to maintain and update. As with N-tier, this can reduce the frequency of updates and limit innovation.

##### Microservices

If your application has a more complex domain, consider moving to a Microservices architecture. A microservices application is composed of many small, independent services. Each service implements a single business capability. Services are loosely coupled, communicating through API contracts.

Each service can be built by a small focused development team. Individual services can be deployed without a lot of coordination between teams, which encourages frequent updates. A microservice architecture is more complex to build and manage than either N-tier or web-queue-worker. It requires a mature development and DevOps culture. But done right, this style can lead to higher release velocity, faster innovation, and a more resilient architecture.

##### Event-driven architecture

Event-Driven Architecture use a publish-subscribe (pub-sub) model, where producers publish events, and consumers to them. The producers are independent from the consumers, and consumers are independent from each other.

Consider an event-driven architecture for applications that ingest and process a large volume of data with very low latency, such as IoT solutions. The style is also useful when different subsystems must perform different types of processing on the same event data.

##### Big Data, Big Compute

Big Data and Big Compute are specialized architecture styles for workloads that fit certain specific profiles. Big data divides a very large dataset into chunks, performing parallel processing across the entire set, for analysis and reporting. Big compute, also called high-performance computing (HPC), makes parallel computations across a large number (thousands) of cores. Domains include simulations, modeling, and 3-D rendering.

#### Architecture styles as constraints

An architecture style places constraints on the design, including the set of elements that can appear and the allowed relationships between those elements. Constraints guide the "shape" of an architecture by restricting the universe of choices. When an architecture conforms to the constraints of a particular style, certain desirable properties emerge.

For example, the constraints in microservices include:

1. A service represents a single responsibility.
2. Every service is independent of the others.
3. Data is private to the service that owns it. Services do not share data.

By adhering to these constraints, what emerges is a system where services can be deployed independently, faults are isolated, frequent updates are possible, and it's easy to introduce new technologies into the application.

Before choosing an architecture style, make sure that you understand the underlying principles and constraints of that style. Otherwise, you can end up with a design that conforms to the style at a superficial level, but does not achieve the full potential of that style. It's also important to be pragmatic. Sometimes it's better to relax a constraint, rather than insist on architectural purity.

The following table summarizes how each style manages dependencies, and the types of domain that are best suited for each.

|    Architecture style     |                    Dependency management                     |                         Domain type                          |
| :-----------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|          N-tier           |              Horizontal tiers divided by subnet              |  Traditional business domain. Frequency of updates is low.   |
|     Web-Queue-Worker      |    Front and backend jobs, decoupled by async messaging.     | Relatively simple domain with some resource intensive tasks. |
|       Microservices       | Vertically(functionally) decomposed services that call each other through APIs. |            Complicated domain. Frequent updates.             |
| Event-driven architecture |     Producer/consumer. Independent view per sub-system.      |                  IoT and real-time systems                   |
|         Big data          | Divide a huge dataset into small chunks. Parallel processing on local datasets. | Batch and real-time data analysis. Predictive analysis using ML. |
|        Big compute        |            Data allocation to thousands of cores.            |        Compute intensive domains such as simulation.         |

#### Consider challenges and benefits

Constraints also create challenges, so it's important to understand the trade-offs when adopting any of these styles. Do the benefits of the architecture style outweigh the challenges, for *this subdomain and bounded context*.

Here are some of the types of challenges to consider when selecting an architecture style:

* **Complexity**: Is the complexity of the architecture justified for your domain? Conversely, is the style too simplistic for your domain? In that case, you risk ending up with a "[big ball of mud](https://en.wikipedia.org/wiki/Big_ball_of_mud)", because the architecture does not help you to manage dependencies cleanly.
* **Asynchronous messaging and eventual consistency**. Asynchronous messaging can be used to decouple services, and increase reliability (because messages can be retried) and scalability. However, this also creates challenges in handling eventual consistency, as well as the possibility of duplicate messages.
* **Inter-service communication**. As you decompose an application into separate services, there is a risk that communication between services will cause unacceptable latency or create network congestion (for example, in a microservices architecture).
* **Manageability**. How hard is it to manage the application, monitor, deploy updates, and so on?

### [1-2-2 Big compute](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/big-compute)

The term *big compute* describes large-scale workloads that require a large number of cores, often numbering in the hundreds or thousands. Scenarios include image rendering, fluid dynamics, financial risk modeling, oil exploration, drug design, and engineering stress analysis, among others.

![](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/images/big-compute-logical.png)

Here are some typical characteristics of big compute applications:

* The work can be split into discrete tasks, which can be run across many cores simultaneously.
* Each task is finite. It takes some input, does some processing, and produces output. The entire application runs for a finite amount of time (minutes to days). A common pattern is to provision[^1-2-2-1] a large number of cores in a burst[^1-2-2-2], and then spin[^1-2-2-3] down to zero once the application completes.
* The application does not need to stay up 24/7. However, the system must handle node failures or application crashes.
* For some applications, tasks are independent and can run in parallel. In other cases, tasks are tightly coupled, meaning they must interact or exchange intermediate results. In that case, consider using high-speed networking technologies such as InfiniBand and remote direct memory access (RDMA).
* Depending on your workload, you might use compute-intensive VM sizes (H 16r, H16mr, and A9).

#### When to use this architecture

* Computationally intensive operations such as simulation and number crunching[^1-2-2-4].
* Simulations that are computationally intensive and must be split across CPUs in multiple computers (10-1000s).
* Simulations that require too much memory for one computer, and must be split across multiple computers.
* Long-running computations that would take too long to complete on a single computer.
* Smaller computations that must be run 100s or 1000s of times, such as Monte Carlo simulations.

#### Benefits

* High performance with "[embarrassingly parallel](https://en.wikipedia.org/wiki/Embarrassingly_parallel)" processing.
* Can harness[^1-2-2-5] hundreds or thousands of computer cores to solve large problems faster.
* Access to specialized high-performance hardware, with dedicated high-speed InfiniBand networks.
* You can provision VMs as needed to do work, and then tear them down.

#### Challenges

* Managing the VM infrastructure.
* Managing the volume of number crunching.
* Provisioning thousands of cores in a timely manner.
* For tightly coupled tasks, adding more cores can have diminishing returns. You may need to experiment to find the optimum number of cores.

#### Big compute using Azure Batch

[Azure Batch](https://docs.microsoft.com/en-us/azure/batch) is a managed service for running large-scale high-performance computing (HPC) applications.

Using Azure Batch, you configure a VM tool, and upload the applications and data files. Then the Batch service provisions the VMs, assign tasks to the VMs, runs the tasks, and monitors the progress. Batch can automatically scale out the VMs in response to the workload. Batch also provides job scheduling.

![](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/images/big-compute-batch.png)

#### Big compute running on Virtual Machines

You can use [Microsoft HPC Pack](https://docs.microsoft.com/en-us/powershell/high-performance-computing/overview?view=hpc19-ps&preserve-view=true) to administer a cluster of VMs, and schedule and monitor HPC jobs. With this approach, you must provision and manage the VMs and network infrastructure. Consider this approach if you have existing HPC workloads and want to move some or all it to Azure. You can move the entire HPC cluster to Azure, or you can keep your HPC cluster on-premises but use Azure for burst capacity.  For more information, see [Batch and HPC solutions for large-scale computing workloads](https://docs.microsoft.com/en-us/azure/architecture/topics/high-performance-computing).

#### HPC Pack deployed to Azure

In this scenario, the HPC cluster is created entirely within Azure.

![](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/images/big-compute-iaas.png)

The head node provides management and job scheduling services to the cluster. For tightly coupled tasks, use an RDMA network that provides very high bandwidth, low latency communication between VMs. For more information, see [Deploy an HPC Pack 2016 cluster in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/hpcpack-2016-cluster).

#### Burst an HPC cluster to Azure

In this scenario, an organization is running HPC Pack on-premises, and uses Azure VMs for burst capacity. The cluster head node is on-premises. ExpressRoute or VPN Gateway connects the on-premises network to the Azure VNet.

![](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/images/big-compute-hybrid.png)

#### Next steps

* [Choose an Azure compute service for your application](https://docs.microsoft.com/en-us/azure/architecture/guide/technology-choices/compute-decision-tree)
* [High Performance Computing (HPC) on Azure](https://docs.microsoft.com/en-us/azure/architecture/topics/high-performance-computing)
* [HPC cluster deployed in the cloud](https://docs.microsoft.com/en-us/azure/architecture/solution-ideas/articles/hpc-cluster)

### [1-2-3 Big data](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/big-data)

A big data architecture is designed to handle the ingestion[^1-2-3-1], processing, and analysis of data that is too large or complex for traditional database systems.

![](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/images/big-data-logical.svg)

Big data solutions typically involve one or more of the following types of workload:

* Batch processing of big data sources at rest[^1-2-3-2].
* Real-time processing of big data in motion.
* Interactive exploration of big data.
* Predictive analytics and machine learning.

Most big data architectures include some or all of the following components:

* <u>Data sources</u>: All big data solutions start with one or more data sources. Examples include:
  * Application data stores, such as relational databases.
  * Static files produces by applications, such as web server log files.
  * Real-time data sources, such as IoT devices.
* <u>Data storage</u>: Data for batch processing operations is typically stored in a distributed file store that can hold high volumes of large files in various formats. This kind of store is often called a *data lake*. Options for implementing this storage include Azure Data Lake Store or blob containers in Azure Storage.
* <u>Batch processing</u>: Because the data sets are so large, often a big data solution must process data files using long-running batch jobs to filter, aggregate, and otherwise prepare the data for analysis. Usually these jobs involve reading source files, processing them, and writing the output to new files. Options include running U-SQL jobs in Azure Data Lake Analytics, using Hive, Pig, or custom Map/Reduce jobs in an HDInsight Hadoop cluster, or using Java, Scala, or Python programs in an HDInsight Spark cluster.
* <u>Real-time message ingestion</u>: If the solution includes real-time sources, the architecture must include a way to capture and store real-time messages for stream processing. This might be a simple data store, where incoming messages are dropped into a folder for processing. However, many solutions need a message ingestion store to act as a buffer for messages, and to support scale-out processing, reliable delivery, and other message queuing semantics. Options include Azure Event Hubs, Azure IoT Hubs, and Kafka.
* <u>Stream processing</u>: After capturing real-time messages, the solution must process them by filtering, aggregating, and otherwise preparing the data for analysis. The processed stream data is then written to an output sink. Azure Stream Analytics provides a managed stream processing service based on perpetually running SQL queries that operate on unbounded streams. You can also use open source Apache streaming technologies like Storm and Spark Streaming in an HDInsight cluster.
* <u>Analytical data store</u>: Many big data solutions prepare data for analysis and then serve the processed data in a structured format that can be queried using analytical tools. The analytical data store used to serve these queries can be a Kimball-style relational data warehouse, as seen in most traditional business intelligence (BI) solutions. Alternatively, the data could be presented through a low-latency NoSQL technology such as HBase, or an interactive Hive database that provides a metadata abstraction over data files in the distributed data store. Azure Synapse Analytics provides a managed service for large-scale, cloud-based data warehousing. HDInsight supports Interactive Hive, HBase, and Spark SQL, which can also be used to serve data for analysis.
* <u>Analysis and reporting</u>: The goal of most big data solutions is to provide insights into the data through analysis and reporting. To empower[^1-2-3-3] users to analyze the data, the architecture may include a data modeling layer, such as a multidimensional OLAP cube or tabular data model in Azure Analysis Services. It might also support self-service BI, using the modeling and visualization technologies in Microsoft Power BI or Microsoft Excel. Analysis and reporting can also take the form of interactive data exploration by data scientists or data analyst. For these scenarios, many Azure services support analytical notebooks, such as Jupyter, enabling these users to leverage their existing skills with Python or R. For large-scale data exploration, you can use Microsoft R Server, either standalone or with Spark.
* <u>Orchestration</u>: Most big data solutions consist of repeated data processing operations, encapsulated in workflows, that transform source data, move data between multiple sources and sinks, load the processed data into an analytical data store, or push the results straight to a report or dashboard. To automate these workflows, you can use an orchestration technology such Azure Data Factory or Apache Oozie and Sqoop.

Azure includes many services that can be used in a big data architecture. They fall roughly into two categories:

* Managed services, including Azure Data Lake Store, Azure Data Lake Analytics, Azure Synapse Analytics, Azure Stream Analytics, Azure Event Hub, Azure IoT Hub, and Azure Data Factory.
* Open source technologies based on the Apache Hadoop platform, including HDFS, HBase, Hive, Pig, Spark, Storm, Oozie, Sqoop, and Kafka. These technologies are available on Azure in the Azure HDInsight service.

These options are not mutually exclusive, and many solutions combine open source technologies with Azure services.

#### When to use this architecture

Consider this architecture style when you need to:

* Store and process data in volumes too large for a traditional database.
* Transform unstructured data for analysis and reporting.
* Capture, process, and analyze unbounded streams of data in real time, or with low latency.
* Use Azure Machine Learning or Microsoft Cognitive Services.

#### Benefits

* <u>Technology choices</u>. You can mix and match Azure managed services and Apache technologies in HDInsight clusters, to capitalize on existing skills or technology investments.
* <u>Performance through parallelism</u>. Big data solutions take advantage of parallelism, enabling high-performance solutions that scale to large volumes of data.
* <u>Elastic scale</u>. All of the components in the big data architecture support scale-out provisioning, so that you can adjust your solution to small or large workloads, and pay only for the resources that you use.
* <u>Interoperability[^1-2-3-4] with existing solutions</u>. The components of the big data architecture are also used for IoT processing and enterprise BI solutions, enabling you to create an integrated solution across data workloads.

#### Challenges

* <u>Complexity</u>. Big data solutions can be extremely complex, with numerous components to handle data ingestion from multiple data sources. It can be challenging to build, test, and troubleshoot big data processes. Moreover, there may be a large number of configuration settings across multiple systems that must be used in order to optimize performance.
* <u>Skillset</u>. Many big data technologies are highly specialized, and use frameworks and languages that are not typical of more general application architectures. On the other hand, big data technologies are evolving new APIs that build on more established languages. For example, the U-SQL language in Azure Data Lake Analytics is based on a combination of Transact-SQL and C#. Similarly, SQL-based APIs are available for Hive, HBase, and Spark.
* <u>Technology maturity</u>[^1-2-3-5]. Many of the technologies used in big data are evolving. While core Hadoop technologies such as Hive and Pig have stabilized, emerging technologies such as Spark introduce extensive changes and enhancements with each new release. Managed services such as Azure Data Lake Analytics and Azure Data Factory are relatively young, compared with other Azure services, and will likely evolve over time.
* <u>Security</u>. Big data solutions usually rely on storing all static data in a centralized data lake. Securing access to this data can be challenging, especially when the data must be ingested and consumed by multiple applications and platforms.

#### Best practices

* <u>Leverage parallelism</u>. Most big data processing technologies distribute the workload across multiple processing units. This requires that static data files are created and stored in a splittable format. Distributed file systems such as HDFS can optimize read and write performance, and the actual processing is performed by multiple cluster nodes in parallel, which reduces overall job times.
* <u>Partition data</u>. Batch processing usually happens on a recurring schedule -- for example, weekly or monthly. Partition data files, and data structures such as tables, based on temporal periods that match the processing schedule. That simplifies data ingestion and job scheduling, and makes it easier to troubleshoot failures. Also, partitioning tables that are used in Hive, U-SQL, or SQL queries can significantly improve query performance.
* <u>Apply schema-on-read semantics</u>. Using a data lake lets you to combine storage for files in multiple formats, whether structured, semi-structured, or unstructured. Use *schema-on-read* semantics, which project a schema onto the data when the data is processing, not when the data is stored. This builds flexibility into the solution, and prevents bottlenecks during data ingestion caused by data validation and type checking.
* <u>Process data in-place</u>. Traditional BI solutions often use an extract, transform, and load (ETL) process to move data into a data warehouse. With larger volumes data, and a greater variety of formats, big data solutions generally use variations of ETL, such as transform, extract, and load (TEL). With this approach, the data is processed within the distributed data store, transforming it to the required structure, before moving the transformed data into an analytical data store.
* <u>Balance utilization and time costs</u>. For batch processing jobs, it's important to consider two factors: The per-unit cost of the compute nodes, and the per-minute cost of using those nodes to complete the job. For example, a batch job may take eight hours with four cluster nodes. However, it might turn out that the job uses all four nodes only during the first two hours, and after that, only two nodes are required. In that case, running the entire job on two nodes would increase the total job time, but would not double it, so the total cost would be less. In some business scenarios, a longer processing time may be preferable to the higher cost of using underutilized cluster resources.
* <u>Separate cluster resources</u>. When deploying HDInsight clusters, you will normally achieve better performance by provisioning separate cluster resources for each type of workload. For example, although Spark clusters include Hive, if you need to perform extensive processing with both Hive and Spark, you should consider deploying separate dedicated Spark and Hadoop clusters. Similarly, if you are using HBase and Storm for low latency stream processing and Hive for batch processing, consider separate clusters for Storm, HBase, and Hadoop.
* <u>Orchestrate[^1-2-3-6] data ingestion</u>. In some cases, existing business applications may write data files for batch processing directly into Azure storage blob containers, where they can be consumed by HDInsight or Azure Data Lake Analytics. However, you will often need to orchestrate the ingestion of data from on-premises or external data sources into the data lake. Use an orchestration workflow or pipeline, such as those supported by Azure Data Factory or Oozie[^1-2-3-7], to achieve this in a predictable and centrally manageable fashion.
* <u>Scrub[^1-2-3-8] sensitive data early</u>. The data ingestion workflow should scrub sensitive data early in the process, to avoid storing it in the data lake.

#### IoT architecture

Internet of Things (IoT) is a specialized subset of big data solutions. The following diagram shows a possible logical architecture for IoT. The diagram emphasizes the event-streaming components of the architecture.

![](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/images/iot.png)

The <u>cloud gateway</u> ingests device events at the cloud boundary, using a reliable, low latency messaging system.

Devices might send events directly to the cloud gateway, or through a <u>field gateway</u>. A field gateway is a specialized device or software, usually colocated with the devices, that receives events and forwards them to the cloud gateway. The field gateway might also preprocess the raw device events, performing functions such as filtering, aggregation, or protocol transformation.

After ingestion, events go through one or more <u>stream processors</u> that can route the data (for example, to storage) or perform analytics and other processing.

The following are some common types of processing. (This list is certainly not exhausive.)

* Writing event data to cold storage, for archiving or batch analytics.
* Hot path analytics, analyzing the event stream in (near) real time, to detect anomalies, recognize patterns over rolling time windows, or trigger alerts when a specific condition occurs in the stream.
* Handling special types of non-telemetry messages from devices, such as notifications and alarms.
* Machine learning.

The boxes that are shaded gray show components of an IoT system that are not directly related to event streaming, but are included here for completeness.

* The <u>device registry</u> is a database of the provisioned devices, including the device IDs and usually device metadata, such as location.
* The <u>provisioning API</u> is a common external interface for provisioning and registering new devices.
* Some IoT solutions allow <u>command and control messages</u> to be sent to devices.

> This section has presented a very high-level view of IoT, and there are many subtleties[^1-2-3-9]  and challenges to consider. For a more detailed reference architecture and discussion, see the [Microsoft Azure IoT Reference Architecture](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available).

#### Next steps

* Learn more about [big data architectures](https://docs.microsoft.com/en-us/azure/architecture/data-guide/big-data/).
* Learn more about [IoT solutions](https://docs.microsoft.com/en-us/azure/architecture/example-scenario/iot/introduction-to-solutions).

### [1-2-4 Event-driven architecture](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/event-driven)



### [1-2-5 Microservices](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/microservices)



### [1-2-6 N-tier application](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/n-tier)



### [1-2-7 Web-queue-worker](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/web-queue-worker)



# 2 Design Patterns

## [2-1 Overview]((https://docs.microsoft.com/en-us/azure/architecture/patterns/))



## 2-2 Categories

### [2-2-1 Data management](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/data-management)



### [2-2-2 Design and implementation](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/design-implementation)



### [2-2-3 Messaging](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/messaging)



## [2-3 Priority Queue](https://docs.microsoft.com/en-us/azure/architecture/patterns/priority-queue)



## [2-4 Publisher-Subscriber](https://docs.microsoft.com/en-us/azure/architecture/patterns/publisher-subscriber)

Enable an application to announce events to multiple interested consumers asynchronously, ==without coupling the senders to the receivers==.

**Also called**: Pub/sub messaging

### 2-4-1 Context and problem

In cloud-based and distributed applications, components of the system often need to provide information to other components as events happen.

==Asynchronous messaging is an effective way to decouple senders from consumers, and avoid blocking the sender to wait for a response.== However, using a dedicated message queue for each consumer does not effectively scale to many consumers. Also, some of the consumers might be interested in only a subset of the information. How can the sender announce events to all interested consumers without knowing their identities?

### 2-4-2 Solution

Introduce an asynchronous messaging subsystem that includes the following:

* An input messaging channel used by the sender. The sender packages events into messages, using a known message format, and sends these messages via the input channel. The sender in this pattern is also called the *publisher*.[^1]
* One output messaging channel per consumer. The consumers are known as *subscribers*.
* A mechanism for copying each message from the input channel to the output channels for all subscribers interested in that message. This operation is typically handled by an intermediary[^2] such as a message broker[^3] or event bus.

The following diagram shows the logical components of this pattern:

![](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/publish-subscribe.png)

Pub/sub messaging has the following benefits:

* It ==decouples subsystems that still need to communicate==. Subsystems can be managed independently, and messages can be properly managed even if one or more receivers are offline.
* It increases scalability and ==improves responsiveness of the sender==. The sender can quickly send a single message to the input channel, then return to its core processing responsibilities. The messaging infrastructure is responsible for ensuring messages are delivered to the interested subscribers.
* It improves reliability. Asynchronous messaging helps applications continue to run smoothly under increased loads and handle intermittent failures more effectively.
* It ==allows for deferred or scheduled processing==. Subscribers can wait to pick up messages until off-peak hours, or messages can be routed or processed according to a specific schedule.
* It ==enables simpler integration between systems== using different platforms, programming languages, or communication protocols, as well as between on-premises systems and applications running in the cloud.
* It facilitates asynchronous workflows across an enterprise.
* It ==improves testability==. Channels can be monitored and messages can be inspected or logged as part of an overall integration test strategy.
* It provides ==separation of concerns for your applications==. Each application can focus on its core capabilities, while the messaging infrastructure handles everything required to reliably route messages to multiple consumers. 

### 2-4-3 Issues and considerations

Consider the following points when deciding how to implement this pattern:

* **Existing technologies**. It is strongly recommended to use available messaging products and services that support a publish-subscribe model, rather than building your own. In Azure, consider using <u>Service Bus</u>, <u>Event Hubs</u> or <u>Event Grid</u>. Other technologies that can be used for pub/sub messaging include Redis, RabbitMQ, and Apache Kafka.
* **Subscription handling**. The messaging infrastructure must provide mechanisms that consumers can use to subscribe to or unsubscribe from available channels.
* **Security**.  Connecting to any message channel must be restricted by security policy to prevent eavesdropping[^4] by unauthorized users or applications.
* **Subsets of messages**. Subscribers are usually only interested in subset of the messages distributed by a publisher. Messaging services often allow subscribers to narrow the set of messages received by:
  * **Topics**. Each topic has a dedicated output channel, and each consumer can subscribe to all relevant topics.
  * **Content filtering**. ==Messages are inspected and distributed based on the content of each message.== Each subscriber can specify the content it is interested in.
* **Wildcard subscribers**. Consider allowing subscribers to subscribe to multiple topics via wildcards.
* **Bi-directional communication**. The channels in a publish-subscribe system are treated as unidirectional[^5]. If a specific subscriber needs to send acknowledgement or communicate status back to the publisher, consider using the [Request/Reply Pattern](http://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html). This pattern uses one channel to send a message to the subscriber, and a separate reply channel for communicating back to the publisher.
* **Message ordering**. The order in which consumer instances receive messages isn't guaranteed, and doesn't necessarily reflect the order in which the messages were created. Design the system to ensure that message processing is idempotent[^6]to help eliminate any dependency on the order of message handling.
* **Message priority**. Some solutions may require that messages are processed in a specific order. The [Priority Queue pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/priority-queue) provides a mechanism for ensuring specific messages are delivered before others.
* **Poison messages**. A malformed[^7]message, or a task that requires access to resources that aren't available, can cause a service instance to fail. ==The system should prevent such messages being returned to the queue.== Instead, capture and store the details of these messages elsewhere so that they can be analyzed if necessary.
* **Repeated messages**. The same message might be sent more than once. For example, the sender might fail after posting a message. Then a new instance of the sender might start up and repeat the message. The messaging infrastructure should implement ==duplicate message detection and removal== (also known as de-duping[^8]) based on message IDs in order to provide at-most-once delivery of messages.
* **Message expiration**. A message might have a limited lifetime. If it isn't processed within this period, it might no longer be relevant and should be discarded. A sender can specify an expiration time as part of the data in the message. A receiver can examine this information before deciding whether to perform the business logic associated with the message.
* **Message scheduling**. A message might be temporarily embargoed[^9] and should not be processed until a specific date and time. The message should not be available to a receiver until this time.

### 2-4-4 When to use this pattern

Use this pattern when:

* An application needs to broadcast information to a significant number of consumers.
* An application needs to communicate with one or more independently-developed applications or services, which may use different platforms, programming languages, and communication protocols.
* An application can send information to consumers without requiring real-time responses from the consumers.
* The systems being integrated are designed to support an eventual consistency model for their data.
* An application needs to communicate information to multiple consumers, which may have different availability requirements or uptime[^10] schedules than the sender.

This pattern might not be useful when:

* An application has only a few consumers who need significantly different information from the producing application.
* An application requires near real-time interaction with consumers.

### 2-4-5 Example

The following diagram shows enterprise integration architecture that uses Service Bus to coordinate workflows, and Event Grid to notify subsystems of events that occur. For more information, see [Enterprise integration on Azure using message queues and events](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/enterprise-integration/queues-events).

![Enterprise integration architecture](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/enterprise-integration/_images/enterprise-integration-queues-events.png)

### 2-4-6 Next steps

The following guidance might be relevant when implementing this pattern:

* [Choose between Azure services that deliver messages](https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services).
* The [Event-driven architecture style](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/event-driven) is an architecture style that uses pub/sub messaging.
* [Asynchronous Messaging Primer[^11]](https://docs.microsoft.com/en-us/previous-versions/msp-n-p/dn589781(v=pandp.10)). Message queues are an asynchronous communications mechanism. If a consumer service needs to send a reply to an application, it might be necessary to implement some form of response messaging. The Asynchronous Messaging Primer provides information on how to implement request/reply messaging using message queues.

### 2-4-1-7 Related guidance

The following patterns might be relevant when implementing this pattern:

* [Observer pattern](https://en.wikipedia.org/wiki/Observer_pattern). The Publish-Subscribe pattern builds on the Observer pattern by decoupling subjects from observers via asynchronous messaging.
* [Message Broker pattern](https://en.wikipedia.org/wiki/Message_broker). Many messaging subsystems that support a publish-subscribe model are implemented via a message broker.

# 3 Miscellaneous

## [3-1 Request/Reply Pattern](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html)





## [3-2 Observer pattern](https://en.wikipedia.org/wiki/Observer_pattern)



## [3-3 Message Broker pattern](https://en.wikipedia.org/wiki/Message_broker)



# 4 Azure categories

## 4-1 Integration

### 4-1-1 Architectures

#### [4-1-1-1-1 Enterprise integration - queues and events](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/enterprise-integration/queues-events)



[^1]: A *message* is a packet of data. An *event* is a message that notifies other components about a change or an action that has taken place.

[^2]: [ˌɪntərˈmiːdieri], n.中间人; 媒介; 调解人; 中间阶段
[^3]: [ˈbroʊkər], n.经纪人，代理商；v.协调，安排
[^4]: [ˈivzdrɑp] vi.偷听（别人的谈话）
[^5]: [ˌjunɪdə'rekʃənəl] adj.单向的，单向性的
[^6]: [aɪ'dempətənt], n.幂等，等幂；adj.幂等的
[^7]: [ˌmælˈfɔrmd], adj. 难看的，畸形的
[^8]: De-deduping software, also know as dedupe or deduplication software, is a **software application that will help to identify and remove duplicated records from lists and databases**. This process is commonly used to help prevent duplicated names and addresses.
[^9]: [ɪmˈbɑːrɡoʊ], n.禁运（令；v.禁运（货物）
[^10]: n.（计算机等的）正常运行时间
[^11]: [ˈprɪmɚ] n.底漆; 启蒙读本; 入门书





[^1-2-2-1]: vt.& vi.为…提供所需物品（尤指食物） to provide someone or something with a lot of food and supplies, especially for a journey
[^1-2-2-2]: /bɜːrst/ v.（使）爆裂，爆炸; 冲，闯; 突然出现; 充满  n.爆裂，爆炸; 裂口; 情感爆发 if something bursts, or if you burst it, it breaks open or apart suddenly and violently so that its contents come out
[^1-2-2-3]: v.使旋转; 急转弯; 疾驰; 甩干衣服; 有倾向性陈述; 纺纱; 吐丝 to turn around and around very quickly, or to make something do this
[^1-2-2-4]: v.嘎吱嘎吱地嚼/行进; （使）嘎吱作响; （用计算器等大量）处理（数字）**crunch (the) numbers** to do very complicated calculations on large amounts of data (=information stored on a computer) in order to find out about something
[^1-2-2-5]: n.马具，挽具; 背带; 系带 vt.利用; 给（马等）套轭具; 控制 to control and use the natural force or power of something
[^1-2-3-1]: n.摄取; 采食 ingest vt. to take food or other substances into your body
[^1-2-3-2]: 静止,不动; 安息 a) an expression meaning dead, and free from pain and problems b) *technical* not moving
[^1-2-3-3]: vt.授权; 准许; 使能够; 使控制局势 to give a person or an organization the power or the legal right to do something
[^1-2-3-4]: n.互用性，协同工作的能力
[^1-2-3-5]: /məˈtʃʊərəti $ -ˈtʃʊr-/ n.成熟; 完备; （票据等的）到期; [地]壮年期 the time or state when someone or something is fully grown or developed
[^1-2-3-6]: vt.把（乐曲）编成管弦乐; 和谐地安排; 精心策划 *written* to organize an important event or a complicated plan, especially secretly
[^1-2-3-7]: ['uzɪ] n.（缅甸的）驯象人，驭象者
[^1-2-3-8]: vt.& vi.用力擦洗，刷洗; 取消（原有安排）to rub something hard, especially with a stiff brush, in order to clean it
[^1-2-3-9]: /ˈsʌtlti/ n.精妙，巧妙; 敏锐，敏感; 狡猾，阴险; 细微的差别等 a thought, idea, or detail that is important but difficult to notice or understand
