:original_name: dli_08_15016.html

.. _dli_08_15016:

Avro
====

Function
--------

Apache Avro is supported for you to read and write Avro data based on an Avro schema with Flink. The Avro schema is derived from the table schema.

For details, see `Avro Format <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/formats/avro/>`__.

Supported Connectors
--------------------

-  Kafka
-  Upsert Kafka
-  FileSystem

Parameters
----------

.. table:: **Table 1** Parameters

   +------------+-----------+---------------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Default value | Type   | Description                                                                                                                                                               |
   +============+===========+===============+========+===========================================================================================================================================================================+
   | format     | Yes       | None          | String | Format to be used. Set the value to **avro**.                                                                                                                             |
   +------------+-----------+---------------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avro.codec | No        | None          | String | For Filesystem only, the compression codec for avro. Snappy compression as default. The valid enumerations are: **null**, **deflate**, **snappy**, **bzip2**, and **xz**. |
   +------------+-----------+---------------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Data Type Mapping
-----------------

Currently, the Avro schema is derived from the table schema and cannot be explicitly defined. The following table lists mappings between Flink to Avro types.

In addition to the following types, Flink supports reading/writing nullable types. Flink maps nullable types to Avro **union(something, null)**, where **something** is an Avro type converted from Flink type.

.. table:: **Table 2** Data type mapping

   +-------------------------------------------------------------------+-----------+-------------------+
   | Flink SQL Type                                                    | Avro Type | Avro Logical Type |
   +===================================================================+===========+===================+
   | CHAR/VARCHAR/STRING                                               | String    | ``-``             |
   +-------------------------------------------------------------------+-----------+-------------------+
   | BOOLEAN                                                           | Boolean   | ``-``             |
   +-------------------------------------------------------------------+-----------+-------------------+
   | BINARY/VARBINARY                                                  | bytes     | ``-``             |
   +-------------------------------------------------------------------+-----------+-------------------+
   | DECIMAL                                                           | fixed     | decimal           |
   +-------------------------------------------------------------------+-----------+-------------------+
   | TINYINT                                                           | int       | ``-``             |
   +-------------------------------------------------------------------+-----------+-------------------+
   | SMALLINT                                                          | int       | ``-``             |
   +-------------------------------------------------------------------+-----------+-------------------+
   | INT                                                               | int       | ``-``             |
   +-------------------------------------------------------------------+-----------+-------------------+
   | BIGINT                                                            | long      | ``-``             |
   +-------------------------------------------------------------------+-----------+-------------------+
   | FLOAT                                                             | float     | ``-``             |
   +-------------------------------------------------------------------+-----------+-------------------+
   | DOUBLE                                                            | double    | ``-``             |
   +-------------------------------------------------------------------+-----------+-------------------+
   | DATE                                                              | int       | date              |
   +-------------------------------------------------------------------+-----------+-------------------+
   | TIME                                                              | int       | time-millis       |
   +-------------------------------------------------------------------+-----------+-------------------+
   | TIMESTAMP                                                         | long      | timestamp-millis  |
   +-------------------------------------------------------------------+-----------+-------------------+
   | ARRAY                                                             | array     | ``-``             |
   +-------------------------------------------------------------------+-----------+-------------------+
   | MAP (keys must be of the string, char, or varchar type.)          | map       | ``-``             |
   +-------------------------------------------------------------------+-----------+-------------------+
   | MULTISET (elements must be of the string, char, or varchar type.) | map       | ``-``             |
   +-------------------------------------------------------------------+-----------+-------------------+
   | ROW                                                               | record    | ``-``             |
   +-------------------------------------------------------------------+-----------+-------------------+

Example
-------

Read data from Kafka, deserialize the data to the Avro format, and outputs the data to Print.

#. Create a datasource connection for access to the VPC and subnet where Kafka locates and bind the connection to the queue. Set a security group and inbound rule to allow access of the queue and test the connectivity of the queue using the Kafka IP address. For example, locate a general-purpose queue where the job runs and choose **More** > **Test Address Connectivity** in the **Operation** column. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job and select Flink 1.15. Copy the following statement and submit the job:

   .. code-block::

      CREATE TABLE kafkaSource (
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
        'topic' = 'kafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'avro'
      );


      CREATE TABLE printSink (
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
        'connector' = 'print'
      );
      insert into printSink select * from kafkaSource;

#. Insert the following data to Kafka using Avro data serialization:

   .. code-block::

      {"order_id":"202103241000000001","order_channel":"webShop","order_time":"2021-03-24 10:00:00","pay_amount":100.0,"real_pay":100.0,"pay_time":"2021-03-24 10:02:03","user_id":"0001","user_name":"Alice","area_id":"330106"}

      {"order_id":"202103241606060001","order_channel":"appShop","order_time":"2021-03-24 16:06:06","pay_amount":200.0,"real_pay":180.0,"pay_time":"2021-03-24 16:10:06","user_id":"0001","user_name":"Alice","area_id":"330106"}

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **.out** file, and view result logs.

   .. code-block::

      +I[202103241000000001, webShop, 2021-03-24 10:00:00, 100.0, 100.0, 2021-03-24 10:02:03, 0001, Alice, 330106]
      +I[202103241606060001, appShop, 2021-03-24 16:06:06, 200.0, 180.0, 2021-03-24 16:10:06, 0001, Alice, 330106]
