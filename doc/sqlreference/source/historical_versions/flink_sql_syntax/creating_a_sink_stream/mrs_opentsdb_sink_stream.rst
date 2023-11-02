:original_name: dli_08_0286.html

.. _dli_08_0286:

MRS OpenTSDB Sink Stream
========================

Function
--------

DLI exports the output data of the Flink job to OpenTSDB of MRS.

Prerequisites
-------------

-  OpenTSDB has been installed in the MRS cluster.

-  In this scenario, jobs must run on the dedicated queue of DLI. Therefore, DLI must interconnect with the enhanced datasource connection that has been connected with MRS clusters. You can also set the security group rules as required.

   For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

   For details about how to configure security group rules, see **Security Group** in the *Virtual Private Cloud User Guide*.

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "opentsdb",
       region = "",
       tsdb_metrics = "",
       tsdb_timestamps = "",
       tsdb_values = "",
       tsdb_tags = "",
       batch_insert_data_num = ""
     )

Keywords
--------

.. table:: **Table 1** Keyword description

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                              |
   +=======================+=======================+==========================================================================================================================================================================+
   | type                  | Yes                   | Sink channel type. **opentsdb** indicates that data is exported to OpenTSDB of MRS.                                                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region                | Yes                   | Region where MRS resides.                                                                                                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tsdb_link_address     | Yes                   | Service address of the OpenTSDB instance in MRS. The format is **http://ip:port** or **https://ip:port**.                                                                |
   |                       |                       |                                                                                                                                                                          |
   |                       |                       | .. note::                                                                                                                                                                |
   |                       |                       |                                                                                                                                                                          |
   |                       |                       |    If **tsd.https.enabled** is set to **true**, HTTPS must be used. Note that HTTPS does not support certificate authentication.                                         |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tsdb_metrics          | Yes                   | Metric of a data point, which can be specified through parameter configurations.                                                                                         |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tsdb_timestamps       | Yes                   | Timestamp of a data point. The data type can be LONG, INT, SHORT, or STRING. Only dynamic columns are supported.                                                         |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tsdb_values           | Yes                   | Value of a data point. The data type can be SHORT, INT, LONG, FLOAT, DOUBLE, or STRING. Dynamic columns or constant values are supported.                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tsdb_tags             | Yes                   | Tags of a data point. Each of tags contains at least one tag value and up to eight tag values. Tags of the data point can be specified through parameter configurations. |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | batch_insert_data_num | No                    | Number of data records to be written in batches at a time. The value must be a positive integer. The upper limit is **65536**. The default value is **8**.               |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

If a configuration item can be specified through parameter configurations, one or more columns in the record can be used as part of the configuration item. For example, if the configuration item is set to **car_$ {car_brand}** and the value of **car_brand** in a record is **BMW**, the value of this configuration item is **car_BMW** in the record.

Example
-------

Output data of stream **weather_out** to OpenTSDB of MRS.

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
       tsdb_link_address = "https://x.x.x.x:4242",
       tsdb_metrics = "weather",
       tsdb_timestamps = "${timestamp_value}",
       tsdb_values = "${temperature}; ${humidity}",
       tsdb_tags = "location:${location},signify:temperature; location:${location},signify:humidity",
       batch_insert_data_num = "10"
   );
