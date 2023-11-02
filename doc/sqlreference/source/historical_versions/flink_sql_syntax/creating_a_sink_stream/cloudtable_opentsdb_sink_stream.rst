:original_name: dli_08_0244.html

.. _dli_08_0244:

CloudTable OpenTSDB Sink Stream
===============================

Function
--------

DLI exports the job output data to OpenTSDB of CloudTable. OpenTSDB is a distributed, scalable time series database based on HBase. It stores time series data. Time series data refers to the data collected at different time points. This type of data reflects the change status or degree of an object over time. OpenTSDB supports data collection and monitoring in seconds, permanent storage, index, and queries. It can be used for system monitoring and measurement as well as collection and monitoring of IoT data, financial data, and scientific experimental results.

CloudTable is a distributed, scalable, and fully-hosted key-value data storage service based on Apache HBase. It provides DLI with high-performance random read and write capabilities, which are helpful when applications need to store and query a massive amount of structured data, semi-structured data, and time series data. CloudTable applies to IoT scenarios and storage and query of massive volumes of key-value data. For more information about CloudTable, see the *CloudTable Service User Guide*.

Prerequisites
-------------

-  In this scenario, jobs must run on the dedicated queue of DLI. Therefore, DLI must interconnect with the enhanced datasource connection that has been connected with CloudTable HBase. You can also set the security group rules as required.

   For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

   For details about how to configure security group rules, see **Security Group** in the *Virtual Private Cloud User Guide*.

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "opentsdb",
       region = "",
       cluster_id = "",
       tsdb_metrics = "",
       tsdb_timestamps = "",
       tsdb_values = "",
       tsdb_tags = "",
       batch_insert_data_num = ""
     )

Keyword
-------

.. table:: **Table 1** Keyword description

   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                                                                                      |
   +=======================+=======================+==================================================================================================================================================================================================================================================================================================================+
   | type                  | Yes                   | Output channel type. **opentsdb** indicates that data is exported to CloudTable (OpenTSDB).                                                                                                                                                                                                                      |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region                | Yes                   | Region to which CloudTable belongs.                                                                                                                                                                                                                                                                              |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cluster_id            | No                    | ID of the cluster to which the data to be inserted belongs. Either this parameter or **tsdb_link_address** must be specified.                                                                                                                                                                                    |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tsdb_metrics          | Yes                   | Metric of a data point, which can be specified through parameter configurations.                                                                                                                                                                                                                                 |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tsdb_timestamps       | Yes                   | Timestamp of a data point. The data type can be LONG, INT, SHORT, or STRING. Only dynamic columns are supported.                                                                                                                                                                                                 |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tsdb_values           | Yes                   | Value of a data point. The data type can be SHORT, INT, LONG, FLOAT, DOUBLE, or STRING. Dynamic columns or constant values are supported.                                                                                                                                                                        |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tsdb_tags             | Yes                   | Tags of a data point. Each of tags contains at least one tag value and up to eight tag values. Tags of the data point can be specified through parameter configurations.                                                                                                                                         |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | batch_insert_data_num | No                    | Number of data records to be written in batches at a time. The value must be a positive integer. The upper limit is **65536**. The default value is **8**.                                                                                                                                                       |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tsdb_link_address     | No                    | OpenTSDB link of the cluster to which the data to be inserted belongs. If this parameter is used, the job must run in a dedicated DLI queue, and the DLI queue must be connected to the CloudTable cluster through an enhanced datasource connection. Either this parameter or **cluster_id** must be specified. |
   |                       |                       |                                                                                                                                                                                                                                                                                                                  |
   |                       |                       | .. note::                                                                                                                                                                                                                                                                                                        |
   |                       |                       |                                                                                                                                                                                                                                                                                                                  |
   |                       |                       |    For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.                                                                                                                                                             |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

If a configuration item can be specified through parameter configurations, one or more columns in the record can be used as part of the configuration item. For example, if the configuration item is set to **car_$ {car_brand}** and the value of **car_brand** in a record is **BMW**, the value of this configuration item is **car_BMW** in the record.

Example
-------

Output data of stream **weather_out** to CloudTable (OpenTSDB).

::

   CREATE SINK STREAM weather_out (
     timestamp_value LONG, /* Time */
     temperature FLOAT, /* Temperature value */
     humidity FLOAT, /* Humidity */
     location STRING /* Location */
   )
     WITH (
       type = "opentsdb",
       region = "xxx",
       cluster_id = "e05649d6-00e2-44b4-b0ff-7194adaeab3f",
       tsdb_metrics = "weather",
       tsdb_timestamps = "${timestamp_value}",
       tsdb_values = "${temperature}; ${humidity}",
       tsdb_tags = "location:${location},signify:temperature; location:${location},signify:humidity",
       batch_insert_data_num = "10"
   );
