:original_name: dli_08_0348.html

.. _dli_08_0348:

OpenTSDB Result Table
=====================

Function
--------

OpenTSDB is a distributed, scalable time series database based on HBase. OpenTSDB is designed to collect monitoring information of a large-scale cluster and query data in seconds, facilitating querying and storing massive amounts of monitoring data in common databases. OpenTSDB can be used for system monitoring and measurement as well as collection and monitoring of IoT data, financial data, and scientific experimental results.

DLI uses enhanced datasource connections to write the output of Flink jobs to OpenTSDB.

Prerequisites
-------------

-  The OpenTSDB service has been enabled.
-  An enhanced datasource connection has been created for DLI to connect to OpenTSDB, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

Syntax
------

.. code-block::

   create table tsdbSink (
     attr_name attr_type
     (',' attr_name attr_type)*
   )
   with (
     'connector.type' = 'opentsdb',
     'connector.region' = '',
     'connector.tsdb-metrics' = '',
     'connector.tsdb-timestamps' = '',
     'connector.tsdb-values' = '',
     'connector.tsdb-tags' = '',
     'connector.tsdb-link-address' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +---------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                       | Mandatory             | Description                                                                                                                                                                    |
   +=================================+=======================+================================================================================================================================================================================+
   | connector.type                  | Yes                   | Connector type. Set this parameter to **opentsdb**.                                                                                                                            |
   +---------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.region                | Yes                   | Region where OpenTSDB locates                                                                                                                                                  |
   +---------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.tsdb-metrics          | Yes                   | Metrics of data points, which can be specified through parameter configurations.                                                                                               |
   |                                 |                       |                                                                                                                                                                                |
   |                                 |                       | The number of metrics must be 1 or the same as the number of **connector.tsdb-values**.                                                                                        |
   |                                 |                       |                                                                                                                                                                                |
   |                                 |                       | Use semicolons (;) to separate multiple metrics.                                                                                                                               |
   +---------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.tsdb-timestamps       | Yes                   | Timestamps of data points. Only dynamic columns are supported.                                                                                                                 |
   |                                 |                       |                                                                                                                                                                                |
   |                                 |                       | The data type can be int, bigint, or string. Only numbers are supported.                                                                                                       |
   |                                 |                       |                                                                                                                                                                                |
   |                                 |                       | The number of metrics must be 1 or the same as the number of **connector.tsdb-values**.                                                                                        |
   |                                 |                       |                                                                                                                                                                                |
   |                                 |                       | Use semicolons (;) to separate multiple timestamps.                                                                                                                            |
   +---------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.tsdb-values           | Yes                   | Values of data points. You can specify dynamic columns or constant values.                                                                                                     |
   |                                 |                       |                                                                                                                                                                                |
   |                                 |                       | Separate multiple values with semicolons (;).                                                                                                                                  |
   +---------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.tsdb-tags             | Yes                   | Tags of data points. Each tag contains at least one tag value and a maximum of eight tag values. Separate multiple tags by commas (,). You can specify the tags by parameters. |
   |                                 |                       |                                                                                                                                                                                |
   |                                 |                       | The number of metrics must be 1 or the same as the number of **connector.tsdb-values**.                                                                                        |
   |                                 |                       |                                                                                                                                                                                |
   |                                 |                       | Separate multiple tags with semicolons (;).                                                                                                                                    |
   +---------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.batch-insert-data-num | No                    | Number of data records to be written in batches at a time. The value must be a positive integer. The default value is **8**.                                                   |
   +---------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.tsdb-link-address     | Yes                   | OpenTSDB address for connecting to the cluster where the data to be inserted belongs.                                                                                          |
   +---------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

-  If your OpenTSDB runs in an MRS cluster, ensure that:

   #. The IP address and port number of OpenTSDB must be obtained from **tsd.network.bind** and **tsd.network.port** in the OpenTSDB service configuration.
   #. If **tsd.https.enabled** is set to **true**, the value format of **connector.tsdb-link-address** in the SQL statement is **https://**\ *ip:port*. If **tsd.https.enabled** is set to **false**, the value of **connector.tsdb-link-address** can be in the format of **http://**\ *ip:port* or *ip:port*.
   #. When establishing an enhanced datasource connection, you need to add the mapping between MRS cluster hosts and IP addresses in **/etc/hosts** to the Host Information parameter.

-  If a configuration item can be specified through parameter configurations, one or more columns in the record can be used as part of the configuration item. For example, if the configuration item is set to **car_$ {car_brand}** and the value of **car_brand** in a record is **BMW**, the value of this configuration item is **car_BMW** in the record.
-  If dynamic columns are supported, the format must be ${columnName}, where **columnName** indicates a field name.

Example
-------

.. code-block::

   create table sink1(
     attr1 bigint,
     attr2 int,
     attr3 int
   ) with (
     'connector.type' = 'opentsdb',
     'connector.region' = '',
     'connector.tsdb-metrics' = '',
     'connector.tsdb-timestamps' = '${attr1}',
     'connector.tsdb-values' = '${attr2};10',
     'connector.tsdb-tags' = 'key1:value1,key2:value2;key3:value3',
     'connector.tsdb-link-address' = ''
   );
