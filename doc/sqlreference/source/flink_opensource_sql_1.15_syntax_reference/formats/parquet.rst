:original_name: dli_08_15025.html

.. _dli_08_15025:

Parquet
=======

Function
--------

The Apache Parquet format allows to read and write Parquet data. For details, see `Parquet Format <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/formats/parquet/>`__.

Supported Connectors
--------------------

-  FileSystem

Parameter Description
---------------------

.. table:: **Table 1** Parameters

   +----------------------+-----------+---------------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter            | Mandatory | Default Value | Data Type | Description                                                                                                                                                        |
   +======================+===========+===============+===========+====================================================================================================================================================================+
   | format               | Yes       | None          | String    | Specify what format to use, here should be **parquet**.                                                                                                            |
   +----------------------+-----------+---------------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | parquet.utc-timezone | No        | false         | Boolean   | Use UTC timezone or local timezone to the conversion between epoch time and LocalDateTime. Hive 0.x/1.x/2.x use local timezone. But Hive 3.\ *x* use UTC timezone. |
   +----------------------+-----------+---------------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Data Type Mapping
-----------------

Currently, Parquet format type mapping is compatible with Apache Hive, but different with Apache Spark:

-  Timestamp: mapping timestamp type to int96 whatever the precision is.
-  Decimal: mapping decimal type to fixed length byte array according to the precision.

The following table lists the type mapping from Flink type to Parquet type.

Note that currently only writing is supported for composite data types (Array, Map, and Row), while reading is not supported.

.. table:: **Table 2** Data type mapping

   =================== ==================== ====================
   Flink SQL Type      Parquet Type         Parquet Logical Type
   =================== ==================== ====================
   CHAR/VARCHAR/STRING BINARY               UTF8
   BOOLEAN             BOOLEAN              ``-``
   BINARY/VARBINARY    BINARY               ``-``
   DECIMAL             FIXED_LEN_BYTE_ARRAY DECIMAL
   TINYINT             INT32                INT_8
   SMALLINT            INT32                INT_16
   INT                 INT32                ``-``
   BIGINT              INT64                ``-``
   FLOAT               FLOAT                ``-``
   DOUBLE              DOUBLE               ``-``
   DATE                INT32                DATE
   TIME                INT32                TIME_MILLIS
   TIMESTAMP           INT96                ``-``
   ARRAY               ``-``                LIST
   MAP                 ``-``                MAP
   ROW                 ``-``                STRUCT
   =================== ==================== ====================

Example
-------

Use Kafka to send data and output the data to Print.

#. Create a datasource connection for the communication with the VPC and subnet where Kafka locates and bind the connection to the queue. Set a security group and inbound rule to allow access of the queue and test the connectivity of the queue using the Kafka IP address. For example, locate a general-purpose queue where the job runs and choose **More** > **Test Address Connectivity** in the **Operation** column. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job and enable checkpointing. Copy the following statement and submit the job:

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
        'topic-pattern' = 'kafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );


      CREATE TABLE sink (
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
        'connector' = 'filesystem',
        'format' = 'parquet',
        'path' = 'obs://xx'
      );
      insert into sink select * from kafkaSource;

#. Insert the following data into the source Kafka topic:

   .. code-block::

      202103251505050001,appShop,2021-03-25 15:05:05,500.00,400.00,2021-03-25 15:10:00,0003,Cindy,330108

      202103241606060001,appShop,2021-03-24 16:06:06,200.00,180.00,2021-03-24 16:10:06,0001,Alice,330106

#. Read the Parquet file in the OBS path configured in the sink table. The data results are as follows:

   .. code-block::

      202103251202020001, miniAppShop, 2021-03-25 12:02:02, 60.0, 60.0, 2021-03-25 12:03:00, 0002, Bob, 330110

      202103241606060001, appShop, 2021-03-24 16:06:06, 200.0, 180.0, 2021-03-24 16:10:06, 0001, Alice, 330106
