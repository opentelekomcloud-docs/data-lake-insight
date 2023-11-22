:original_name: dli_01_0567.html

.. _dli_01_0567:

Overview
========

DLI Job Type
------------

DLI provides the following job types:

-  SQL job: SQL jobs provide you with standard SQL statements and are compatible with Spark SQL and Presto SQL (based on Presto). You can query and analyze heterogeneous data sources on the cloud through visualized APIs, JDBC, ODBC, or Beeline. SQL jobs are compatible with mainstream data formats such as CSV, JSON, Parquet, Carbon, and ORC.
-  Flink job: Flink jobs are real-time streaming big data analysis service jobs running on the public cloud. In full hosting mode, you only need to focus on Stream SQL services and execute jobs instantly without being aware of compute clusters. Flink jobs are fully compatible with Apache Flink APIs.
-  Spark job: Spark jobs provide fully-managed Spark compute services. You can submit jobs through the GUI or RESTful APIs. Full-stack Spark jobs, such as Spark Core, DataSet, Streaming, MLlib, and GraphX jobs, are supported.

Constraints
-----------

-  Only the latest 100 jobs are displayed on DLI's SparkUI.
-  A maximum of 1,000 job results can be displayed on the console. To view more or all jobs, export the job data to OBS.
-  To export job run logs, you must have the permission to access OBS buckets. You need to configure a DLI job bucket on the **Global Configuration** > **Project** page in advance.
-  **View Log** and **Export Log** buttons are not available for synchronization jobs and jobs running on the default queue.
-  Only Spark jobs support custom images.
-  An elastic resource pool supports a maximum of 32,000 CUs.
