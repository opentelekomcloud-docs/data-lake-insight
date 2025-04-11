:original_name: dli_08_15043.html

.. _dli_08_15043:

Source Table
============

Function
--------

Create a source stream to obtain data from HBase as input for jobs. HBase is a column-oriented distributed cloud storage system that features enhanced reliability, excellent performance, and elastic scalability. It applies to the storage of massive amounts of data and distributed computing. You can use HBase to build a storage system capable of storing TB- or even PB-level data. With HBase, you can filter and analyze data with ease and get responses in milliseconds, rapidly mining data value. DLI can read data from HBase for filtering, analysis, and data dumping.

Prerequisites
-------------

-  An enhanced datasource connection has been created for DLI to connect to HBase, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

-  **If MRS HBase is used, IP addresses of all hosts in the MRS cluster have been added to host information of the enhanced datasource connection.**

   For details, see "Modifying Host Information" in *Data Lake Insight User Guide*.

Caveats
-------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.

-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .

-  The column families in created HBase source table must be declared as the ROW type, the field names map the column family names, and the nested field names map the column qualifier names.

   There is no need to declare all the families and qualifiers in the schema. Users can declare what is used in the query. Except the ROW type fields, the single atomic type field (for example, STRING or BIGINT) will be recognized as the HBase rowkey. The rowkey field can be an arbitrary name, but should be quoted using backticks if it is a reserved keyword.

Syntax
------

