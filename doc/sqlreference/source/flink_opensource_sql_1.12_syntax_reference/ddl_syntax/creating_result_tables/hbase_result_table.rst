:original_name: dli_08_0396.html

.. _dli_08_0396:

HBase Result Table
==================

Function
--------

DLI outputs the job data to HBase. HBase is a column-oriented distributed cloud storage system that features enhanced reliability, excellent performance, and elastic scalability. It applies to the storage of massive amounts of data and distributed computing. You can use HBase to build a storage system capable of storing TB- or even PB-level data. With HBase, you can filter and analyze data with ease and get responses in milliseconds, rapidly mining data value. Structured and semi-structured key-value data can be stored, including messages, reports, recommendation data, risk control data, logs, and orders. With DLI, you can write massive volumes of data to HBase at a high speed and with low latency.

Prerequisites
-------------

-  An enhanced datasource connection has been created for DLI to connect to HBase, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

-  If MRS HBase is used, IP addresses of all hosts in the MRS cluster have been added to host information of the enhanced datasource connection.

   .

Precautions
-----------

-  When creating a Flink OpenSource SQL job, you need to set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.
-  Do not enable Kerberos authentication for the HBase cluster.
-  The column families in created HBase result table must be declared as the ROW type, the field names map the column family names, and the nested field names map the column qualifier names. There is no need to declare all the families and qualifiers in the schema. Users can declare what is used in the query. Except the ROW type fields, the single atomic type field (for example, STRING or BIGINT) will be recognized as the HBase rowkey. The rowkey field can be an arbitrary name, but should be quoted using backticks if it is a reserved keyword.

Syntax
------

