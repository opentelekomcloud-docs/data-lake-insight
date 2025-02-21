:original_name: dli_07_0005.html

.. _dli_07_0005:

Notes and Constraints
=====================

Elastic Resource Pools
----------------------

For more notes and constraints on elastic resource pools, see :ref:`Notes and Constraints on Using an Elastic Resource Pool <dli_01_0505>`.

.. table:: **Table 1** Notes and constraints on elastic resource pools

   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Item                              | Description                                                                                                                                                                                                                                                                                                                                                                                |
   +===================================+============================================================================================================================================================================================================================================================================================================================================================================================+
   | Resource specifications           | -  An elastic resource pool currently supports up to 32,000 CUs.                                                                                                                                                                                                                                                                                                                           |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Managing elastic resource pools   | -  You cannot change the region of an elastic resource pool once the pool is created.                                                                                                                                                                                                                                                                                                      |
   |                                   | -  Flink 1.10 or later jobs can run in elastic resource pools.                                                                                                                                                                                                                                                                                                                             |
   |                                   | -  The CIDR block of an elastic resource pool cannot be changed once set.                                                                                                                                                                                                                                                                                                                  |
   |                                   | -  You can view only the scaling history of an elastic resource pool within 30 days.                                                                                                                                                                                                                                                                                                       |
   |                                   | -  Elastic resource pools cannot directly access the Internet.                                                                                                                                                                                                                                                                                                                             |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Elastic resource pool scaling     | -  Changes to elastic resource pool CUs can occur when setting the CU, adding or deleting queues in an elastic resource pool, or modifying the scaling policies of queues in an elastic resource pool, or when the system automatically triggers elastic resource pool scaling. However, in some cases, the system cannot guarantee that the scaling will reach the target CUs as planned. |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                   |    -  If there are not enough physical resources, an elastic resource pool may not be able to scale out to the desired target size.                                                                                                                                                                                                                                                        |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                   |    -  The system does not guarantee that an elastic resource pool will be scaled in to the desired target size.                                                                                                                                                                                                                                                                            |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                   |       The system checks the resource usage before scaling in the elastic resource pool to determine if there is enough space for scaling in. If the existing resources cannot be scaled in according to the minimum scaling step, the pool may not be scaled in successfully or only partially.                                                                                            |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                   |       The scaling step may vary depending on the resource specifications, usually 16 CUs, 32 CUs, 48 CUs, 64 CUs, etc.                                                                                                                                                                                                                                                                     |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                   |       For example, if the elastic resource pool has a capacity of 192 CUs and the queues in the pool are using 68 CUs due to running jobs, the plan is to scale in to 64 CUs.                                                                                                                                                                                                              |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                   |       When executing a scaling in task, the system determines that there are 124 CUs remaining and scales in by the minimum step of 64 CUs. However, the remaining 60 CUs cannot be scaled in any further. Therefore, after the elastic resource pool executes the scaling in task, its capacity is reduced to 128 CUs.                                                                    |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Queues
------

For more notes and constraints on using a queue, see :ref:`Overview of DLI Elastic Resource Pools and Queues <dli_01_0504>`.

.. table:: **Table 2** Notes and constraints on queues

   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | Item                              | Description                                                                                                     |
   +===================================+=================================================================================================================+
   | Resource type                     | -  Queue types:                                                                                                 |
   |                                   |                                                                                                                 |
   |                                   |    -  **default** queue: A queue named **default** is preset in DLI, where you can use resources on demand.     |
   |                                   |    -  For SQL: Spark SQL jobs can be submitted to SQL queues.                                                   |
   |                                   |    -  For general purpose: The queue is used to run Spark programs, Flink SQL jobs, and Flink Jar jobs.         |
   |                                   |                                                                                                                 |
   |                                   | -  You cannot change the queue type once a queue is purchased. To use another queue type, purchase a new queue. |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | Managing queues                   | -  The region of a queue cannot be changed.                                                                     |
   |                                   | -  DLI queues cannot access the Internet.                                                                       |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | Queue scaling                     | -  Queues with 16 CUs do not support scale-out or scale-in.                                                     |
   |                                   | -  Queues with 64 CUs do not support scale-in.                                                                  |
   |                                   | -  Newly created queues need to run jobs before they can be scaled in or out.                                   |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------+

Jobs
----