.. code-block::

   create table hbaseSource (
     attr_name attr_type
     (',' attr_name attr_type)*
     (',' watermark for rowtime_column_name as watermark-strategy_expression)
     ','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
     'connector' = 'hbase-2.2',
     'table-name' = '',
     'zookeeper.quorum' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter              | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                                                                                                                            |
   +========================+=============+===============+=============+========================================================================================================================================================================================================================================================================+
   | connector              | Yes         | None          | String      | Connector to be used. Set this parameter to **hbase-2.2**.                                                                                                                                                                                                             |
   +------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table-name             | Yes         | None          | String      | Name of the HBase table to connect.                                                                                                                                                                                                                                    |
   +------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | zookeeper.quorum       | Yes         | None          | String      | HBase ZooKeeper quorum, in the format of "ZookeeperAddress:ZookeeperPort".                                                                                                                                                                                             |
   |                        |             |               |             |                                                                                                                                                                                                                                                                        |
   |                        |             |               |             | The following uses an MRS HBase cluster as an example to describe how to obtain the IP address and port number of ZooKeeper used by this parameter:                                                                                                                    |
   |                        |             |               |             |                                                                                                                                                                                                                                                                        |
   |                        |             |               |             | -  On MRS Manager, choose **Cluster** and click the name of the desired cluster. Choose **Services** > **ZooKeeper** > **Instance**, and obtain the IP address of the ZooKeeper instance.                                                                              |
   |                        |             |               |             | -  On MRS Manager, choose **Cluster** and click the name of the desired cluster. Choose **Services** > **ZooKeeper** > **Configurations** > **All Configurations**, search for the **clientPort** parameter, and obtain its value, that is, the ZooKeeper port number. |
   +------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | zookeeper.znode.parent | No          | /hbase        | String      | Root directory in ZooKeeper. The default value is **/hbase**.                                                                                                                                                                                                          |
   +------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | null-string-literal    | No          | None          | String      | Representation for null values for string fields.                                                                                                                                                                                                                      |
   |                        |             |               |             |                                                                                                                                                                                                                                                                        |
   |                        |             |               |             | HBase source encodes/decodes empty bytes as null values for all types except the string type.                                                                                                                                                                          |
   +------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Data Type Mapping
-----------------

HBase stores all data as byte arrays. The data needs to be serialized and deserialized during read and write operations.

When serializing and de-serializing, Flink HBase connector uses utility class **org.apache.hadoop.hbase.util.Bytes** provided by HBase (Hadoop) to convert Flink data types to and from byte arrays.

Flink HBase connector encodes null values to empty bytes, and decodes empty bytes to null values for all data types except the string type. For string type, the null literal is determined by the **null-string-literal** option.

.. table:: **Table 2** Data type mapping

   +-----------------------------------+---------------------------------------------------------------+
   | Flink SQL Type                    | HBase Conversion                                              |
   +===================================+===============================================================+
   | CHAR/VARCHAR/STRING               | byte[] toBytes(String s)                                      |
   |                                   |                                                               |
   |                                   | String toString(byte[] b)                                     |
   +-----------------------------------+---------------------------------------------------------------+
   | BOOLEAN                           | byte[] toBytes(boolean b)                                     |
   |                                   |                                                               |
   |                                   | boolean toBoolean(byte[] b)                                   |
   +-----------------------------------+---------------------------------------------------------------+
   | BINARY/VARBINARY                  | Returns byte[] as is.                                         |
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
   | MAP/MULTISET                      | Not supported                                                 |
   +-----------------------------------+---------------------------------------------------------------+
   | ROW                               | Not supported                                                 |
   +-----------------------------------+---------------------------------------------------------------+

Example
-------

In this example, data is read from the HBase data source and written to the Print result table. (The HBase version used in this example is 2.2.3.)

#. Create an enhanced datasource connection in the VPC and subnet where HBase locates, and bind the connection to the required Flink queue.

#. Set HBase cluster security groups and add inbound rules to allow access from the Flink job queue. Test the connectivity using the HBase address. If the connection passes the test, it is bound to the queue.

#. Use the HBase shell to create HBase table **order** that has only one column family **detail**. The creation statement is as follows:

   .. code-block::

      create 'order', {NAME => 'detail'}

#. Run the following command in the HBase shell to insert a data record:

   .. code-block::

      put 'order', '202103241000000001', 'detail:order_channel','webShop'
      put 'order', '202103241000000001', 'detail:order_time','2021-03-24 10:00:00'
      put 'order', '202103241000000001', 'detail:pay_amount','100.00'
      put 'order', '202103241000000001', 'detail:real_pay','100.00'
      put 'order', '202103241000000001', 'detail:pay_time','2021-03-24 10:02:03'
      put 'order', '202103241000000001', 'detail:user_id','0001'
      put 'order', '202103241000000001', 'detail:user_name','Alice'
      put 'order', '202103241000000001', 'detail:area_id','330106'

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job. The job script uses the HBase data source and the Print result table.

   When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

   .. code-block::

      create table hbaseSource (
        order_id string,-- Indicates the unique rowkey.
        detail Row( -- Indicates the column family.
          order_channel string,
          order_time string,
          pay_amount string,
          real_pay string,
          pay_time string,
          user_id string,
          user_name string,
          area_id string),
        primary key (order_id) not enforced
      ) with (
        'connector' = 'hbase-2.2',
         'table-name' = 'order',
         'zookeeper.quorum' = 'ZookeeperAddress:ZookeeperPort'
      ) ;

      create table printSink (
        order_id string,
        order_channel string,
        order_time string,
        pay_amount string,
        real_pay string,
        pay_time string,
        user_id string,
        user_name string,
        area_id string
      ) with (
       'connector' = 'print'
      );

      insert into printSink select order_id, detail.order_channel,detail.order_time,detail.pay_amount,detail.real_pay,
      detail.pay_time,detail.user_id,detail.user_name,detail.area_id from hbaseSource;

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **taskmanager.out** file, and view result logs.

   The data result is as follows:

   .. code-block::

      +I(202103241000000001,webShop,2021-03-24 10:00:00,100.00,100.00,2021-03-24 10:02:03,0001,Alice,330106)

FAQ
---

-  Q: What should I do if the Flink job execution fails and the log contains the following error information?

   .. code-block::

      java.lang.IllegalArgumentException: offset (0) + length (8) exceed the capacity of the array: 6

   A: If data in the HBase table is imported in other modes, the data is represented in the string format. Therefore, this error is reported when other data formats are used. Change the type of the non-string fields in the HBase source table created by Flink to the string format.

-  Q: What should I do if the Flink job execution fails and the log contains the following error information?

   .. code-block::

      org.apache.zookeeper.ClientCnxn$SessionTimeoutException: Client session timed out, have not heard from server in 90069ms for connection id 0x0

   A: The datasource connection is not bound, the binding fails, or the security group of the HBase cluster is not configured to allow access from the network segment of the DLI queue. Configure the datasource connection or configure the security group of the HBase cluster to allow access from the DLI queue.
