:original_name: dli_08_0401.html

.. _dli_08_0401:

Upsert Kafka Result Table
=========================

Function
--------

Apache Kafka is a fast, scalable, and fault-tolerant distributed message publishing and subscription system. It delivers high throughput and built-in partitions and provides data replicas and fault tolerance. Apache Kafka is applicable to scenarios of handling massive messages. DLI outputs the Flink job output data to Kafka in upsert mode.

The Upsert Kafka connector allows for reading data from and writing data into Kafka topics in the upsert fashion.

As a sink, the Upsert Kafka connector can consume a changelog stream. It will write INSERT/UPDATE_AFTER data as normal Kafka messages value, and write DELETE data as Kafka messages with null values (indicate tombstone for the key). Flink will guarantee the message ordering on the primary key by partition data on the values of the primary key columns, so the UPDATE/DELETE messages on the same key will fall into the same partition.

Prerequisites
-------------

-  You have created a Kafka cluster.
-  An enhanced datasource connection has been created for DLI to connect to Kafka clusters, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

Precautions
-----------

-  When creating a Flink OpenSource SQL job, you need to set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.
-  For details about how to use data types, see section :ref:`Format <dli_08_0407>`.
-  The Upsert Kafka always works in the upsert fashion and requires to define the primary key in the DDL.
-  By default, an Upsert Kafka sink ingests data with at-least-once guarantees into a Kafka topic if the query is executed with checkpointing enabled. This means that Flink may write duplicate records with the same key into the Kafka topic. Therefore, the Upsert Kafka connector achieves idempotent writes.

Syntax
------

::

   create table kafkaSource(
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
     'connector' = 'upsert-kafka',
     'topic' = '',
     'properties.bootstrap.servers' = '',
     'key.format' = '',
     'value.format' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                    | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                                                                                                                                                                                                                                      |
   +==============================+=============+===============+=============+==================================================================================================================================================================================================================================================================================================================================================================================+
   | connector                    | Yes         | (none)        | String      | Connector to be used. Set this parameter to **upsert-kafka**.                                                                                                                                                                                                                                                                                                                    |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | topic                        | Yes         | (none)        | String      | Kafka topic name.                                                                                                                                                                                                                                                                                                                                                                |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.bootstrap.servers | Yes         | (none)        | String      | Comma separated list of Kafka brokers.                                                                                                                                                                                                                                                                                                                                           |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.format                   | Yes         | (none)        | String      | Format used to deserialize and serialize the key part of Kafka messages. The key fields are specified by the **PRIMARY KEY** syntax. The following formats are supported:                                                                                                                                                                                                        |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | -  csv                                                                                                                                                                                                                                                                                                                                                                           |
   |                              |             |               |             | -  json                                                                                                                                                                                                                                                                                                                                                                          |
   |                              |             |               |             | -  avro                                                                                                                                                                                                                                                                                                                                                                          |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | Refer to :ref:`Format <dli_08_0407>` for more details and format parameters.                                                                                                                                                                                                                                                                                                     |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.fields-prefix            | No          | (none)        | String      | Defines a custom prefix for all fields of the key format to avoid name clashes with fields of the value format.                                                                                                                                                                                                                                                                  |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | By default, the prefix is empty. If a custom prefix is defined, both the table schema and **key.fields** will work with prefixed names. When constructing the data type of the key format, the prefix will be removed and the non-prefixed names will be used within the key format. Note that this option requires that **value.fields-include** must be set to **EXCEPT_KEY**. |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.format                 | Yes         | (none)        | String      | Format used to deserialize and serialize the value part of Kafka messages. The following formats are supported:                                                                                                                                                                                                                                                                  |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | -  csv                                                                                                                                                                                                                                                                                                                                                                           |
   |                              |             |               |             | -  json                                                                                                                                                                                                                                                                                                                                                                          |
   |                              |             |               |             | -  avro                                                                                                                                                                                                                                                                                                                                                                          |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | Refer to :ref:`Format <dli_08_0407>` for more details and format parameters.                                                                                                                                                                                                                                                                                                     |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.fields-include         | No          | 'ALL'         | String      | Controls which fields should appear in the value part. Options:                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | -  **ALL**: All fields in the schema, including the primary key field, are included in the value part.                                                                                                                                                                                                                                                                           |
   |                              |             |               |             | -  **EXCEPT_KEY**: All the fields of the table schema are included, except the primary key field.                                                                                                                                                                                                                                                                                |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.parallelism             | No          | (none)        | Interger    | Defines the parallelism of the Upsert Kafka sink operator. By default, the parallelism is determined by the framework using the same parallelism of the upstream chained operator.                                                                                                                                                                                               |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.\*                | No          | (none)        | String      | This option can set and pass arbitrary Kafka configurations.                                                                                                                                                                                                                                                                                                                     |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | The suffix of this parameter must match the parameter defined in `Kafka Configuration documentation <https://kafka.apache.org/documentation/#configuration>`__. Flink will remove the **properties.** key prefix and pass the transformed key and value to the underlying KafkaClient.                                                                                           |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | For example, you can disable automatic topic creation via **'properties.allow.auto.create.topics' = 'false'**. But there are some configurations that do not support to set, because Flink will override them, for example, **'key.deserializer'** and **'value.deserializer'**.                                                                                                 |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

In this example, Kafka source topic data is read from the Kafka source table and written to the Kafka sink topic through the Upsert Kafka result table.

#. Create an enhanced datasource connection in the VPC and subnet where Kafka locates, and bind the connection to the required Flink elastic resource pool.

#. Set Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Kafka address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

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
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        "format" = "json"
      );
      CREATE TABLE UPSERTKAFKASINK (
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string,
        PRIMARY KEY (order_id) NOT ENFORCED
      ) WITH (
        'connector' = 'upsert-kafka',
        'topic' = 'KafkaTopic',
        'properties.bootstrap.servers' =  'KafkaAddress1:KafkaPort,KafkAddress2:KafkaPort',
        'key.format' = 'json',
        'value.format' = 'json'
      );
      insert into UPSERTKAFKASINK
      select * from orders;

#. Connect to the Kafka cluster and send the following test data to the Kafka source topic:

   .. code-block::

      {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

      {"order_id":"202103251505050001", "order_channel":"qqShop", "order_time":"2021-03-25 15:05:05", "pay_amount":"500.00", "real_pay":"400.00", "pay_time":"2021-03-25 15:10:00", "user_id":"0003", "user_name":"Cindy", "area_id":"330108"}

      {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

#. Connect to the Kafka cluster and read data from the Kafka sink topic. The result is as follows:

   .. code-block::

      {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

      {"order_id":"202103251505050001", "order_channel":"qqShop", "order_time":"2021-03-25 15:05:05", "pay_amount":"500.00", "real_pay":"400.00", "pay_time":"2021-03-25 15:10:00", "user_id":"0003", "user_name":"Cindy", "area_id":"330108"}

      {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

FAQ
---

None