.. table:: **Table 3** Notes and constraints on jobs

   +-----------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Item                                                      | Description                                                                                                                                                                          |
   +===========================================================+======================================================================================================================================================================================+
   | Job type                                                  | -  DLI supports the following job types: SQL, Spark Jar, Flink OpenSource SQL, and Flink Jar.                                                                                        |
   +-----------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Engines and versions supported by different types of jobs | -  DLI supports the following Spark versions: Spark 3.3.1, Spark 3.1.1 (EOM), Spark 2.4.5 (EOM), and Spark 2.3 (EOS).                                                                |
   |                                                           | -  DLI supports the following Flink versions: Flink Jar 1.15, Flink 1.12 (EOM), Flink 1.10 (EOS), and Flink 1.7 (EOS).                                                               |
   +-----------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Managing jobs                                             | -  SparkUI can only display the latest 100 jobs.                                                                                                                                     |
   |                                                           | -  A maximum of 1,000 job results can be displayed on the console. To view more or all jobs, export the job data to OBS.                                                             |
   |                                                           | -  To export job run logs, you must have the permission to access OBS buckets. You need to configure a DLI job bucket on the **Global Configuration** > **Project** page in advance. |
   |                                                           | -  The **View Log** button is not available for synchronization jobs and jobs running on the default queue.                                                                          |
   |                                                           | -  Only Spark jobs support custom images.                                                                                                                                            |
   +-----------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

DLI Data Resources
------------------

For more notes and constraints on resources, see :ref:`Data Management <dli_01_0004>`.

.. table:: **Table 4** Notes and constraints on DLI resources

   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Item                              | Description                                                                                                                                                                                                                                                                                                   |
   +===================================+===============================================================================================================================================================================================================================================================================================================+
   | Database                          | -  **default** is the database built in DLI. You cannot create a database named **default**.                                                                                                                                                                                                                  |
   |                                   | -  DLI supports a maximum of 50 databases.                                                                                                                                                                                                                                                                    |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Data table                        | -  DLI supports a maximum of 5,000 tables.                                                                                                                                                                                                                                                                    |
   |                                   | -  DLI supports the following table types:                                                                                                                                                                                                                                                                    |
   |                                   |                                                                                                                                                                                                                                                                                                               |
   |                                   |    -  **MANAGED**: Data is stored in a DLI table.                                                                                                                                                                                                                                                             |
   |                                   |    -  **EXTERNAL**: Data is stored in an OBS table.                                                                                                                                                                                                                                                           |
   |                                   |    -  **View**: A view can only be created using SQL statements.                                                                                                                                                                                                                                              |
   |                                   |    -  Datasource table: The table type is also **EXTERNAL**.                                                                                                                                                                                                                                                  |
   |                                   |                                                                                                                                                                                                                                                                                                               |
   |                                   | -  You cannot specify a storage path when creating a DLI table.                                                                                                                                                                                                                                               |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Data import                       | -  Only OBS data can be imported to DLI or OBS.                                                                                                                                                                                                                                                               |
   |                                   | -  You can import data in CSV, Parquet, ORC, JSON, or Avro format from OBS to tables created on DLI.                                                                                                                                                                                                          |
   |                                   | -  To import data in CSV format to a partitioned table, place the partition column in the last column of the data source.                                                                                                                                                                                     |
   |                                   | -  The encoding format of imported data can only be UTF-8.                                                                                                                                                                                                                                                    |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Data export                       | -  Data in DLI tables (whose table type is **MANAGED**) can only be exported to OBS buckets, and the export path must contain a folder.                                                                                                                                                                       |
   |                                   | -  The exported file is in JSON format, and the text format can only be UTF-8.                                                                                                                                                                                                                                |
   |                                   | -  Data can be exported across accounts. That is, after account B authorizes account A, account A has the permission to read the metadata and permission information of account B's OBS bucket as well as the read and write permissions on the path. Account A can export data to the OBS path of account B. |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Packages
--------

.. table:: **Table 5** Notes and constraints on package usage

   +-----------------------------------+---------------------------------------------------------------------+
   | Item                              | Description                                                         |
   +===================================+=====================================================================+
   | Package                           | -  A package can be deleted, but a package group cannot be deleted. |
   |                                   | -  The following types of packages can be uploaded:                 |
   |                                   |                                                                     |
   |                                   |    -  **JAR**: JAR file                                             |
   |                                   |    -  **PyFile**: User Python file                                  |
   |                                   |    -  **File**: User file                                           |
   |                                   |    -  **ModelFile**: User AI model file                             |
   +-----------------------------------+---------------------------------------------------------------------+

Enhanced Datasource Connections
-------------------------------

For more notes and constraints on enhanced datasource connections, see :ref:`Enhanced Datasource Connection Overview <dli_01_0003>`.

