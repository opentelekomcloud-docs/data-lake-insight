:original_name: dli_08_15028.html

.. _dli_08_15028:

Overview
========

Table Type
----------

-  Source table: A source table is the data input table for Flink jobs, such as real-time streaming data input from Kafka, etc.
-  Dimension Table: An auxiliary table for the data source table, used to enrich and expand the data in the source table. In Flink jobs, because the data collected by the data collection end is often limited, the required dimension information needs to be completed before data analysis can be performed, and the dimension table represents the data source that stores dimension information. Common user dimension tables include MySQL, Redis, etc.
-  Result table: The result data table output by the Flink job, which writes each real-time processed data into the target storage, such as MySQL, HBase, and other databases.

Example:

Flink real-time consumes user order data from the Kafka source table, associates the product ID with the dimension table through Redis to obtain the product category, calculates the sales amount of different categories of products, and writes the calculation results into the RDS (Relational Database Service, such as MySQL) result table.

Table information is as follows:

-  Source table: Order data table, including user ID, product ID, order ID, order amount, and other information.
-  Dimension table: User information table, including product ID and product category information.
-  Result table: Statistics of order sales amount data by product category.

The job first reads real-time order data from the order data source table, associates the order data stream with the product category information dimension table, then aggregates and calculates the total order amount, and finally writes the statistical results into the result table.

In this example, the order table serves as the driving source table input, the product category information table serves as the static dimension table, and the statistical result table serves as the final output of the job.

Supported Connectors
--------------------

.. table:: **Table 1** Supported connectors

   +-------------------------------------+---------------+-----------------+---------------+
   | Connector                           | Source Table  | Dimension Table | Result Table  |
   +=====================================+===============+=================+===============+
   | :ref:`BlackHole <dli_08_15029>`     | Not supported | Not supported   | Supported     |
   +-------------------------------------+---------------+-----------------+---------------+
   | :ref:`ClickHouse <dli_08_15030>`    | Not supported | Not supported   | Supported     |
   +-------------------------------------+---------------+-----------------+---------------+
   | :ref:`DataGen <dli_08_15031>`       | Supported     | Not supported   | Not supported |
   +-------------------------------------+---------------+-----------------+---------------+
   | :ref:`Doris <dli_08_15032>`         | Supported     | Supported       | Supported     |
   +-------------------------------------+---------------+-----------------+---------------+
   | :ref:`GaussDB(DWS) <dli_08_15037>`  | Supported     | Supported       | Supported     |
   +-------------------------------------+---------------+-----------------+---------------+
   | :ref:`Elasticsearch <dli_08_15038>` | Not supported | Not supported   | Supported     |
   +-------------------------------------+---------------+-----------------+---------------+
   | :ref:`FileSystem <dli_08_15039>`    | Supported     | Not supported   | Supported     |
   +-------------------------------------+---------------+-----------------+---------------+
   | :ref:`HBase <dli_08_15042>`         | Supported     | Supported       | Supported     |
   +-------------------------------------+---------------+-----------------+---------------+
   | :ref:`Hive <dli_08_15046>`          | Supported     | Supported       | Supported     |
   +-------------------------------------+---------------+-----------------+---------------+
   | :ref:`JDBC <dli_08_15057>`          | Supported     | Supported       | Supported     |
   +-------------------------------------+---------------+-----------------+---------------+
   | :ref:`Kafka <dli_08_15058>`         | Supported     | Not supported   | Supported     |
   +-------------------------------------+---------------+-----------------+---------------+
   | :ref:`Print <dli_08_15060>`         | Not supported | Not supported   | Supported     |
   +-------------------------------------+---------------+-----------------+---------------+
   | :ref:`Redis <dli_08_15061>`         | Supported     | Supported       | Supported     |
   +-------------------------------------+---------------+-----------------+---------------+
   | :ref:`Upsert Kafka <dli_08_15065>`  | Supported     | Not supported   | Supported     |
   +-------------------------------------+---------------+-----------------+---------------+
