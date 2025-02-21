:original_name: dli_01_0001.html

.. _dli_01_0001:

DLI Job Development Process
===========================

This chapter walks you through on how to develop a DLI job.

Creating an IAM User and Granting Permissions
---------------------------------------------

-  To manage fine-grained permissions for your DLI resources using IAM, create an IAM user and grant them permissions to DLI if you are an enterprise user. For details, see :ref:`Creating an IAM User and Granting Permissions <dli_01_0418>`.

-  When using DLI for the first time, you need to update the DLI agency according to the console's guidance so that DLI can use other cloud services and perform resource O&M operations on your behalf. The agency includes permissions to obtain IAM user information, access and use VPCs, CIDR blocks, routes, and peering connections, and send notifications via SMN in case of job execution failure.

   For more information on the specific permissions included in the agency, refer to :ref:`Configuring DLI Agency Permissions <dli_01_0618>`.

Creating Compute Resources and Metadata Required for Running Jobs
-----------------------------------------------------------------

-  Before submitting a job using DLI, you need to create an elastic resource pool and create queues within it. This will provide the necessary compute resources for running the job. For how to create an elastic resource pool and create queues within it, see :ref:`Overview of DLI Elastic Resource Pools and Queues <dli_01_0504>`.

   Alternatively, you can enhance DLI's computing environment by creating custom images. Specifically, to enhance the functions and performance of Spark and Flink jobs, you can create custom images by downloading the base images provided by DLI and adding dependencies (files, JAR files, or software) and private capabilities required for job execution. This changes the container runtime environment for the jobs.

   For example, you can add a Python package or C library related to machine learning to a custom image to help you extend functions. For how to create a custom image, see :ref:`Using a Custom Image to Enhance the Job Running Environment <dli_01_0494>`.

-  DLI metadata is the basis for developing SQL and Spark jobs. Before executing a job, you need to define databases and tables based on your business scenario.

   .. note::

      Flink allows for dynamic data types, enabling the definition of data structures at runtime without the need for predefined metadata.

   -  Define your data structures, including data catalogs, databases, and tables. For details, see :ref:`Creating Databases and Tables <dli_01_0390>`.
   -  Create a bucket to store temporary data generated during job running, such as job logs and job results. For details, see :ref:`Configuring a DLI Job Bucket <dli_01_0536>`.
   -  Configure the permission to access metadata. For details, see :ref:`Configuring Database Permissions on the DLI Console <dli_01_0447>` and :ref:`Configuring Table Permissions on the DLI Console <dli_01_0448>`.

Importing Data to DLI
---------------------

-  DLI allows you to analyze and query data stored in OBS without the need to migrate it. Simply upload your data to OBS and use DLI for data analysis.

-  Cross-source access can reduce data duplication and latency when real-time access and processing of data from different sources is required for service needs.

   The prerequisites for cross-source access are that DLI can communicate with the data source network and DLI can obtain the access credentials to the data source.

   -  Configure network connection between DLI and the data source by referring to :ref:`Configuring the Network Connection Between DLI and Data Sources (Enhanced Datasource Connection) <dli_01_0426>`.
   -  Manage data source credentials.

      -  You can use DLI's datasource authentication to manage the authentication information for accessing a specified datasource.

         This applies to SQL jobs and Flink 1.12 jobs. For details, see :ref:`Using DLI Datasource Authentication to Manage Access Credentials for Data Sources <dli_01_0422>`.

      -  You can also use DEW to manage access credentials for data sources and use a custom agency to authorize DLI to access DEW.

         This applies to Spark 3.3.1 or later and Flink 1.15 or later. For details, see :ref:`Using DEW to Manage Access Credentials for Data Sources <dli_01_0636>` and :ref:`Configuring an Agency to Allow DLI to Access Other Cloud Services <dli_01_0486>`.

Submitting a Job Using DLI
--------------------------

-  DLI offers a serverless service that integrates stream processing, batch processing, and interactive analytics. It supports various job types to meet different data processing needs.

   .. table:: **Table 1** Job types supported by DLI

      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Job Type              | Description                                                                                                                                                                                                                                                                                                  | Use Case                                                                                                                                                               |
      +=======================+==============================================================================================================================================================================================================================================================================================================+========================================================================================================================================================================+
      | SQL job               | This type is suitable for scenarios where standard SQL statements are used for querying. It is typically used for querying and analyzing structured data.                                                                                                                                                    | It applies to scenarios such as data warehouse query, report generation, and online analytical processing (OLAP).                                                      |
      |                       |                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                        |
      |                       | For details, see :ref:`Creating and Submitting a SQL Job <dli_01_0320>`.                                                                                                                                                                                                                                     |                                                                                                                                                                        |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Flink job             | This type is specifically designed for real-time data stream processing, making it ideal for scenarios that require low latency and quick response. It is well-suited for real-time monitoring and online analysis.                                                                                          | It applies to scenarios that require quick response, such as real-time data monitoring and real-time recommender systems.                                              |
      |                       |                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                        |
      |                       | -  Flink OpenSource job: DLI provides standard connectors and various APIs to facilitate quick integration with other data systems. For details, see :ref:`Creating a Flink OpenSource SQL Job <dli_01_0498>`.                                                                                               | Flink Jar jobs are suitable for data analysis scenarios that require custom stream processing logic, complex state management, or integration with specific libraries. |
      |                       |                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                        |
      |                       | -  Flink Jar job: allows you to submit Flink jobs compiled into JAR files, providing greater flexibility and customization capabilities.                                                                                                                                                                     |                                                                                                                                                                        |
      |                       |                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                        |
      |                       |    It is suitable for complex data processing scenarios that require user-defined functions (UDFs) or specific library integration. The Flink ecosystem can be utilized to implement advanced stream processing logic and status management. For details, see :ref:`Creating a Flink Jar Job <dli_01_0457>`. |                                                                                                                                                                        |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Spark job             | Compute jobs can be submitted through interactive sessions or batch processing. Jobs are submitted to queues created within an elastic resource pool, simplifying resource management and job scheduling.                                                                                                    | It is suitable for large-scale data processing and analysis, such as machine learning training, log analysis, and large-scale data mining.                             |
      |                       |                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                        |
      |                       | It supports multiple data sources and formats, providing rich data processing capabilities, including but not limited to SQL queries and machine learning. For details, see :ref:`Creating a Spark Job <dli_01_0384>`.                                                                                       |                                                                                                                                                                        |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  Manage program packages of Jar jobs.

   DLI allows you to submit Flink or Spark jobs compiled as JAR files, which contain the necessary code and dependency information for executing the job. These files are used for specific data processing tasks such as data query, analysis, and machine learning. You can manage program packages required for jobs on the DLI console.

   To submit a Spark Jar or Flink Jar job, you must first upload the program package to OBS, create a program package in DLI, and then submit the program package, data, and job parameters to run the job. For details, see :ref:`Managing Program Packages of Jar Jobs <dli_01_0366>`.

   .. note::

      For Spark 3.3.1 or later and Flink 1.15 or later, when creating a Jar job, you can directly configure the program package in OBS. Program packages cannot be read from DLI.

Using Cloud Eye to Monitor DLI
------------------------------

You can query DLI monitoring metrics and alarms through Cloud Eye management console or APIs.

For example, you can monitor the resource usage and job status of a DLI queue. For details about DLI metrics, see :ref:`Using Cloud Eye to Monitor DLI <dli_01_0445>`.

Using CTS to Audit DLI
----------------------

With CTS, you can log operations related to DLI, making it easier to search, audit, and trace in the future. For the supported operations, see :ref:`Using CTS to Audit DLI <dli_01_0318>`.