.. table:: **Table 6** Notes and constraints on enhanced datasource connections

   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Item                              | Description                                                                                                                                                                         |
   +===================================+=====================================================================================================================================================================================+
   | Use case                          | -  Datasource connections cannot be created for the **default** queue.                                                                                                              |
   |                                   | -  Flink jobs can directly access DIS, OBS, and SMN data sources without using datasource connections.                                                                              |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Permission                        | -  **VPC Administrator** permissions are required for enhanced connections to use VPCs, subnets, routes, VPC peering connections.                                                   |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Usage                             | -  If you use an enhanced datasource connection, the CIDR block of the elastic resource pool or queue cannot overlap with that of the data source.                                  |
   |                                   | -  Only queues bound with datasource connections can access datasource tables.                                                                                                      |
   |                                   | -  Datasource tables do not support the preview function.                                                                                                                           |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Connectivity check                | -  When checking the connectivity of datasource connections, the notes and constraints on IP addresses are:                                                                         |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  The IP address must be valid, which consists of four decimal numbers separated by periods (.). The value ranges from 0 to 255.                                                |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  During the test, you can add a port after the IP address and separate them with colons (:). The port can contain a maximum of five digits. The value ranges from 0 to 65535.  |
   |                                   |                                                                                                                                                                                     |
   |                                   |       For example, **192.168.**\ *xx*\ **.**\ *xx* or **192.168.**\ *xx*\ **.**\ *xx*\ **:8181**.                                                                                   |
   |                                   |                                                                                                                                                                                     |
   |                                   | -  When checking the connectivity of datasource connections, the notes and constraints on domain names are:                                                                         |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  The domain name can contain 1 to 255 characters. Only letters, numbers, underscores (_), and hyphens (-) are allowed.                                                         |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  The top-level domain name must contain at least two letters, for example, **.com**, **.net**, and **.cn**.                                                                    |
   |                                   |                                                                                                                                                                                     |
   |                                   |    -  During the test, you can add a port after the domain name and separate them with colons (:). The port can contain a maximum of five digits. The value ranges from 0 to 65535. |
   |                                   |                                                                                                                                                                                     |
   |                                   |       Example: **example.com:8080**                                                                                                                                                 |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Datasource Authentication
-------------------------

For more notes and constraints on datasource authentication, see :ref:`Datasource Authentication Introduction <dli_01_0561>`.

.. table:: **Table 7** Notes and constraints on datasource authentication

   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------+
   | Item                              | Description                                                                                                          |
   +===================================+======================================================================================================================+
   | Use case                          | -  Only Spark SQL and Flink OpenSource SQL 1.12 jobs support datasource authentication.                              |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------+
   | Datasource authentication type    | -  DLI supports four types of datasource authentication. Select an authentication type specific to each data source. |
   |                                   |                                                                                                                      |
   |                                   |    -  CSS: applies to 6.5.4 or later CSS clusters with the security mode enabled.                                    |
   |                                   |    -  Kerberos: applies to MRS security clusters with Kerberos authentication enabled.                               |
   |                                   |    -  Kafka_SSL: applies to Kafka with SSL enabled.                                                                  |
   |                                   |    -  Password: applies to GaussDB(DWS), RDS, DDS, and DCS.                                                          |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------+

SQL Syntax
----------

.. table:: **Table 8** Notes and constraints on SQL syntax

   +--------------------------------------------+-----------------------------------------------------------------------------------------------+
   | Item                                       | Description                                                                                   |
   +============================================+===============================================================================================+
   | SQL syntax                                 | You are not allowed to specify a storage path when creating a DLI table using SQL statements. |
   +--------------------------------------------+-----------------------------------------------------------------------------------------------+
   | Constraints on the size of SQL statements: | -  Each SQL statement should contain less than 500,000 characters.                            |
   |                                            | -  The size of each SQL statement must be less than 1 MB.                                     |
   +--------------------------------------------+-----------------------------------------------------------------------------------------------+

Other
-----

.. table:: **Table 9** Other notes and constraints

   +-----------------------------------+-------------------------------------------------------------------+
   | Item                              | Description                                                       |
   +===================================+===================================================================+
   | Quota                             | For quota notes and constraints, see :ref:`Quotas <dli_07_0009>`. |
   +-----------------------------------+-------------------------------------------------------------------+
   | Browser version                   | Recommended browsers and their versions:                          |
   |                                   |                                                                   |
   |                                   | -  Google Chrome 43.0 or later                                    |
   |                                   | -  Mozilla Firefox 38.0 or later                                  |
   |                                   | -  Internet Explorer 9.0 or later                                 |
   +-----------------------------------+-------------------------------------------------------------------+
