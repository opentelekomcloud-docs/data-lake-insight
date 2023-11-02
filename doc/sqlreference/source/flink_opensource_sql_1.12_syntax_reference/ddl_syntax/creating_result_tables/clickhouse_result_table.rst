:original_name: dli_08_0393.html

.. _dli_08_0393:

ClickHouse Result Table
=======================

Function
--------

DLI can output Flink job data to the ClickHouse database. ClickHouse is a column-based database oriented to online analysis and processing. It supports SQL query and provides good query performance. The aggregation analysis and query performance based on large and wide tables is excellent, which is one order of magnitude faster than other analytical databases.

Prerequisites
-------------

-  Your jobs are running on a dedicated queue (non-shared queue) of DLI.
-  You have established an enhanced datasource connection to ClickHouse and set the port in the security group rule of the ClickHouse cluster as needed.

Precautions
-----------

-  When creating a Flink OpenSource SQL job, you need to set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.

-  When you create a ClickHouse cluster for MRS, set the cluster version to MRS 3.1.0 or later and do not enable Kerberos authentication.

-  The ClickHouse result table does not support table data deletion.

-  Flink supports the following data types: string, tinyint, smallint, int, long, float, double, date, timestamp, decimal, and array.

   The array supports only the int, bigint, string, float, and double data types.

Syntax
------

