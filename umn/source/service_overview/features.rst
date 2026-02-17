:original_name: dli_01_0698.html

.. _dli_01_0698:

Features
========

This section outlines the key features supported by DLI. For detailed information on regional availability of each feature, you can refer to the console.

Elastic Resource Pools and Queues
---------------------------------

Before submitting jobs using DLI, you need to prepare the necessary compute resources.

-  **Elastic Resource Pool**

   An elastic resource pool provides the required compute resources (CPU and memory) for running DLI jobs. It offers robust computing power, high availability, and flexible resource management capabilities. This makes it ideal for large-scale computing tasks and business use cases that require long-term resource planning. Additionally, it can dynamically adapt to changing demands for compute resources.

-  **Queue**

   After creating an elastic resource pool, you can create multiple queues within it. Each queue is associated with specific jobs and data processing tasks, serving as the fundamental unit for allocating and utilizing resources in the pool. In other words, a queue represents the actual compute resources needed to execute a job.

   Within the same elastic resource pool, compute resources are shared among queues. By configuring appropriate allocation policies for these queues, you can optimize the utilization of compute resources.

-  **default Queue**

   DLI comes pre-configured with a default queue named **default**, where resources are allocated on-demand. If you are unsure about the required queue capacity or lack available space to create queues, you can use this **default** queue to run your jobs. However, since the **default** queue is shared among all users, there may be instances of resource contention. As a result, access to resources cannot always be guaranteed for every operation.

DLI Metadata Management
-----------------------

DLI metadata serves as the foundation for developing SQL jobs and Spark jobs. Before executing a job, you need to define databases and tables based on your business requirements.

In addition to managing its own metadata, DLI supports integration with LakeFormation for unified metadata management. This enables seamless connectivity with various compute engines and big data cloud services, making it efficient and convenient to build data lakes and operate related businesses.

-  **DLI Metadata**

   DLI metadata serves as the foundation for developing SQL jobs and Spark jobs. Prior to running a job, you must define databases and tables according to your specific use case. Data catalog: A data catalog is a metadata management object that can contain multiple databases. You can create and manage multiple catalogs in DLI to isolate different sets of metadata.

   -  Database

      A database is a structured repository built on computer storage devices used to organize, store, and manage data. It typically stores, retrieves, and manages structured data, consisting of multiple interrelated data tables connected through keys and indexes.

   -  Table

      Tables are one of the most critical components of a database, composed of rows and columns. Each row represents a data entry, while each column defines an attribute or characteristic of the data. Tables are used to organize and store specific types of data, enabling efficient query and analysis. While a database provides the framework, tables constitute its actual content—a single database may include one or more tables.

   -  Metadata

      Metadata refers to data that describes other data. It primarily includes information about the source, size, format, or other characteristics of the data itself. In the context of database fields, metadata helps interpret the contents of a data warehouse. When creating a table, metadata is defined by specifying three elements: column names, data types, and column descriptions.

-  **Connecting DLI to LakeFormation for Metadata Management**

   After creating an elastic resource pool, you can create multiple queues within it. Each queue is associated with specific jobs and data processing tasks, serving as the fundamental unit for allocating and utilizing resources in the pool. In other words, a queue represents the actual compute resources needed to execute a job. Within the same elastic resource pool, compute resources are shared among queues. By configuring appropriate allocation policies for these queues, you can optimize the utilization of compute resources.

DLI SQL Job
-----------

DLI SQL jobs, also known as DLI Spark SQL jobs, allow you to execute data queries and other operations by executing SQL statements in the SQL editor. It supports SQL:2003 and is fully compatible with Spark SQL.

DLI Spark Job
-------------

Spark is a unified analytics engine designed for large-scale data processing, focusing on query, computation, and analysis. DLI has undergone extensive performance optimization and service-oriented enhancements over the open-source Spark, maintaining compatibility with the Apache Spark ecosystem and APIs while boosting performance by 2.5 times, enabling exabyte-scale data queries and analyses within hours.

DLI Flink Job
-------------

DLI Flink jobs are specifically designed for real-time data stream processing, making them ideal for scenarios that require low latency and quick response. They support cross-source connectivity with various cloud services, forming a robust streaming ecosystem. These jobs are ideal for applications such as real-time monitoring and online analytics.

-  **Flink OpenSource Job**

   DLI provides standard connectors and a rich set of APIs, facilitating seamless integration with other data systems.

-  **Flink Jar Job**

   You can submit Flink jobs compiled into JAR files, offering greater flexibility and customization capabilities. This option is well-suited for complex data processing scenarios requiring custom functions, user-defined functions (UDFs), or specific library integrations. Leveraging Flink's ecosystem, advanced stream processing logic and state management can be achieved.

-  **Flink Python Job**

   Starting from Flink 1.17, DLI introduces support for PyFlink jobs, providing you with more flexible and powerful data processing tools. You can directly submit PyFlink jobs through the DLI job management page. Additionally, it allows specifying third-party Python and Java dependencies, customizing Python virtual environments, and uploading compressed data files, significantly enhancing the convenience of job submission and execution flexibility.

   Additionally, the DLI Flink image comes pre-installed with default Python execution environments supporting versions 3.7, 3.8, 3.9, and 3.10, meeting diverse development needs. This empowers Python developers to efficiently use DLI for data processing and analysis tasks.

   Flink Python jobs are particularly suitable for scenarios involving customized stream processing logic, complex state management, or specific library integrations. You are required to write and build Python job packages independently. Before submitting a Flink Python job, upload the Jar job package to OBS and submit it along with the data and job parameters to execute the job.

Datasource Connection
---------------------

Before performing cross-source analysis using DLI, you need to establish a datasource connection to enable network communication between data sources.

DLI's enhanced datasource connections use VPC peering connections to directly connect the VPC networks of DLI queues and destination data sources. This point-to-point approach facilitates seamless data exchange, offering more flexible use cases and superior performance compared to basic datasource connections.

Note: The system's **default** queue does not support creating datasource connections. Establishing such connections requires functionalities like VPCs, subnets, routing, and VPC peering connections. Therefore, you must have the **VPC Administrator** permission for VPC. You can set these permissions by referring to "Service Authorization".

Permission Management
---------------------

DLI is equipped with a robust permission control mechanism. Additionally, DLI supports fine-grained authentication through Identity and Access Management (IAM). You can create IAM policies to manage access controls within DLI. Both permission control mechanisms can operate simultaneously without conflict.

Custom DLI Agency
-----------------

To perform cross-source analysis, DLI requires agency permissions to access other cloud services. This allows DLI to act on behalf of users or services in other cloud services, enabling it to read/write data and execute specific operations during job execution. Custom DLI agency ensures secure and efficient access to other cloud services during cross-source analysis.

Custom Image
------------

DLI supports containerized cluster deployments. In these clusters, components related to Spark and Flink jobs run within containers. By downloading custom images provided by DLI, you can modify the runtime environment of Spark and Flink containers. For example, adding Python packages or C libraries for machine learning into the custom image enables easy functional expansion tailored to your needs.
