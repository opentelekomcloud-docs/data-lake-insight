:original_name: dli_08_15030.html

.. _dli_08_15030:

ClickHouse
==========

Function
--------

DLI has the capability to export data from Flink jobs to the ClickHouse database. However, it only supports exporting data to result tables.

ClickHouse is a column-based database oriented to online analysis and processing. It supports SQL query and provides good query performance. The aggregation analysis and query performance based on large and wide tables is excellent, which is one order of magnitude faster than other analytical databases.

.. table:: **Table 1** Supported types

   ===================== ============
   Type                  Description
   ===================== ============
   Supported Table Types Result table
   ===================== ============

Prerequisites
-------------

-  Your jobs are running on a dedicated queue of DLI.
-  You have established an enhanced datasource connection to ClickHouse and set the port in the security group rule of the ClickHouse cluster as needed.

Caveats
-------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.

-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .

-  When you create a ClickHouse cluster for MRS, set the cluster version to MRS 3.1.0 or later.

-  The ClickHouse result table does not support table data deletion.

-  Flink supports the following data types: string, tinyint, smallint, int, bigint, float, double, date, timestamp, decimal, and array.

   The array supports only the int, bigint, string, float, and double data types.

Syntax
------

::

   create table clickhouseSink (
     attr_name attr_type
     (',' attr_name attr_type)*
   )
   with (
     'type' = 'clickhouse',
     'url' = '',
     'table-name' = ''
   );

Parameter Description
---------------------

.. table:: **Table 2** Parameters

   +----------------------------+-------------+---------------------------------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory   | Default Value                         | Data Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
   +============================+=============+=======================================+=============+========================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | connector                  | Yes         | None                                  | String      | Result table type. Set this parameter to **clickhouse**.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   +----------------------------+-------------+---------------------------------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | url                        | Yes         | None                                  | String      | ClickHouse URL                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                            |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   |                            |             |                                       |             | It is in the format of **jdbc:clickhouse://**\ *ClickHouseBalancer instance service IP address 1*\ **:**\ *ClickHouseBalancer port*\ **,**\ *ClickHouseBalancer instance service IP address 2*\ **:**\ *ClickHouseBalancer port*\ **/**\ *Database name*.                                                                                                                                                                                                                                                                                              |
   |                            |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   |                            |             |                                       |             | -  IP address of a ClickHouseBalancer instance:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   |                            |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   |                            |             |                                       |             |    Log in to the MRS console and choose **Clusters** > **Active Clusters** in the navigation pane. Click a cluster name, and choose **Components** > **ClickHouse** > **Instances** to obtain the business IP address of the ClickHouseBalancer instance.                                                                                                                                                                                                                                                                                              |
   |                            |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   |                            |             |                                       |             | -  ClickHouseBalancer port number:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
   |                            |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   |                            |             |                                       |             |    Log in to the MRS console and choose **Clusters** > **Active Clusters** in the navigation pane. Click a cluster name, and choose **Components** > **ClickHouse** > **Service Configuration**. On the **Service Configuration** page, select **ClickHouseBalancer** from the **All Roles** drop-down list. If the MRS cluster does not have Kerberos authentication enabled, search for **lb_http_port** and set it (defaults to **21425**). If Kerberos authentication is enabled, search for **lb_https_port** and set it (defaults to **21426**). |
   |                            |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   |                            |             |                                       |             | -  The database name is the name of the database created for the ClickHouse cluster. If the database name does not exist, there is no need to specify it.                                                                                                                                                                                                                                                                                                                                                                                              |
   |                            |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   |                            |             |                                       |             | -  You can configure multiple IP addresses for ClickHouseBalancer instances to avoid single points of failure (SPOFs) of the instances.                                                                                                                                                                                                                                                                                                                                                                                                                |
   |                            |             |                                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   |                            |             |                                       |             | -  If the MRS cluster has Kerberos authentication enabled, you also need to add the **ssl** and **sslmode** request parameters to the URL, setting **ssl** to **true** and **sslmode** to **none**. Refer to :ref:`Example 2 <dli_08_15030__li1997152814379>` for an example.                                                                                                                                                                                                                                                                          |
   +----------------------------+-------------+---------------------------------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table-name                 | Yes         | None                                  | String      | ClickHouse table name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
   +----------------------------+-------------+---------------------------------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | driver                     | No          | ru.yandex.clickhouse.ClickHouseDriver | String      | Driver required for connecting to the database. If you do not set this parameter, the automatically extracted driver will be used, which defaults to **ru.yandex.clickhouse.ClickHouseDriver**.                                                                                                                                                                                                                                                                                                                                                        |
   +----------------------------+-------------+---------------------------------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username                   | No          | None                                  | String      | Username for accessing the ClickHouse database. This parameter is mandatory when Kerberos authentication is enabled for the MRS cluster.                                                                                                                                                                                                                                                                                                                                                                                                               |
   +----------------------------+-------------+---------------------------------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                   | No          | None                                  | String      | Password for accessing the ClickHouse database. This parameter is mandatory when Kerberos authentication is enabled for the MRS cluster.                                                                                                                                                                                                                                                                                                                                                                                                               |
   +----------------------------+-------------+---------------------------------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.buffer-flush.max-rows | No          | 100                                   | Integer     | Maximum number of rows to be updated when data is written. The default value is **100**.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   +----------------------------+-------------+---------------------------------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.buffer-flush.interval | No          | 1s                                    | Duration    | Interval for data update. The unit can be ms, milli, millisecond/s, sec, second/min, or minute. The default value is **1s**. Value **0** indicates that data is not updated.                                                                                                                                                                                                                                                                                                                                                                           |
   +----------------------------+-------------+---------------------------------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.max-retries           | No          | 3                                     | Integer     | Maximum number of retries for writing data to the result table. The default value is **3**.                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
   +----------------------------+-------------+---------------------------------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