.. code-block::

   create table hbaseSink (
     attr_name attr_type
     (',' attr_name attr_type)*
     ','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   ) with (
     'connector' = 'hbase-2.2',
     'table-name' = '',
     'zookeeper.quorum' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                                                                                                                            |
   +============================+=============+===============+=============+========================================================================================================================================================================================================================================================================+
   | connector                  | Yes         | None          | String      | Connector to be used. Set this parameter to **hbase-2.2**.                                                                                                                                                                                                             |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table-name                 | Yes         | None          | String      | Name of the HBase table to connect.                                                                                                                                                                                                                                    |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | zookeeper.quorum           | Yes         | None          | String      | HBase ZooKeeper instance information, in the format of ZookeeperAddress:ZookeeperPort.                                                                                                                                                                                 |
   |                            |             |               |             |                                                                                                                                                                                                                                                                        |
   |                            |             |               |             | The following uses an MRS HBase cluster as an example to describe how to obtain the IP address and port number of ZooKeeper used by this parameter:                                                                                                                    |
   |                            |             |               |             |                                                                                                                                                                                                                                                                        |
   |                            |             |               |             | -  On MRS Manager, choose **Cluster** and click the name of the desired cluster. Choose **Services** > **ZooKeeper** > **Instance**, and obtain the IP address of the ZooKeeper instance.                                                                              |
   |                            |             |               |             | -  On MRS Manager, choose **Cluster** and click the name of the desired cluster. Choose **Services** > **ZooKeeper** > **Configurations** > **All Configurations**, search for the **clientPort** parameter, and obtain its value, that is, the ZooKeeper port number. |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | zookeeper.znode.parent     | No          | /hbase        | String      | Root directory in ZooKeeper. The default value is **/hbase**.                                                                                                                                                                                                          |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | null-string-literal        | No          | null          | String      | Representation for null values for string fields.                                                                                                                                                                                                                      |
   |                            |             |               |             |                                                                                                                                                                                                                                                                        |
   |                            |             |               |             | The HBase sink encodes/decodes empty bytes as null values for all types except the string type.                                                                                                                                                                        |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.buffer-flush.max-size | No          | 2mb           | MemorySize  | Maximum size in memory of buffered rows for each write request.                                                                                                                                                                                                        |
   |                            |             |               |             |                                                                                                                                                                                                                                                                        |
   |                            |             |               |             | This can improve performance for writing data to the HBase database, but may increase the latency.                                                                                                                                                                     |
   |                            |             |               |             |                                                                                                                                                                                                                                                                        |
   |                            |             |               |             | You can set this parameter to **0** to disable it.                                                                                                                                                                                                                     |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.buffer-flush.max-rows | No          | 1000          | Integer     | Maximum number of rows to buffer for each write request.                                                                                                                                                                                                               |
   |                            |             |               |             |                                                                                                                                                                                                                                                                        |
   |                            |             |               |             | This can improve performance for writing data to the HBase database, but may increase the latency.                                                                                                                                                                     |
   |                            |             |               |             |                                                                                                                                                                                                                                                                        |
   |                            |             |               |             | You can set this parameter to **0** to disable it.                                                                                                                                                                                                                     |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.buffer-flush.interval | No          | 1s            | Duration    | Interval for flushing any buffered rows.                                                                                                                                                                                                                               |
   |                            |             |               |             |                                                                                                                                                                                                                                                                        |
   |                            |             |               |             | This can improve performance for writing data to the HBase database, but may increase the latency.                                                                                                                                                                     |
   |                            |             |               |             |                                                                                                                                                                                                                                                                        |
   |                            |             |               |             | You can set this parameter to **0** to disable it.                                                                                                                                                                                                                     |
   |                            |             |               |             |                                                                                                                                                                                                                                                                        |
   |                            |             |               |             | Note: Both **sink.buffer-flush.max-size** and **sink.buffer-flush.max-rows** can be set to **0** with the flush interval set allowing for complete asynchronous processing of buffered actions.                                                                        |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.parallelism           | No          | None          | Integer     | Defines the parallelism of the HBase sink operator.                                                                                                                                                                                                                    |
   |                            |             |               |             |                                                                                                                                                                                                                                                                        |
   |                            |             |               |             | By default, the parallelism is determined by the framework using the same parallelism of the upstream chained operator.                                                                                                                                                |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Data Type Mapping
-----------------

HBase stores all data as byte arrays. The data needs to be serialized and deserialized during read and write operations.

When serializing and de-serializing, Flink HBase connector uses utility class **org.apache.hadoop.hbase.util.Bytes** provided by HBase (Hadoop) to convert Flink data types to and from byte arrays.

Flink HBase connector encodes null values to empty bytes, and decode empty bytes to null values for all data types except the string type. For the string type, the null literal is determined by the **null-string-literal** option.

.. table:: **Table 2** Data type mapping

   +-----------------------------------+---------------------------------------------------------------+
   | Flink SQL Type                    | HBase Conversion                                              |
   +===================================+===============================================================+
   | CHAR / VARCHAR / STRING           | byte[] toBytes(String s)                                      |
   |                                   |                                                               |
   |                                   | String toString(byte[] b)                                     |
   +-----------------------------------+---------------------------------------------------------------+
   | BOOLEAN                           | byte[] toBytes(boolean b)                                     |
   |                                   |                                                               |
   |                                   | boolean toBoolean(byte[] b)                                   |
   +-----------------------------------+---------------------------------------------------------------+
   | BINARY / VARBINARY                | Returns byte[] as is.                                         |
   +-----------------------------------+---------------------------------------------------------------+
   | DECIMAL                           | byte[] toBytes(BigDecimal v)                                  |
   |                                   |                                                               |
   |                                   | BigDecimal toBigDecimal(byte[] b)                             |
   +-----------------------------------+---------------------------------------------------------------+
   | TINYINT                           | new byte[] { val }                                            |
   |                                   |                                                               |
   |                                   | bytes[0] // returns first and only byte from bytes            |
   +-----------------------------------+---------------------------------------------------------------+
   | SMALLINT                          | byte[] toBytes(short val)                                     |
   |                                   |                                                               |
   |                                   | short toShort(byte[] bytes)                                   |
   +-----------------------------------+---------------------------------------------------------------+
   | INT                               | byte[] toBytes(int val)                                       |
   |                                   |                                                               |
   |                                   | int toInt(byte[] bytes)                                       |
   +-----------------------------------+---------------------------------------------------------------+
   | BIGINT                            | byte[] toBytes(long val)                                      |
   |                                   |                                                               |
   |                                   | long toLong(byte[] bytes)                                     |
   +-----------------------------------+---------------------------------------------------------------+
   | FLOAT                             | byte[] toBytes(float val)                                     |
   |                                   |                                                               |
   |                                   | float toFloat(byte[] bytes)                                   |
   +-----------------------------------+---------------------------------------------------------------+
   | DOUBLE                            | byte[] toBytes(double val)                                    |
   |                                   |                                                               |
   |                                   | double toDouble(byte[] bytes)                                 |
   +-----------------------------------+---------------------------------------------------------------+
   | DATE                              | Stores the number of days since epoch as an int value.        |
   +-----------------------------------+---------------------------------------------------------------+
   | TIME                              | Stores the number of milliseconds of the day as an int value. |
   +-----------------------------------+---------------------------------------------------------------+
   | TIMESTAMP                         | Stores the milliseconds since epoch as a long value.          |
   +-----------------------------------+---------------------------------------------------------------+
   | ARRAY                             | Not supported                                                 |
   +-----------------------------------+---------------------------------------------------------------+
   | MAP / MULTISET                    | Not supported                                                 |
   +-----------------------------------+---------------------------------------------------------------+
   | ROW                               | Not supported                                                 |
   +-----------------------------------+---------------------------------------------------------------+

Example
-------

In this example, data is read from the Kafka data source and written to the HBase result table. The procedure is as follows (the HBase versions used in this example are 1.3.1 and 2.2.3):

#. Create an enhanced datasource connection in the VPC and subnet where HBase and Kafka locate, and bind the connection to the required Flink elastic resource pool. .

#. Set HBase and Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the HBase and Kafka address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Use the HBase shell to create HBase table **order** that has only one column family **detail**.

   .. code-block::

      create 'order', {NAME => 'detail'}

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job. The job script uses Kafka as the data source and HBase as the result table (the Rowkey is **order_id** and the column family name is **detail**).

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

      create table hbaseSink(
        order_id string,
        detail Row(
          order_channel string,
          order_time string,
          pay_amount double,
          real_pay double,
          pay_time string,
          user_id string,
          user_name string,
          area_id string)
      ) with (
        'connector' = 'hbase-2.2',
        'table-name' = 'order',
        'zookeeper.quorum' = 'ZookeeperAddress:ZookeeperPort',
        'sink.buffer-flush.max-rows' = '1'
      );

      insert into hbaseSink select order_id, Row(order_channel,order_time,pay_amount,real_pay,pay_time,user_id,user_name,area_id) from orders;

#. Connect to the Kafka cluster and enter the following data to Kafka:

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

#. Run the following statement on the HBase shell to view the data result:

   .. code-block::

       scan 'order'

   The data result is as follows:

   .. code-block::

      202103241000000001   column=detail:area_id, timestamp=2021-12-16T21:30:37.954, value=330106

      202103241000000001   column=detail:order_channel, timestamp=2021-12-16T21:30:37.954, value=webShop

      202103241000000001   column=detail:order_time, timestamp=2021-12-16T21:30:37.954, value=2021-03-24 10:00:00

      202103241000000001   column=detail:pay_amount, timestamp=2021-12-16T21:30:37.954, value=@Y\x00\x00\x00\x00\x00\x00

      202103241000000001   column=detail:pay_time, timestamp=2021-12-16T21:30:37.954, value=2021-03-24 10:02:03

      202103241000000001   column=detail:real_pay, timestamp=2021-12-16T21:30:37.954, value=@Y\x00\x00\x00\x00\x00\x00

      202103241000000001   column=detail:user_id, timestamp=2021-12-16T21:30:37.954, value=0001

      202103241000000001   column=detail:user_name, timestamp=2021-12-16T21:30:37.954, value=Alice

      202103241606060001   column=detail:area_id, timestamp=2021-12-16T21:30:44.842, value=330106

      202103241606060001   column=detail:order_channel, timestamp=2021-12-16T21:30:44.842, value=appShop

      202103241606060001   column=detail:order_time, timestamp=2021-12-16T21:30:44.842, value=2021-03-24 16:06:06

      202103241606060001   column=detail:pay_amount, timestamp=2021-12-16T21:30:44.842, value=@i\x00\x00\x00\x00\x00\x00

      202103241606060001   column=detail:pay_time, timestamp=2021-12-16T21:30:44.842, value=2021-03-24 16:10:06

      202103241606060001   column=detail:real_pay, timestamp=2021-12-16T21:30:44.842, value=@f\x80\x00\x00\x00\x00\x00

      202103241606060001   column=detail:user_id, timestamp=2021-12-16T21:30:44.842, value=0001

      202103241606060001   column=detail:user_name, timestamp=2021-12-16T21:30:44.842, value=Alice

      202103251202020001   column=detail:area_id, timestamp=2021-12-16T21:30:52.181, value=330110

      202103251202020001   column=detail:order_channel, timestamp=2021-12-16T21:30:52.181, value=miniAppShop

      202103251202020001   column=detail:order_time, timestamp=2021-12-16T21:30:52.181, value=2021-03-25 12:02:02

      202103251202020001   column=detail:pay_amount, timestamp=2021-12-16T21:30:52.181, value=@N\x00\x00\x00\x00\x00\x00

      202103251202020001   column=detail:pay_time, timestamp=2021-12-16T21:30:52.181, value=2021-03-25 12:03:00

      202103251202020001   column=detail:real_pay, timestamp=2021-12-16T21:30:52.181, value=@N\x00\x00\x00\x00\x00\x00

      202103251202020001   column=detail:user_id, timestamp=2021-12-16T21:30:52.181, value=0002

      202103251202020001   column=detail:user_name, timestamp=2021-12-16T21:30:52.181, value=Bob

FAQ
---

Q: What should I do if the Flink job execution fails and the log contains the following error information?

.. code-block::

   org.apache.zookeeper.ClientCnxn$SessionTimeoutException: Client session timed out, have not heard from server in 90069ms for connection id 0x0

A: The datasource connection is not bound or the binding fails. Configure the datasource connection or configure the security group of the Kafka cluster to allow access from the DLI queue.
