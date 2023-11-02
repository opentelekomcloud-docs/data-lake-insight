:original_name: dli_08_0390.html

.. _dli_08_0390:

Upsert Kafka Source Table
=========================

Function
--------

Apache Kafka is a fast, scalable, and fault-tolerant distributed message publishing and subscription system. It delivers high throughput and built-in partitions and provides data replicas and fault tolerance. Apache Kafka is applicable to scenarios of handling massive messages.

As a source, the upsert-kafka connector produces a changelog stream, where each data record represents an update or delete event. More precisely, the value in a data record is interpreted as an UPDATE of the last value for the same key, if any (if a corresponding key does not exist yet, the UPDATE will be considered an INSERT). Using the table analogy, a data record in a changelog stream is interpreted as an UPSERT, also known as INSERT/UPDATE, because any existing row with the same key is overwritten. Also, null values are interpreted in a special way: A record with a null value represents a DELETE.

Prerequisites
-------------

An enhanced datasource connection has been created for DLI to connect to Kafka clusters, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

Precautions
-----------

-  When creating a Flink OpenSource SQL job, you need to set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.
-  The Upsert Kafka always works in the upsert fashion and requires to define the primary key in the DDL. With the assumption that records with the same key should be ordered in the same partition, the primary key semantic on the changelog source means the materialized changelog is unique on the primary keys. The primary key definition will also control which fields should end up in Kafka's key.
-  Because the connector is working in upsert mode, the last record on the same key will take effect when reading back as a source.
-  For details about how to use data types, see section :ref:`Format <dli_08_0407>`.

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
   | connector                    | Yes         | None          | String      | Connector to be used. Set this parameter to **upsert-kafka**.                                                                                                                                                                                                                                                                                                                    |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | topic                        | Yes         | None          | String      | Kafka topic name.                                                                                                                                                                                                                                                                                                                                                                |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.bootstrap.servers | Yes         | None          | String      | Comma separated list of Kafka brokers.                                                                                                                                                                                                                                                                                                                                           |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.format                   | Yes         | None          | String      | Format used to deserialize and serialize the key part of Kafka messages. The key fields are specified by the **PRIMARY KEY** syntax. The following formats are supported:                                                                                                                                                                                                        |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | -  csv                                                                                                                                                                                                                                                                                                                                                                           |
   |                              |             |               |             | -  json                                                                                                                                                                                                                                                                                                                                                                          |
   |                              |             |               |             | -  avro                                                                                                                                                                                                                                                                                                                                                                          |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | Refer to :ref:`Format <dli_08_0407>` for more details and format parameters.                                                                                                                                                                                                                                                                                                     |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.fields-prefix            | No          | None          | String      | Defines a custom prefix for all fields of the key format to avoid name clashes with fields of the value format.                                                                                                                                                                                                                                                                  |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | By default, the prefix is empty. If a custom prefix is defined, both the table schema and **key.fields** will work with prefixed names. When constructing the data type of the key format, the prefix will be removed and the non-prefixed names will be used within the key format. Note that this option requires that **value.fields-include** must be set to **EXCEPT_KEY**. |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.format                 | Yes         | None          | String      | Format used to deserialize and serialize the value part of Kafka messages. The following formats are supported:                                                                                                                                                                                                                                                                  |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | -  csv                                                                                                                                                                                                                                                                                                                                                                           |
   |                              |             |               |             | -  json                                                                                                                                                                                                                                                                                                                                                                          |
   |                              |             |               |             | -  avro                                                                                                                                                                                                                                                                                                                                                                          |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | Refer to :ref:`Format <dli_08_0407>` for more details and format parameters.                                                                                                                                                                                                                                                                                                     |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.fields-include         | Yes         | ALL           | String      | Controls which fields should appear in the value part. Possible values are:                                                                                                                                                                                                                                                                                                      |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | -  **ALL**: All fields in the schema, including the primary key field, are included in the value part.                                                                                                                                                                                                                                                                           |
   |                              |             |               |             | -  **EXCEPT_KEY**: All the fields of the table schema are included, except the primary key field.                                                                                                                                                                                                                                                                                |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.\*                | No          | None          | String      | This option can set and pass arbitrary Kafka configurations.                                                                                                                                                                                                                                                                                                                     |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | The suffix to **properties.** must match the parameter defined in `Kafka Configuration documentation <https://kafka.apache.org/documentation/#configuration>`__. Flink will remove the **properties.** key prefix and pass the transformed key and value to the underlying KafkaClient.                                                                                          |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | For example, you can disable automatic topic creation via **'properties.allow.auto.create.topics' = 'false'**.                                                                                                                                                                                                                                                                   |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                  |
   |                              |             |               |             | But there are some configurations that do not support to set, because Flink will override them, for example, **'key.deserializer'** and **'value.deserializer'**.                                                                                                                                                                                                                |
   +------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

In this example, data is read from the Kafka data source and written to the Print result table. The procedure is as follows:

#. Create an enhanced datasource connection in the VPC and subnet where Kafka locates, and bind the connection to the required Flink elastic resource pool.

#. Set Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Kafka address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

   When you create a job, set **Flink Version** to **1.12** on the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

   .. code-block::

      CREATE TABLE upsertKafkaSource (
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
        'key.format' = 'csv',
        'value.format' = 'json'
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
        area_id string,
        PRIMARY KEY (order_id) NOT ENFORCED
      ) WITH (
        'connector' = 'print'
      );

      INSERT INTO printSink
      SELECT * FROM upsertKafkaSource;

#. Insert the following data to the specified topics in Kafka. (Note: Specify the key when inserting data to Kafka.)

   .. code-block::

      {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

      {"order_id":"202103251505050001", "order_channel":"qqShop", "order_time":"2021-03-25 15:05:05", "pay_amount":"500.00", "real_pay":"400.00", "pay_time":"2021-03-25 15:10:00", "user_id":"0003", "user_name":"Cindy", "area_id":"330108"}


      {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

#. Perform the following operations to view the output:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **taskmanager.out** file, and view result logs.

   The data result is as follows:

   .. code-block::

      +I(202103251202020001,miniAppShop,2021-03-2512:02:02,60.0,60.0,2021-03-2512:03:00,0002,Bob,330110)
      +I(202103251505050001,qqShop,2021-03-2515:05:05,500.0,400.0,2021-03-2515:10:00,0003,Cindy,330108)
      -U(202103251202020001,miniAppShop,2021-03-2512:02:02,60.0,60.0,2021-03-2512:03:00,0002,Bob,330110)
      +U(202103251202020001,miniAppShop,2021-03-2512:02:02,60.0,60.0,2021-03-2512:03:00,0002,Bob,330110)

FAQ
---

None