-  **Example 1: Read data from Kafka and insert the data into ClickHouse. (The ClickHouse version is 21.3.4.25 of MRS, and Kerberos authentication is not enabled for the MRS cluster):**

   #. Create an enhanced datasource connection in the VPC and subnet where ClickHouse and Kafka clusters locate, and bind the connection to the required Flink elastic resource pool.

   #. Set ClickHouse and Kafka cluster security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the ClickHouse address. If the connection passes the test, it is bound to the queue.

   #. Use the ClickHouse client to connect to the ClickHouse server and run the following command to query other environment parameters such as the cluster identifier:

      .. code-block::

         select cluster,shard_num,replica_num,host_name from system.clusters;

      The following information is displayed:

      .. code-block::

         ┌─cluster────┬────┬─shard_num─┐
         │ default_cluster │    1   │           1 │
         │ default_cluster │    1   │           2 │
         └──────── ┴────┴────── ┘

      Run the following command to create database **flink** on a node of the ClickHouse cluster based on the obtained cluster ID, for example, **default_cluster**:

      .. code-block::

         CREATE DATABASE flink ON CLUSTER default_cluster;

   #. Run the following command to create the ReplicatedMergeTree table named **order** on the node of cluster **default_cluster** and on database **flink**:

      .. code-block::

         CREATE TABLE flink.order ON CLUSTER default_cluster(order_id String,order_channel String,order_time String,pay_amount Float64,real_pay Float64,pay_time String,user_id String,user_name String,area_id String) ENGINE = ReplicatedMergeTree('/clickhouse/tables/{shard}/flink/order', '{replica}')ORDER BY order_id;

   #. Create a Flink OpenSource SQL job. Enter the following job script and submit the job. The job script uses the DMS Kafka data source and the ClickHouse result table.

      Change the values of the parameters in bold as needed in the following script.

      .. code-block::

         create table orders (
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
           'connector' = 'clickhouse',
           'url' = 'jdbc:clickhouse://ClickhouseAddress1:ClickhousePort,ClickhouseAddress2:ClickhousePort/flink',
           'username' = 'username',
           'password' = 'password',
           'table-name' = 'order',
           'sink.buffer-flush.max-rows' = '10',
           'sink.buffer-flush.interval' = '3s'
         );

         insert into clickhouseSink select * from orders;

   #. Connect to the Kafka cluster and insert the following test data into DMS Kafka:

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

-  .. _dli_08_15030__li1997152814379:

   **Example 2: Read data from Kafka and insert the data into ClickHouse. The procedure is as follows (The ClickHouse version is 21.3.4.25 of MRS, and Kerberos authentication is enabled for the MRS cluster):**

   #. Create an enhanced datasource connection in the VPC and subnet where ClickHouse and Kafka clusters locate, and bind the connection to the required Flink elastic resource pool.

   #. Set ClickHouse and Kafka cluster security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the ClickHouse address. If the connection passes the test, it is bound to the queue.

   #. Use the ClickHouse client to connect to the ClickHouse server and run the following command to query other environment parameters such as the cluster identifier:

      .. code-block::

         select cluster,shard_num,replica_num,host_name from system.clusters;

      The following information is displayed:

      .. code-block::

         ┌─cluster────┬────┬─shard_num─┐
         │ default_cluster │    1   │           1 │
         │ default_cluster │    1   │           2 │
         └──────── ┴────┴────── ┘

      Run the following command to create database **flink** on a node of the ClickHouse cluster based on the obtained cluster ID, for example, **default_cluster**:

      .. code-block::

         CREATE DATABASE flink ON CLUSTER default_cluster;

   #. Run the following command to create the ReplicatedMergeTree table named **order** on the node of cluster **default_cluster** and on database **flink**:

      .. code-block::

         CREATE TABLE flink.order ON CLUSTER default_cluster(order_id String,order_channel String,order_time String,pay_amount Float64,real_pay Float64,pay_time String,user_id String,user_name String,area_id String) ENGINE = ReplicatedMergeTree('/clickhouse/tables/{shard}/flink/order', '{replica}')ORDER BY order_id;

   #. Create a Flink OpenSource SQL job. Enter the following job script and submit the job. The job script uses the Kafka data source and the ClickHouse result table.

      Change the values of the parameters in bold as needed in the following script.

      .. code-block::

         create table orders (
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
           'connector' = 'clickhouse',
           'url' = 'jdbc:clickhouse://ClickhouseAddress1:ClickhousePort,ClickhouseAddress2:ClickhousePort/flink?ssl=true&sslmode=none',
           'table-name' = 'order',
           'username' = 'username',
           'password' = 'password', --Key in the DEW secret
           'sink.buffer-flush.max-rows' = '10',
           'sink.buffer-flush.interval' = '3s',
           'dew.endpoint'='kms.xx.xx.com', --Endpoint information for the DEW service being used
           'dew.csms.secretName'='xx', --Name of the DEW shared secret
           'dew.csms.decrypt.fields'='password', --The password field value must be decrypted and replaced using DEW secret management.
           'dew.csms.version'='v1'
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
