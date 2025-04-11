:original_name: dli_08_15065.html

.. _dli_08_15065:

Upsert Kafka
============

Function
--------

Apache Kafka is a fast, scalable, and fault-tolerant distributed message publishing and subscription system. It delivers high throughput and built-in partitions and provides data replicas and fault tolerance. Apache Kafka is applicable to scenarios of handling massive messages. The Upsert Kafka connector allows for reading data from and writing data into Kafka topics in the upsert fashion. Source tables and result tables are supported.

-  As a source, the upsert-kafka connector produces a changelog stream, where each data record represents an update or delete event.

   The value in a data record is interpreted as an UPDATE of the last value for the same key, if any (if a corresponding key does not exist yet, the UPDATE will be considered an INSERT). Using the table analogy, a data record in a changelog stream is interpreted as an UPSERT, also known as INSERT/UPDATE, because any existing row with the same key is overwritten. Also, null values are interpreted in a special way: A record with a null value represents a DELETE.

-  As a sink, the upsert-kafka connector can consume a changelog stream. It will write INSERT/UPDATE_AFTER data as normal Kafka messages value, and write DELETE data as Kafka messages with null values (indicate tombstone for the key). Flink will guarantee the message ordering on the primary key by partition data on the values of the primary key columns, so the UPDATE/DELETE messages on the same key will fall into the same partition.

.. table:: **Table 1** Supported types

   ===================== =============================
   Type                  Description
   ===================== =============================
   Supported Table Types Source table and result table
   ===================== =============================

Prerequisites
-------------

An enhanced datasource connection has been created for DLI to connect to Kafka clusters, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

Caveats
-------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .
-  The Upsert Kafka always works in the upsert fashion and requires to define the primary key in the DDL. With the assumption that records with the same key should be ordered in the same partition, the primary key semantic on the changelog source means the materialized changelog is unique on the primary keys. The primary key definition will also control which fields should end up in Kafka's key.
-  Because the connector is working in upsert mode, the last record on the same key will take effect when reading back as a source.
-  For details about how to use data types, see :ref:`Format <dli_08_15014>`.

Syntax
------

