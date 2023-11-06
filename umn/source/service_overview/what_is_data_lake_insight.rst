:original_name: dli_01_0378.html

.. _dli_01_0378:

What Is Data Lake Insight?
==========================

DLI Introduction
----------------

Data Lake Insight (DLI) is a serverless data processing and analysis service fully compatible with `Apache Spark <https://spark.apache.org/>`__ and `Apache Flink <https://flink.apache.org/>`__ ecosystems. It frees you from managing any servers.

DLI supports standard SQL and is compatible with Spark SQL and Flink SQL. It also supports multiple access modes, and is compatible with mainstream data formats. DLI supports SQL statements and Spark applications for heterogeneous data sources, including CloudTable, RDS, GaussDB(DWS), CSS, OBS, custom databases on ECSs, and offline databases.

Functions
---------

You can query and analyze heterogeneous data sources such as RDS, and GaussDB(DWS) on the cloud using access methods, such as visualized interface, RESTful API, JDBC, and Beeline. The data format is compatible with five mainstream data formats: CSV, JSON, Parquet, and ORC.

-  Basic functions

   -  You can use standard SQL statements to query in SQL jobs.
   -  Flink jobs support Flink SQL online analysis. Aggregation functions such as Window and Join, geographic functions, and CEP functions are supported. SQL is used to express service logic, facilitating service implementation.
   -  For spark jobs, fully-managed Spark computing can be performed. You can submit computing tasks through interactive sessions or in batch to analyze data in the fully managed Spark queues.

-  Federated analysis of heterogeneous data sources

   -  Spark datasource connection: Data sources such as DWS, RDS, and CSS can be accessed through DLI.
   -  Interconnection with multiple cloud services is supported in Flink jobs to form a rich stream ecosystem. The DLI stream ecosystem consists of cloud service ecosystems and open source ecosystems.

      -  Cloud service ecosystem: DLI can interconnect with other services in Flink SQL. You can directly use SQL to read and write data from cloud services.
      -  Open-source ecosystems: After connections to other VPCs are established through datasource connections, you can access all data sources and output targets (such as Kafka, HBase, and Elasticsearch) supported by Flink and Spark in your dedicated DLI queue.

-  Storage-compute decoupling

   DLI is interconnected with OBS for data analysis. In this architecture where storage and compute are decoupled, resources of these two types are charged separately, helping you reduce costs and improving resource utilization.

   You can choose single-AZ or multi-AZ storage when you create an OBS bucket for storing redundant data on the DLI console. The differences between the two storage policies are as follows:

   -  Multi-AZ storage means data is stored in multiple AZs, improving data reliability. If the multi-AZ storage is enabled for a bucket, data is stored in multiple AZs in the same region. If one AZ becomes unavailable, data can still be properly accessed from the other AZs. The multi-AZ storage is ideal for scenarios that demand high reliability. You are advised to use this policy.
   -  Single-AZ storage means that data is stored in a single AZ, with lower costs.

DLI Core Engine: Spark+Flink
----------------------------

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
