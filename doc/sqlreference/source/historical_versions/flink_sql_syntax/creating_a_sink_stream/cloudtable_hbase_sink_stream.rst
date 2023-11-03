:original_name: dli_08_0243.html

.. _dli_08_0243:

CloudTable HBase Sink Stream
============================

Function
--------

DLI exports the job output data to HBase of CloudTable. HBase is a column-oriented distributed cloud storage system that features enhanced reliability, excellent performance, and elastic scalability. It applies to the storage of massive amounts of data and distributed computing. You can use HBase to build a storage system capable of storing TB- or even PB-level data. With HBase, you can filter and analyze data with ease and get responses in milliseconds, rapidly mining data value. Structured and semi-structured key-value data can be stored, including messages, reports, recommendation data, risk control data, logs, and orders. With DLI, you can write massive volumes of data to HBase at a high speed and with low latency.

CloudTable is a distributed, scalable, and fully-hosted key-value data storage service based on Apache HBase. It provides DLI with high-performance random read and write capabilities, which are helpful when applications need to store and query a massive amount of structured data, semi-structured data, and time series data. CloudTable applies to IoT scenarios and storage and query of massive volumes of key-value data. For more information about CloudTable, see the *CloudTable Service User Guide*.

Prerequisites
-------------

In this scenario, jobs must run on the dedicated queue of DLI. Therefore, DLI must interconnect with the enhanced datasource connection that has been connected with CloudTable HBase. You can also set the security group rules as required.

For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

For details about how to configure security group rules, see **Security Group** in the *Virtual Private Cloud User Guide*.

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "cloudtable",
       region = "",
       cluster_id = "",
       table_name = "",
       table_columns = "",
       create_if_not_exist = ""
     )

Keyword
-------

.. table:: **Table 1** Keyword description

   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory | Description                                                                                                                                                                                                                                                                        |
   +=======================+===========+====================================================================================================================================================================================================================================================================================+
   | type                  | Yes       | Output channel type. **cloudtable** indicates that data is exported to CloudTable (HBase).                                                                                                                                                                                         |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region                | Yes       | Region to which CloudTable belongs.                                                                                                                                                                                                                                                |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cluster_id            | Yes       | ID of the cluster to which the data you want to insert belongs.                                                                                                                                                                                                                    |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name            | Yes       | Name of the table, into which data is to be inserted. It can be specified through parameter configurations. For example, if you want one or more certain columns as part of the table name, use **car_pass_inspect_with_age_${car_age}**, where **car_age** is the column name.    |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_columns         | Yes       | Columns to be inserted. The format is **rowKey, f1:c1, f1:c2, f2:c1**, where **rowKey** must be specified. If you do not want to add a column (for example, the third column) to the database, set this parameter to **rowKey,f1:c1,,f2:c1**.                                      |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | illegal_data_table    | No        | If this parameter is specified, abnormal data (for example, **rowKey** does not exist) will be written into the table. If not specified, abnormal data will be discarded. The rowKey value is a timestamp followed by six random digits, and the schema is info:data, info:reason. |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | create_if_not_exist   | No        | Whether to create a table or column into which the data is written when this table or column does not exist. The value can be **true** or **false**. The default value is **false**.                                                                                               |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | batch_insert_data_num | No        | Number of data records to be written in batches at a time. The value must be a positive integer. The upper limit is **100**. The default value is **10**.                                                                                                                          |
   +-----------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

-  If a configuration item can be specified through parameter configurations, one or more columns in the record can be used as part of the configuration item. For example, if the configuration item is set to **car_$ {car_brand}** and the value of **car_brand** in a record is **BMW**, the value of this configuration item is **car_BMW** in the record.
-  In this way, data is written to HBase of CloudTable. The speed is limited. The dedicated resource mode is recommended.

Example
-------

Output data of stream **qualified_cars** to CloudTable (HBase).

::

   CREATE SINK STREAM qualified_cars (
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
       table_name = "car_pass_inspect_with_age_${car_age}",
       table_columns = "rowKey,info:owner,,car:speed,car:miles",
       illegal_data_table = "illegal_data",
       create_if_not_exist = "true",
       batch_insert_data_num = "20"
   );
