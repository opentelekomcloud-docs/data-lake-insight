:original_name: dli_07_0003.html

.. _dli_07_0003:

Basic Concepts
==============

Actual CUs, Used CUs, CU Range, and Yearly/Monthly CUs (Specifications) of an Elastic Resource Pool
---------------------------------------------------------------------------------------------------

-  **Actual CUs**: The current allocated resource size of the elastic resource pool, measured in CUs.

   -  When no queues exist in the resource pool: The actual CUs equal the minimum CUs set during its creation.

   -  When there are queues in the resource pool, the formula to calculate actual CUs is:

      -  Actual CUs = max{(min[sum(maximum CUs of queues), maximum CUs of the elastic resource pool]), minimum CUs of the elastic resource pool}.
      -  The result must be a multiple of 16 CUs. If not divisible by 16, round up to the nearest multiple.

   -  Scaling out or in an elastic resource pool means adjusting its actual CUs. See :ref:`Scaling Out or In an Elastic Resource Pool <dli_01_0686>`.

   -  Example of actual CU allocation:

      Consider :ref:`Table 1 <dli_07_0003__dli_07_0003_table4844638152415>` below, which illustrates the process of calculating actual CUs for an elastic resource pool:

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

-  **Used CUs**: The portion of CUs currently occupied by jobs or tasks, which may be actively performing computations.
-  **CU range**: CU settings are used to control the maximum and minimum CU ranges for elastic resource pools to avoid unlimited resource scaling.

   -  The sum of all queues' minimum CUs in an elastic resource pool must not exceed the pool's minCU.
   -  Any single queue's maxCU cannot exceed the pool's maxCU.
   -  The resource pool ensures it meets the minCU requirements across all queues while striving to accommodate their maxCU demands.
   -  When expanding the specifications of an elastic resource pool, the minimum value of the CU range is linked to the yearly/monthly CUs (specifications) of the elastic resource pool. After changing the specifications of the elastic resource pool, the minimum value of the CU range is modified to match the yearly/monthly CUs (specifications).

-  **Yearly/monthly CUs (specifications)**: The minimum value of the CU range selected when purchasing an elastic resource pool is the elastic resource pool specifications.

Database
--------

A database is a structured repository designed to organize, store, and manage data efficiently. In DLI, databases serve as the fundamental unit for managing permissions, with access rights assigned at the database level.

Within DLI, both tables and databases act as metadata containers that define underlying data structures. Table metadata informs DLI about the location of the data and specifies its structure, such as column names, data types, and table names. Databases provide logical groupings for these tables.

OBS Tables, DLI Tables, CloudTable Tables
-----------------------------------------

Different table types indicate distinct storage locations:

-  OBS table: Data is stored in buckets within OBS.

-  DLI table: Data is stored in tables internal to DLI.

   DLI storage resources are internal resources used to house databases and DLI tables, essential for importing data into DLI and reflecting the volume of user data stored within the service.

-  CloudTable table: Data is stored in tables managed by CloudTable.

Tables can be created through DLI to establish connections with other services, enabling federated query and analysis across diverse data sources.

Metadata
--------

Metadata refers to data that defines other data types. It primarily describes information about the data itself, including its source, size, format, or other characteristics. In database fields, metadata is used to interpret the contents of a data warehouse.

SQL Jobs
--------

A SQL job refers to the execution entity within the system that handles operations such as running SQL statements, importing data, and exporting data through the SQL job editor.

It is ideal for scenarios involving structured data queries and analysis using standard SQL.

Flink Jobs
----------

Designed for real-time stream processing, Flink jobs are suited for low-latency applications requiring rapid responses, such as real-time monitoring and online analytics.

-  Flink OpenSource jobs: These allow you to use DLI-provided connectors and APIs for seamless integration with other data systems during job submission.
-  Flink Jar jobs: You can submit pre-compiled JAR files containing Flink jobs, offering greater flexibility and customization. This type is ideal for complex data processing tasks involving custom functions, UDFs, or specific library integrations, enabling advanced stream processing logic and state management using Flink's ecosystem.

Spark Jobs
----------

Spark jobs refer to those submitted via visual interfaces or RESTful APIs, supporting full-stack Spark functionalities including Spark Core, DataSet, MLlib, and GraphX.

CU
--

CU represents the unit of compute resources in DLI. One CU equals one vCPU paired with 4 GB of memory. Higher specifications correspond to increased computational power.

Constants and Variables
-----------------------

The differences between constants and variables are as follows:

-  Constants retain their value throughout program execution and cannot be altered. They are strictly read-only.
-  Variables are both readable and writable. A variable represents a specific memory address where the stored value can be updated at any time during runtime. For example, in **int a = 123**, **a** is an integer variable.

Table Lifecycle
---------------

The table lifecycle management feature in DLI automatically reclaims table (or partition) data if it remains unchanged after a specified period from its last update. This duration is termed the lifecycle. The feature simplifies storage space reclamation and data recycling processes while providing backup and recovery options to prevent accidental data loss.