::

   create table clickhouseSink (
     attr_name attr_type
     (',' attr_name attr_type)*
   )
   with (
     'connector.type' = clickhouse,
     'connector.url' = '',
     'connector.table' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +--------------------------------+-------------+---------------------------------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                      | Mandatory   | Default Value                         | Data Type   | Description                                                                                                                                                                                                                                                                                                                                                                                               |
   +================================+=============+=======================================+=============+===========================================================================================================================================================================================================================================================================================================================================================================================================+
   | connector.type                 | Yes         | None                                  | String      | Result table type. Set this parameter to **clickhouse**.                                                                                                                                                                                                                                                                                                                                                  |
   +--------------------------------+-------------+---------------------------------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.url                  | Yes         | None                                  | String      | ClickHouse URL.                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                |             |                                       |             | Parameter format: **jdbc:clickhouse://**\ *ClickHouseBalancer instance IP address*\ **:**\ *HTTP port number for ClickHouseBalancer instances*\ **/**\ *Database name*                                                                                                                                                                                                                                    |
   |                                |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                |             |                                       |             | -  IP address of a ClickHouseBalancer instance:                                                                                                                                                                                                                                                                                                                                                           |
   |                                |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                |             |                                       |             |    Log in to the MRS console and choose **Clusters** > **Active Clusters** in the navigation pane. Click a cluster name, and choose **Components** > **ClickHouse** > **Instances** to obtain the business IP address of the ClickHouseBalancer instance.                                                                                                                                                 |
   |                                |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                |             |                                       |             | -  HTTP port of a ClickHouseBalancer instance:                                                                                                                                                                                                                                                                                                                                                            |
   |                                |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                |             |                                       |             |    Log in to the MRS console and choose **Clusters** > **Active Clusters** in the navigation pane. Click a cluster name, and choose **Components** > **ClickHouse** > **Service Configuration**. On the **Service Configuration** page, select **ClickHouseBalancer** from the **All Roles** drop-down list, search for **lb_http_port**, and obtain the parameter value. The default value is **21425**. |
   |                                |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                |             |                                       |             | -  The database name is the name of the database created for the ClickHouse cluster.                                                                                                                                                                                                                                                                                                                      |
   +--------------------------------+-------------+---------------------------------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.table                | Yes         | None                                  | String      | Name of the ClickHouse table to be created.                                                                                                                                                                                                                                                                                                                                                               |
   +--------------------------------+-------------+---------------------------------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.driver               | No          | ru.yandex.clickhouse.ClickHouseDriver | String      | Driver required for connecting to the database.                                                                                                                                                                                                                                                                                                                                                           |
   |                                |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                |             |                                       |             | -  If this parameter is not specified during table creation, the driver automatically extracts the value from the ClickHouse URL.                                                                                                                                                                                                                                                                         |
   |                                |             |                                       |             | -  If this parameter is specified during table creation, the value must be **ru.yandex.clickhouse.ClickHouseDriver**.                                                                                                                                                                                                                                                                                     |
   +--------------------------------+-------------+---------------------------------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.username             | No          | None                                  | String      | Username for connecting to the ClickHouse database.                                                                                                                                                                                                                                                                                                                                                       |
   +--------------------------------+-------------+---------------------------------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.password             | No          | None                                  | String      | Password for connecting to the ClickHouse database.                                                                                                                                                                                                                                                                                                                                                       |
   +--------------------------------+-------------+---------------------------------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.flush.max-rows | No          | 5000                                  | Integer     | Maximum number of rows to be updated when data is written. The default value is **5000**.                                                                                                                                                                                                                                                                                                                 |
   +--------------------------------+-------------+---------------------------------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.flush.interval | No          | 0                                     | Duration    | Interval for data update. The unit can be ms, milli, millisecond/s, sec, second/min, or minute.                                                                                                                                                                                                                                                                                                           |
   |                                |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                |             |                                       |             | Value **0** indicates that data is not updated.                                                                                                                                                                                                                                                                                                                                                           |
   +--------------------------------+-------------+---------------------------------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.max-retries    | No          | 3                                     | Integer     | Maximum number of retries for writing data to the result table. The default value is **3**.                                                                                                                                                                                                                                                                                                               |
   +--------------------------------+-------------+---------------------------------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

In this example, data is from Kafka and inserted to table **order** in ClickHouse database **flink**. The procedure is as follows (the ClickHouse version is 21.3.4.25 in MRS):

#. Create an enhanced datasource connection in the VPC and subnet where ClickHouse and Kafka clusters locate, and bind the connection to the required Flink queue.

#. Set ClickHouse and Kafka cluster security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the ClickHouse address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Use the ClickHouse client to connect to the ClickHouse server and run the following command to query other environment parameters such as the cluster ID:

   .. code-block::

      select cluster,shard_num,replica_num,host_name from system.clusters;

   The following information is displayed:

   .. code-block::

      ┌─cluster────┬────┬─shard_num─┐
      │ default_cluster │    1   │           1 │
      │ default_cluster │    1   │           2 │
      └──────── ┴────┴────── ┘

#. Run the following command to create database **flink** on a node of the ClickHouse cluster based on the obtained cluster ID, for example, **default_cluster**:

   .. code-block::

      CREATE DATABASE flink ON CLUSTER default_cluster;

#. Run the following command to create the ReplicatedMergeTree table named **order** on the node of cluster **default_cluster** and on database **flink**:

   .. code-block::

      CREATE TABLE flink.order ON CLUSTER default_cluster(order_id String,order_channel String,order_time String,pay_amount Float64,real_pay Float64,pay_time String,user_id String,user_name String,area_id String) ENGINE = ReplicatedMergeTree('/clickhouse/tables/{shard}/flink/order', '{replica}')ORDER BY order_id;

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job. The job script uses the Kafka data source and the ClickHouse result table.

   When you create a job, set **Flink Version** to **1.12** on the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

   .. code-block::

      CREATE TABLE orders (
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string
      ) WITH (
        'connector' = 'kafka',
        'topic' = 'KafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );

      create table clickhouseSink(
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string
      ) with (
        'connector.type' = 'clickhouse',
        'connector.url' = 'jdbc:clickhouse://ClickhouseAddress:ClickhousePort/flink',
        'connector.table' = 'order',
        'connector.write.flush.max-rows' = '1'
      );

      insert into clickhouseSink select * from orders;

#. Connect to the Kafka cluster and insert the following test data into Kafka:

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

#. Use the ClickHouse client to connect to the ClickHouse and run the following command to query the data written to table **order** in database **flink**:

   .. code-block::

      select * from flink.order;

   The query result is as follows:

   .. code-block::

      202103241000000001 webShop 2021-03-24 10:00:00 100 100 2021-03-24 10:02:03 0001 Alice 330106

      202103241606060001 appShop 2021-03-24 16:06:06 200 180 2021-03-24 16:10:06 0001 Alice 330106

      202103251202020001 miniAppShop 2021-03-25 12:02:02 60 60 2021-03-25 12:03:00 0002 Bob 330110

FAQ
---

None
