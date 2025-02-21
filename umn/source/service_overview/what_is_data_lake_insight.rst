:original_name: dli_01_0378.html

.. _dli_01_0378:

What Is Data Lake Insight
=========================

DLI Introduction
----------------

Data Lake Insight (DLI) is a serverless data processing and analysis service fully compatible with `Apache Spark <https://spark.apache.org/>`__ and `Apache Flink <https://flink.apache.org/>`__ ecosystems. It frees you from managing any servers.

DLI supports standard SQL and is compatible with Spark SQL and Flink SQL. It also supports multiple access modes, and is compatible with mainstream data formats. DLI supports SQL statements and Spark applications for heterogeneous data sources, including CloudTable, RDS, GaussDB(DWS), CSS, OBS, custom databases on ECSs, and offline databases.

Functions
---------

You can query and analyze heterogeneous data sources such as RDS, and GaussDB(DWS) on the cloud using access methods, such as visualized interface, RESTful API, JDBC, and Beeline. The data format is compatible with five mainstream data formats: CSV, JSON, Parquet, and ORC.

-  Basic functions

   -  You can use standard SQL statements to query in SQL jobs.
   -  Flink jobs support Flink SQL online analysis capabilities: supporting aggregation functions such as Window and Join, using SQL to express service logic, and achieving service implementation conveniently and quickly.
   -  For spark jobs, fully-managed Spark computing can be performed. You can submit computing tasks through interactive sessions or in batch to analyze data in the fully managed Spark queues.

-  Federated analysis of heterogeneous data sources

   -  Spark datasource connection: Data sources such as GaussDB(DWS), RDS, and CSS can be accessed through DLI.
   -  Interconnection with multiple cloud services is supported in Flink jobs to form a rich stream ecosystem. The DLI stream ecosystem consists of cloud service ecosystems and open source ecosystems.

      -  Cloud service ecosystem: DLI can interconnect with other services in Flink SQL. You can directly use SQL to read and write data from cloud services.
      -  Open-source ecosystem: By establishing network connections with other VPCs through enhanced datasource connections, you can access all Flink and Spark-supported data sources and output sources, such as Kafka, Hbase, Elasticsearch, in the tenant-authorized DLI queues.

-  Storage-compute decoupling

   DLI is interconnected with OBS for data analysis. In this architecture where storage and compute are decoupled, resources of these two types are charged separately, helping you reduce costs and improving resource utilization.

   You can choose single-AZ or multi-AZ storage when you create an OBS bucket for storing redundant data on the DLI console. The differences between the two storage policies are as follows:

   -  Multi-AZ storage means data is stored in multiple AZs, improving data reliability. If the multi-AZ storage is enabled for a bucket, data is stored in multiple AZs in the same region. If one AZ becomes unavailable, data can still be properly accessed from the other AZs. The multi-AZ storage is ideal for scenarios that demand high reliability. You are advised to use this policy.
   -  Single-AZ storage means that data is stored in a single AZ, with lower costs.

-  Elastic resource pool

   The backend of elastic resource pools adopts a CCE cluster architecture, supporting heterogeneous resources, so you can manage and schedule resources in a unified manner.

   For details, see :ref:`Creating an Elastic Resource Pool and Queues Within It <dli_01_0508>`.

   Elastic resource pools have the following advantages:

   -  **Unified management**

      -  You can manage multiple internal clusters and schedule jobs. You can manage millions of cores for compute resources.
      -  Elastic resource pools can be deployed across multiple AZs to support high availability.

   -  **Tenant resource isolation**

      Resources of different queues are isolated to reduce the impact on each other.

   -  **Shared access and flexibility**

      -  Minute-level scaling helps you to handle request peaks.
      -  Queue priorities and CU quotas can be set at different time to improve resource utilization.

   -  **Job-level isolation (supported in later versions)**

      SQL jobs can run on independent Spark instances, reducing mutual impacts between jobs.

   -  **Automatic scaling (supported in later versions)**

      The queue quota is updated in real time based on workload and priority.

   Using elastic resource pools has the following advantages.

   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | Advantage             | No Elastic Resource Pool                                                                                                                       | Use Elastic Resource Pool                                                                                                                        |
   +=======================+================================================================================================================================================+==================================================================================================================================================+
   | Efficiency            | You need to set scaling tasks repeatedly to improve the resource utilization.                                                                  | Dynamic scaling can be done in seconds.                                                                                                          |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | Resource utilization  | Resources cannot be shared among different queues.                                                                                             | Queues added to the same elastic resource pool can share compute resources.                                                                      |
   |                       |                                                                                                                                                |                                                                                                                                                  |
   |                       | For example, a queue has idle CUs and another queue is heavily loaded. Resources cannot be shared. You can only scale up the second queue.     |                                                                                                                                                  |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | When you set a data source, you must allocate different network segments to each queue, which requires a large number of VPC network segments. | You can add multiple general-purpose queues in the same elastic resource pool to one network segment, simplifying the data source configuration. |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | Resource allocation   | If resources are insufficient for scale-out tasks of multiple queues, some queues will fail to be scaled out.                                  | You can set the priority for each queue in the elastic resource pool based on the peak hours to ensure proper resource allocation.               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+

DLI Core Engine: Spark+Flink+Trino
----------------------------------

-  Spark is a unified analysis engine that is ideal for large-scale data processing. It focuses on query, compute, and analysis. DLI optimizes performance and reconstructs services based on open-source Spark. It is compatible with the Apache Spark ecosystem and interfaces, and improves performance by 2.5x when compared with open-source Spark. In this way, DLI enables you to perform query and analysis of EB's of data within hours.
-  Flink is a distributed compute engine that is ideal for batch processing, that is, for processing static data sets and historical data sets. You can also use it for stream processing, that is, processing real-time data streams and generating data results in real time. DLI enhances features and security based on the open-source Flink and provides the Stream SQL feature required for data processing.

Serverless Architecture
-----------------------

DLI is a serverless big data query and analysis service. It has the following advantages:

-  Auto scaling: DLI ensures you always have enough capacity on hand to deal with any traffic spikes.

Accessing DLI
-------------

A web-based service management platform is provided. You can access DLI using the management console or HTTPS-based APIs, or connect to the DLI server through the JDBC client.

-  Using the management console

   You can submit SQL, Spark, or Flink jobs on the DLI management console.

   Log in to the management console and choose **Data Analysis** > **Data Lake Insight**.

-  Using APIs

   If you need to integrate DLI into a third-party system for secondary development, you can call DLI APIs to use the service.

   For details, see `Data Lake Insight API Reference <https://docs.otc.t-systems.com/data-lake-insight/api-ref/>`__.

-  DataArts Studio

   DataArts Studio is a one-stop data operations platform that provides intelligent data lifecycle management. It supports intelligent construction of industrial knowledge libraries and incorporates data foundations such as big data storage, computing, and analysis engines. With DataArts Studio, your company can easily construct end-to-end intelligent data systems. These systems can help eliminate data silos, unify data standards, accelerate data monetization, and promote digital transformation.

   Create a data connection on the DataArts Studio management console to access DLI for data analysis.