::

   create table kafkaTable(
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

Parameter Description
---------------------

.. table:: **Table 2** Parameters

   +------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                    | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                                                                                                                                                                                                                                 |
   +==============================+=============+===============+=============+=============================================================================================================================================================================================================================================================================================================================================================================+
   | connector                    | Yes         | None          | String      | Connector to be used. For the Upsert Kafka connector, set this parameter to **upsert-kafka**.                                                                                                                                                                                                                                                                               |
   +------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | topic                        | Yes         | None          | String      | Kafka topic name                                                                                                                                                                                                                                                                                                                                                            |
   +------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.bootstrap.servers | Yes         | None          | String      | Comma separated list of Kafka brokers                                                                                                                                                                                                                                                                                                                                       |
   +------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.format                   | Yes         | None          | String      | Format used to deserialize and serialize the key part of Kafka messages. The key fields are specified by the **PRIMARY KEY** syntax. The following formats are supported:                                                                                                                                                                                                   |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                             |
   |                              |             |               |             | -  csv                                                                                                                                                                                                                                                                                                                                                                      |
   |                              |             |               |             | -  json                                                                                                                                                                                                                                                                                                                                                                     |
   |                              |             |               |             | -  avro                                                                                                                                                                                                                                                                                                                                                                     |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                             |
   |                              |             |               |             | Refer to :ref:`Format <dli_08_15014>` for more details and format parameters.                                                                                                                                                                                                                                                                                               |
   +------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.fields-prefix            | No          | None          | String      | Defines a custom prefix for all fields of the key format to avoid name clashes with fields of the value format.                                                                                                                                                                                                                                                             |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                             |
   |                              |             |               |             | By default, the prefix is empty. If a custom prefix is defined, both the table schema and **key.fields** will work with prefixed names. When constructing the data type of the key format, the prefix will be removed and the non-prefixed names will be used within the key format. Note that this option requires that **value.fields-include** be set to **EXCEPT_KEY**. |
   +------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.format                 | Yes         | None          | String      | Format used to deserialize and serialize the value part of Kafka messages. The following formats are supported:                                                                                                                                                                                                                                                             |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                             |
   |                              |             |               |             | -  csv                                                                                                                                                                                                                                                                                                                                                                      |
   |                              |             |               |             | -  json                                                                                                                                                                                                                                                                                                                                                                     |
   |                              |             |               |             | -  avro                                                                                                                                                                                                                                                                                                                                                                     |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                             |
   |                              |             |               |             | Refer to :ref:`Format <dli_08_15014>` for more details and format parameters.                                                                                                                                                                                                                                                                                               |
   +------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.fields-include         | Yes         | ALL           | String      | Controls which fields should appear in the value part. Possible values are:                                                                                                                                                                                                                                                                                                 |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                             |
   |                              |             |               |             | -  **ALL**: All fields in the schema, including the primary key field, are included in the value part.                                                                                                                                                                                                                                                                      |
   |                              |             |               |             | -  **EXCEPT_KEY**: All the fields of the table schema are included, except the primary key field.                                                                                                                                                                                                                                                                           |
   +------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.\*                | No          | None          | String      | This option can set and pass arbitrary Kafka configurations.                                                                                                                                                                                                                                                                                                                |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                             |
   |                              |             |               |             | The suffix to **properties.** must match the parameter defined in `Kafka Configuration documentation <https://kafka.apache.org/documentation/#configuration>`__. Flink will remove the **properties.** key prefix and pass the transformed key and value to the underlying KafkaClient.                                                                                     |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                             |
   |                              |             |               |             | For example, you can disable automatic topic creation via **'properties.allow.auto.create.topics' = 'false'**.                                                                                                                                                                                                                                                              |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                             |
   |                              |             |               |             | But there are some configurations that do not support to set, because Flink will override them, for example, **'key.deserializer'** and **'value.deserializer'**.                                                                                                                                                                                                           |
   +------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.parallelism             | No          | None          | Integer     | Defines the parallelism of the Upsert Kafka sink operator. By default, the parallelism is determined by the framework: using the same parallelism as the upstream join operator.                                                                                                                                                                                            |
   +------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.buffer-flush.max-rows   | No          | 0             | Integer     | The max size of buffered records before flushing.                                                                                                                                                                                                                                                                                                                           |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                             |
   |                              |             |               |             | When the sink receives many updates on the same key, the buffer will retain the last record of the same key. This can help to reduce data shuffling and avoid possible tombstone messages to Kafka topic. Can be set to **0** to disable it.                                                                                                                                |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                             |
   |                              |             |               |             | By default, this is disabled. Note both **sink.buffer-flush.max-rows** and **sink.buffer-flush.interval** must be set to be greater than zero to enable sink buffer flushing.                                                                                                                                                                                               |
   +------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.buffer-flush.interval   | No          | 0             | Duration    | The flush interval mills, over this time, asynchronous threads will flush data. The unit can be millisecond (ms), second (s), minute (min), or hour (h). For example, **'sink.buffer-flush.interval'='10 ms'**.                                                                                                                                                             |
   |                              |             |               |             |                                                                                                                                                                                                                                                                                                                                                                             |
   |                              |             |               |             | By default, this is disabled. Note both **sink.buffer-flush.max-rows** and **sink.buffer-flush.interval** must be set to be greater than zero to enable sink buffer flushing.                                                                                                                                                                                               |
   +------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Metadata
--------

For a list of available metadata fields, see :ref:`Kafka Connector <dli_08_15058__section9326019161710>`.

Example
-------

-  **Example 1: This example reads data from a DMS Kafka data source and writes it to the Print result table.**

   #. Create an enhanced datasource connection in the VPC and subnet where Kafka locates, and bind the connection to the required Flink elastic resource pool.

   #. Set Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Kafka address. If the connection passes the test, it is bound to the queue.

   #. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

      When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

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

         INSERT INTO printSink SELECT * FROM upsertKafkaSource;

   #. Insert the following data to the specified topics in Kafka. (Note: Specify the key when inserting data to Kafka.)

      .. code-block::

         {"order_id":"202303251202020001", "order_channel":"miniAppShop", "order_time":"2023-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2023-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

         {"order_id":"202303251505050001", "order_channel":"appshop", "order_time":"2023-03-25 15:05:05", "pay_amount":"500.00", "real_pay":"400.00", "pay_time":"2023-03-25 15:10:00", "user_id":"0003", "user_name":"Cindy", "area_id":"330108"}

         {"order_id":"202303251202020001", "order_channel":"miniAppShop", "order_time":"2023-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2023-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330111"}

   #. View the **out** file of the TaskManager. The data results are as follows:

      .. code-block::

         +I(202303251202020001,miniAppShop,2023-03-2512:02:02,60.0,60.0,2023-03-2512:03:00,0002,Bob,330110)
         +I(202303251505050001,appshop,2023-03-25 15:05:05,500.0,400.0,2023-03-2515:10:00,0003,Cindy,330108)
         -U(202303251202020001,miniAppShop,2023-03-2512:02:02,60.0,60.0,2023-03-2512:03:00,0002,Bob,330110)
         +U(202303251202020001,miniAppShop,2023-03-2512:02:02,60.0,60.0,2023-03-2512:03:00,0002,Bob,330111)

-  **Example 2: This example retrieves DMS Kafka source topic data from a Kafka source table and writes it to a Kafka sink topic using Upsert Kafka result table.**

   #. Create an enhanced datasource connection in the VPC and subnet where Kafka locates, and bind the connection to the required Flink elastic resource pool.

   #. Set Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Kafka address. If the connection passes the test, it is bound to the queue.

   #. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

      When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

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
           'format' = 'json'
         );

         CREATE TABLE upsertKafkaSink (
           order_id string,
           order_channel string,
           order_time string,
           pay_amount double,
           real_pay double,
           pay_time string,
           user_id string,
           user_name string,
           area_id string,
           PRIMARY KEY(order_id) NOT ENFORCED
         ) WITH (
           'connector' = 'upsert-kafka',
           'topic' = 'KafkaTopic',
           'properties.bootstrap.servers' =  'KafkaAddress1:KafkaPort,KafkAddress2:KafkaPort',
           'key.format' = 'csv',
           'value.format' = 'json'
         );

         insert into upsertKafkaSink select * from orders;

   #. Connect to the Kafka cluster and send the following test data to the Kafka source topic:

      .. code-block::

         {"order_id":"202303251202020001", "order_channel":"miniAppShop", "order_time":"2023-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2023-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

         {"order_id":"202303251505050001", "order_channel":"appshop", "order_time":"2023-03-25 15:05:05", "pay_amount":"500.00", "real_pay":"400.00", "pay_time":"2023-03-25 15:10:00", "user_id":"0003", "user_name":"Cindy", "area_id":"330108"}

         {"order_id":"202303251202020001", "order_channel":"miniAppShop", "order_time":"2023-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2023-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330111"}

   #. Connect to the Kafka cluster and read data from the Kafka sink topic. The result is as follows:

      .. code-block::

         {"order_id":"202303251202020001", "order_channel":"miniAppShop", "order_time":"2023-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2023-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

         {"order_id":"202303251505050001", "order_channel":"appshop", "order_time":"2023-03-25 15:05:05", "pay_amount":"500.00", "real_pay":"400.00", "pay_time":"2023-03-25 15:10:00", "user_id":"0003", "user_name":"Cindy", "area_id":"330108"}

         {"order_id":"202303251202020001", "order_channel":"miniAppShop", "order_time":"2023-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2023-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330111"}

-  **Example 3: In this scenario, the MRS cluster has enabled Kerberos authentication and Kafka is using the SASL_PLAINTEXT protocol. Data is retrieved from a Kafka source table and written to the Print result table.**

   #. Create an enhanced datasource connection in the VPC and subnet where the MRS cluster locates, and bind the connection to the required Flink elastic resource pool.

   #. Set MRS cluster security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Kafka address. If the connection passes the test, it is bound to the queue.

   #. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

      When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

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
           PRIMARY KEY(order_id) NOT ENFORCED
         ) WITH (
           'connector' = 'upsert-kafka',
           'topic' = 'KafkaTopic',
           'properties.bootstrap.servers' =  'KafkaAddress1:KafkaPort,KafkAddress2:KafkaPort',
           'key.format' = 'csv',
           'value.format' = 'json',
           'properties.sasl.mechanism' = 'GSSAPI',
           'properties.security.protocol' = 'SASL_PLAINTEXT',
           'properties.sasl.kerberos.service.name' = 'kafka', -- Configured in MRS
           'properties.connector.auth.open' = 'true',
           'properties.connector.kerberos.principal' = 'username', --Username
           'properties.connector.kerberos.krb5' = 'obs://xx/krb5.conf', --krb5_conf path
           'properties.connector.kerberos.keytab' = 'obs://xx/user.keytab' --keytab path
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

         INSERT INTO printSink SELECT * FROM upsertKafkaSource;

   #. Insert the following data to the specified topics in Kafka. (Note: Specify the key when inserting data to Kafka.)

      .. code-block::

         {"order_id":"202303251202020001", "order_channel":"miniAppShop", "order_time":"2023-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2023-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

         {"order_id":"202303251505050001", "order_channel":"appshop", "order_time":"2023-03-25 15:05:05", "pay_amount":"500.00", "real_pay":"400.00", "pay_time":"2023-03-25 15:10:00", "user_id":"0003", "user_name":"Cindy", "area_id":"330108"}

         {"order_id":"202303251202020001", "order_channel":"miniAppShop", "order_time":"2023-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2023-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330111"}

   #. View the **out** file of the TaskManager. The data results are as follows:

      .. code-block::

         +I(202303251202020001,miniAppShop,2023-03-2512:02:02,60.0,60.0,2023-03-2512:03:00,0002,Bob,330110)
         +I(202303251505050001,appshop,2023-03-2515:05:05,500.0,400.0,2023-03-2515:10:00,0003,Cindy,330108)
         -U(202303251202020001,miniAppShop,2023-03-2512:02:02,60.0,60.0,2023-03-2512:03:00,0002,Bob,330110)
         +U(202303251202020001,miniAppShop,2023-03-2512:02:02,60.0,60.0,2023-03-2512:03:00,0002,Bob,330111)

FAQ
---

None
