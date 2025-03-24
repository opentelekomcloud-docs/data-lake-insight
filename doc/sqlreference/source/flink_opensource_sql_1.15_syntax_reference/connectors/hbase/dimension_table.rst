:original_name: dli_08_15045.html

.. _dli_08_15045:

Dimension Table
===============

Function
--------

Create an HBase dimension table to connect to the source streams for wide table generation.

Prerequisites
-------------

-  An enhanced datasource connection has been created for DLI to connect to HBase, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

-  **If MRS HBase is used, IP addresses of all hosts in the MRS cluster have been added to host information of the enhanced datasource connection.**

   For details, see "Modifying Host Information" in *Data Lake Insight User Guide*.

Caveats
-------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .
-  All the column families in HBase table must be declared as ROW type, the field name maps to the column family name, and the nested field names map to the column qualifier names. There is no need to declare all the families and qualifiers in the schema, users can declare what is used in the query. Except the ROW type fields, the single atomic type field (for example, STRING, BIGINT) will be recognized as HBase rowkey. The rowkey field can be an arbitrary name, but should be quoted using backticks if it is a reserved keyword.

Syntax
------

.. code-block::

   create table hbaseSource (
     attr_name attr_type
     (',' attr_name attr_type)*
    )
   with (
     'connector' = 'hbase-2.2',
     'table-name' = '',
     'zookeeper.quorum' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter              | Mandatory   | Default Value | Type        | Description                                                                                                                                                                                                                                           |
   +========================+=============+===============+=============+=======================================================================================================================================================================================================================================================+
   | connector              | Yes         | None          | String      | Connector type. Set this parameter to **hbase-2.2**.                                                                                                                                                                                                  |
   +------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table-name             | Yes         | None          | String      | Name of the HBase table                                                                                                                                                                                                                               |
   +------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | zookeeper.quorum       | Yes         | None          | String      | HBase Zookeeper quorum. The format is ZookeeperAddress:ZookeeperPort.                                                                                                                                                                                 |
   |                        |             |               |             |                                                                                                                                                                                                                                                       |
   |                        |             |               |             | The following describes how to obtain the ZooKeeper IP address and port number:                                                                                                                                                                       |
   |                        |             |               |             |                                                                                                                                                                                                                                                       |
   |                        |             |               |             | -  On the MRS Manager console, choose **Cluster** > *Name of the desired cluster* > **Service** > **ZooKeeper** > **Instance**. On the displayed page, obtain the IP address of the ZooKeeper instance.                                               |
   |                        |             |               |             | -  On the MRS Manager console, choose **Cluster** > *Name of the desired cluster* > **Service** > **ZooKeeper** > **Configuration**, and click **All Configurations**. Search for the **clientPort** parameter, and obtain the ZooKeeper port number. |
   +------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | zookeeper.znode.parent | No          | /hbase        | String      | Root directory in ZooKeeper for the HBase cluster.                                                                                                                                                                                                    |
   +------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.async           | No          | false         | Boolean     | Whether async lookup is enabled.                                                                                                                                                                                                                      |
   +------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.cache.max-rows  | No          | -1            | Long        | Maximum number of cached rows in a dimension table. When the rows exceed this value, the first item added to the cache will be marked as expired.                                                                                                     |
   |                        |             |               |             |                                                                                                                                                                                                                                                       |
   |                        |             |               |             | Lookup cache is disabled by default.                                                                                                                                                                                                                  |
   +------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.cache.ttl       | No          | -1            | Long        | Maximum time to live (TTL) for each row in lookup cache. Caches exceeding the TTL will be expired. The format is {length value}{time unit label}, for example, **123ms, 321s**. The supported time units include d, h, min, s, and ms (default unit). |
   |                        |             |               |             |                                                                                                                                                                                                                                                       |
   |                        |             |               |             | Lookup cache is disabled by default.                                                                                                                                                                                                                  |
   +------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.max-retries     | No          | 3             | Integer     | Maximum retry times if lookup database failed.                                                                                                                                                                                                        |
   +------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Data Type Mapping
-----------------

HBase stores all data as byte arrays. The data needs to be serialized and deserialized during read and write operations.

When serializing and de-serializing, Flink HBase connector uses utility class **org.apache.hadoop.hbase.util.Bytes** provided by HBase (Hadoop) to convert Flink data types to and from byte arrays.

Flink HBase connector encodes null values to empty bytes, and decodes empty bytes to null values for all data types except the string type. For string type, the null literal is determined by the **null-string-literal** option.

.. table:: **Table 2** Data type mapping

   +-----------------------------------+--------------------------------------------------------------------------------------+
   | Flink SQL Type                    | HBase Conversion                                                                     |
   +===================================+======================================================================================+
   | CHAR/VARCHAR/STRING               | byte[] toBytes(String s)                                                             |
   |                                   |                                                                                      |
   |                                   | String toString(byte[] b)                                                            |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | BOOLEAN                           | byte[] toBytes(boolean b)                                                            |
   |                                   |                                                                                      |
   |                                   | boolean toBoolean(byte[] b)                                                          |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | BINARY/VARBINARY                  | Returns byte[] as is.                                                                |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | DECIMAL                           | byte[] toBytes(BigDecimal v)                                                         |
   |                                   |                                                                                      |
   |                                   | BigDecimal toBigDecimal(byte[] b)                                                    |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | TINYINT                           | new byte[] { val }                                                                   |
   |                                   |                                                                                      |
   |                                   | bytes[0] // returns first and only byte from bytes                                   |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | SMALLINT                          | byte[] toBytes(short val)                                                            |
   |                                   |                                                                                      |
   |                                   | short toShort(byte[] bytes)                                                          |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | INT                               | byte[] toBytes(int val)                                                              |
   |                                   |                                                                                      |
   |                                   | int toInt(byte[] bytes)                                                              |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | BIGINT                            | byte[] toBytes(long val)                                                             |
   |                                   |                                                                                      |
   |                                   | long toLong(byte[] bytes)                                                            |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | FLOAT                             | byte[] toBytes(float val)                                                            |
   |                                   |                                                                                      |
   |                                   | float toFloat(byte[] bytes)                                                          |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | DOUBLE                            | byte[] toBytes(double val)                                                           |
   |                                   |                                                                                      |
   |                                   | double toDouble(byte[] bytes)                                                        |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | DATE                              | Number of days since 1970-01-01 00:00:00 UTC. The value is an integer.               |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | TIME                              | Number of milliseconds since 1970-01-01 00:00:00 UTC. The value is an integer.       |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | TIMESTAMP                         | Number of milliseconds since 1970-01-01 00:00:00 UTC. The value is of the long type. |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | ARRAY                             | Not supported                                                                        |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | MAP / MULTISET                    | Not supported                                                                        |
   +-----------------------------------+--------------------------------------------------------------------------------------+
   | ROW                               | Not supported                                                                        |
   +-----------------------------------+--------------------------------------------------------------------------------------+

Example
-------

In this example, data is read from a DMS Kafka data source, an HBase table is used as a dimension table to generate a wide table, and the result is written to a Kafka result table. The procedure is as follows (the HBase version in this example is 2.2.3):

#. Create an enhanced datasource connection in the VPC and subnet where HBase and Kafka locate, and bind the connection to the required Flink elastic resource pool. Add MRS host information for the enhanced datasource connection.

#. Set HBase and Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the HBase and Kafka addresses. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create an HBase table and name it **area_info** using the HBase shell. The table has only one column family **detail**. The creation statement is as follows:

   .. code-block::

      create 'area_info', {NAME => 'detail'}

#. Run the following statement in the HBase shell to insert dimension table data:

   .. code-block::

      put 'area_info', '330106', 'detail:area_province_name', 'a1'
      put 'area_info', '330106', 'detail:area_city_name', 'b1'
      put 'area_info', '330106', 'detail:area_county_name', 'c2'
      put 'area_info', '330106', 'detail:area_street_name', 'd2'
      put 'area_info', '330106', 'detail:region_name', 'e1'

      put 'area_info', '330110', 'detail:area_province_name', 'a1'
      put 'area_info', '330110', 'detail:area_city_name', 'b1'
      put 'area_info', '330110', 'detail:area_county_name', 'c4'
      put 'area_info', '330110', 'detail:area_street_name', 'd4'
      put 'area_info', '330110', 'detail:region_name', 'e1'

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job. The job script uses Kafka as the data source and an HBase table as the dimension table. Data is output to a Kafka result table.

   When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Set the values of the parameters in bold in the following script as needed.**

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
        area_id string,
        proctime as Proctime()
      ) WITH (
        'connector' = 'kafka',
        'topic' = 'KafkaSourceTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );

      -- Create an address dimension table
      create table area_info (
        area_id string,
        detail row(
          area_province_name string,
          area_city_name string,
          area_county_name string,
          area_street_name string,
          region_name string)
      ) WITH (
        'connector' = 'hbase-2.2',
        'table-name' = 'area_info',
        'zookeeper.quorum' = 'ZookeeperAddress:ZookeeperPort',
        'lookup.async' = 'true',
        'lookup.cache.max-rows' = '10000',
        'lookup.cache.ttl' = '2h'
      );

      -- Generate a wide table based on the address dimension table containing detailed order information.
      create table order_detail(
          order_id string,
          order_channel string,
          order_time string,
          pay_amount double,
          real_pay double,
          pay_time string,
          user_id string,
          user_name string,
          area_id string,
          area_province_name string,
          area_city_name string,
          area_county_name string,
          area_street_name string,
          region_name string
      ) with (
        'connector' = 'kafka',
        'topic' = '<yourSinkTopic>',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'format' = 'json'
      );

      insert into order_detail
          select orders.order_id, orders.order_channel, orders.order_time, orders.pay_amount, orders.real_pay, orders.pay_time, orders.user_id, orders.user_name,
                 area.area_id, area.area_province_name, area.area_city_name, area.area_county_name,
                 area.area_street_name, area.region_name  from orders
          left join area_info for system_time as of orders.proctime as area on orders.area_id = area.area_id;

#. Connect to the Kafka cluster and insert the following test data into the source topic in Kafka:

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

#. Connect to the Kafka cluster and read data from the sink topic of Kafka. The result data is as follows:

   .. code-block::

      {"order_id":"202103241000000001","order_channel":"webShop","order_time":"2021-03-24 10:00:00","pay_amount":100.0,"real_pay":100.0,"pay_time":"2021-03-24 10:02:03","user_id":"0001","user_name":"Alice","area_id":"330106","area_province_name":"a1","area_city_name":"b1","area_county_name":"c2","area_street_name":"d2","region_name":"e1"}

      {"order_id":"202103241606060001","order_channel":"appShop","order_time":"2021-03-24 16:06:06","pay_amount":200.0,"real_pay":180.0,"pay_time":"2021-03-24 16:10:06","user_id":"0001","user_name":"Alice","area_id":"330106","area_province_name":"a1","area_city_name":"b1","area_county_name":"c2","area_street_name":"d2","region_name":"e1"}

      {"order_id":"202103251202020001","order_channel":"miniAppShop","order_time":"2021-03-25 12:02:02","pay_amount":60.0,"real_pay":60.0,"pay_time":"2021-03-25 12:03:00","user_id":"0002","user_name":"Bob","area_id":"330110","area_province_name":"a1","area_city_name":"b1","area_county_name":"c4","area_street_name":"d4","region_name":"e1"}

FAQs
----

Q: What should I do if Flink job logs contain the following error information?

.. code-block::

   org.apache.zookeeper.ClientCnxn$SessionTimeoutException: Client session timed out, have not heard from server in 90069ms for connection id 0x0

A: The datasource connection is not bound or the binding fails. Configure the datasource connection or configure the security group of the Kafka cluster to allow access from the DLI queue.
