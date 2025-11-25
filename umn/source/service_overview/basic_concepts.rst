:original_name: dli_07_0003.html

.. _dli_07_0003:

Basic Concepts
==============

Elastic Resource Pool
---------------------

An elastic resource pool consists of dedicated compute resources where the resources in different pools are completely isolated from each other. Within a single elastic resource pool, multiple queues can share the resources available in that pool. Additionally, you can configure policies based on the resource load of these queues to enable time-based elastic scaling to meet diverse business needs.

DLI Storage Resource
--------------------

DLI storage resources are the internal storage capacities of the DLI service. They are utilized for storing databases and DLI tables, playing a crucial role in data import into DLI. These resources also indicate the volume of data that users have stored within DLI.

Actual CUs, Used CUs, CU Range, and Specifications of an Elastic Resource Pool
------------------------------------------------------------------------------

-  **Actual CUs**: actual size of resources currently allocated to the elastic resource pool (in CUs).

   -  When there is no queue in the resource pool, the actual CUs are equal to the minimum CUs when the elastic resource pool is created.

   -  When there are queues in the resource pool, the formula for calculating actual CUs is:

      -  Actual CUs = max{(min[sum(maximum CUs of queues), maximum CUs of the elastic resource pool]), minimum CUs of the elastic resource pool}.
      -  The calculation result must be a multiple of 16 CUs. If it cannot be exactly divided by 16 CUs, round up to the nearest multiple.

   -  Scaling out or in an elastic resource pool means adjusting the actual CUs of the resource pool. Refer to :ref:`Scaling Out or In an Elastic Resource Pool <dli_01_0686>`.

   -  Example of actual CU allocation:

      In :ref:`Table 1 <dli_07_0003__dli_07_0003_table4844638152415>`, the calculation process for the actual allocation of CUs in an elastic resource pool is as follows:

      #. Calculate the sum of maximum CUs of the queues: sum(maximum CUs) = 32 + 56 = 88 CUs.

      #. Compare the sum of maximum CUs of the queues with the maximum CUs of the elastic resource pool and take the smaller value: min{88 CUs, 112 CUs} = 88 CUs.

      #. Compare the value with the minimum CUs of the elastic resource pool and take the larger value: max(88 CUs, 64 CUs) = 88 CUs.

      #. Check if 88 CUs is a multiple of 16 CUs. Since 88 is not divisible by 16, round up to 96 CUs.

         .. _dli_07_0003__dli_07_0003_table4844638152415:

         .. table:: **Table 1** Example of actual CU allocation of an elastic resource pool

            +---------------------------------------------------------------------------------------------------+-----------------------+-----------------------+
            | Scenario                                                                                          | Resource Type         | CU Range              |
            +===================================================================================================+=======================+=======================+
            | New elastic resource pool: 64-112 CUs                                                             | Elastic resource pool | 64-112 CUs            |
            |                                                                                                   |                       |                       |
            | Queues A and B are created within the elastic resource pool. The CU ranges of the two queues are: |                       |                       |
            |                                                                                                   |                       |                       |
            | -  CU range of queue A: 16-32 CUs                                                                 |                       |                       |
            | -  CU range of queue B: 16-56 CUs                                                                 |                       |                       |
            +---------------------------------------------------------------------------------------------------+-----------------------+-----------------------+
            |                                                                                                   | Queue A               | 16-32 CUs             |
            +---------------------------------------------------------------------------------------------------+-----------------------+-----------------------+
            |                                                                                                   | Queue B               | 16-56CUS              |
            +---------------------------------------------------------------------------------------------------+-----------------------+-----------------------+

-  **Used CUs**: CUs that have been used by jobs or tasks. These resources may be executing computing tasks.
-  **CU range**: CU settings are used to control the maximum and minimum CU ranges for elastic resource pools to avoid unlimited resource scaling.

   -  The total minimum CUs of all queues in an elastic resource pool must be no more than the minimum CUs of the pool.
   -  The maximum CUs of any queue in an elastic resource pool must be no more than the maximum CUs of the pool.
   -  An elastic resource pool should at least ensure that all queues in it can run with the minimum CUs and should try to ensure that all queues in it can run with the maximum CUs.
   -  When expanding the specifications of an elastic resource pool, the minimum value of the CU range is linked to the specifications of the elastic resource pool. After changing the specifications of the elastic resource pool, the minimum value of the CU range is modified to match the specifications.

-  **Specifications**: The minimum CUs selected during elastic resource pool purchase are elastic resource pool specifications.

Database
--------

A database is a warehouse where data is organized, stored, and managed based on the data structure. DLI management permissions are granted on a per database basis.

In DLI, tables and databases are metadata containers that define underlying data. The metadata in the table shows the location of the data and specifies the data structure, such as the column name, data type, and table name. A database is a collection of tables.

OBS Table, DLI Table, and CloudTable Table
------------------------------------------

The table type indicates the storage location of data.

-  OBS table indicates that data is stored in the OBS bucket.
-  DLI table indicates that data is stored in the internal table of DLI.
-  CloudTable table indicates that data is stored in CloudTable.

You can create a table on DLI and associate the table with other services to achieve querying data from multiple data sources.

Metadata
--------

Metadata is used to define data types. It describes information about the data, including the source, size, format, and other data features. In database fields, metadata interprets data content in the data warehouse.

SQL Job
-------

SQL job refers to the SQL statement executed in the SQL job editor. It serves as the execution entity used for performing operations, such as importing and exporting data, in the SQL job editor.

This type is suitable for scenarios where standard SQL statements are used for querying. It is typically used for querying and analyzing structured data.

Flink Job
---------

This type is specifically designed for real-time data stream processing, making it ideal for scenarios that require low latency and quick response. It is well-suited for real-time monitoring and online analysis.

-  Flink OpenSource job: When submitting jobs, you can quickly integrate with other data systems using DLI's standard connectors and various APIs.
-  Flink Jar job: allows you to submit Flink jobs compiled into JAR files, providing greater flexibility and customization capabilities. It is suitable for complex data processing scenarios that require user-defined functions (UDFs) or specific library integration. The Flink ecosystem can be utilized to implement advanced stream processing logic and status management.

Spark Job
---------

Spark jobs are those submitted by users through visualized interfaces and RESTful APIs. Full-stack Spark jobs are allowed, such as Spark Core, DataSet, MLlib, and GraphX jobs.

CU
--

CU is the unit of compute resources in DLI, where 1 CU equals 1 vCPU and 4 GB of memory. The higher the specifications of compute resources, the better its computing power.

Constants and Variables
-----------------------

The differences between constants and variables are as follows:

-  During the running of a program, the value of a constant cannot be changed.
-  Variables are readable and writable, whereas constants are read-only. A variable is a memory address that contains a segment of data that can be changed during program running. For example, in **int a = 123**, **a** is an integer variable.

Table Lifecycle
---------------

The table lifecycle management feature in DLI refers to the automatic recycling of tables or partitions that have not been updated for a specified period of time since their last update. This specified period is known as the lifecycle. This feature simplifies the process of recycling data and frees up storage space. Additionally, it provides data backup and recovery functions to prevent data loss due to accidental operations.
