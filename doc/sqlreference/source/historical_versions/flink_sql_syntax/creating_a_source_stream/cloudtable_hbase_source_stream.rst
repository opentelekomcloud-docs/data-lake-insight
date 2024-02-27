:original_name: dli_08_0237.html

.. _dli_08_0237:

CloudTable HBase Source Stream
==============================

Function
--------

Create a source stream to obtain data from HBase of CloudTable as input data of the job. HBase is a column-oriented distributed cloud storage system that features enhanced reliability, excellent performance, and elastic scalability. It applies to the storage of massive amounts of data and distributed computing. You can use HBase to build a storage system capable of storing TB- or even PB-level data. With HBase, you can filter and analyze data with ease and get responses in milliseconds, rapidly mining data value. DLI can read data from HBase for filtering, analysis, and data dumping.

CloudTable is a distributed, scalable, and fully-hosted key-value data storage service based on Apache HBase. It provides DLI with high-performance random read and write capabilities, which are helpful when applications need to store and query a massive amount of structured data, semi-structured data, and time series data. CloudTable applies to IoT scenarios and storage and query of massive volumes of key-value data. For more information about CloudTable, see the *CloudTable Service User Guide*.

Prerequisites
-------------

In this scenario, jobs must run on the dedicated queue of DLI. Therefore, DLI must interconnect with the enhanced datasource connection that has been connected with CloudTable HBase. You can also set the security group rules as required.

For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

For details about how to configure security group rules, see **Security Group** in the *Virtual Private Cloud User Guide*.

Syntax
------

::

   CREATE SOURCE STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "cloudtable",
       region = "",
       cluster_id = "",
       table_name = "",
       table_columns = ""
     );

Keywords
--------

.. table:: **Table 1** Keywords

   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                       |
   +=======================+=======================+===================================================================================================================================================================+
   | type                  | Yes                   | Data source type. **CloudTable** indicates that the data source is CloudTable.                                                                                    |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region                | Yes                   | Region to which CloudTable belongs.                                                                                                                               |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cluster_id            | Yes                   | ID of the cluster to which the data table to be read belongs.                                                                                                     |
   |                       |                       |                                                                                                                                                                   |
   |                       |                       | For details about how to view the ID of the CloudTable cluster, see section "**Viewing Basic Cluster Information**" in the *CloudTable Service User Guide*.       |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name            | Yes                   | Name of the table from which data is to be read. If a namespace needs to be specified, set it to **namespace_name:table_name**.                                   |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_columns         | Yes                   | Column to be read. The format is **rowKey,f1:c1,f1:c2,f2:c1**. The number of columns must be the same as the number of attributes specified in the source stream. |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

When creating a source stream, you can specify a time model for subsequent calculation. Currently, DLI supports two time models: Processing Time and Event Time. For details about the syntax, see :ref:`Configuring Time Models <dli_08_0107>`.

Example
-------

Read the **car_infos** table from HBase of CloudTable.

::

   CREATE SOURCE STREAM car_infos (
     car_id STRING,
     car_owner STRING,
     car_age INT,
     average_speed INT,
     total_miles INT
   )
     WITH (
       type = "cloudtable",
       region = "xxx",
       cluster_id = "209ab1b6-de25-4c48-8e1e-29e09d02de28",
       table_name = "carinfo",
       table_columns = "rowKey,info:owner,info:age,car:speed,car:miles"
   );
