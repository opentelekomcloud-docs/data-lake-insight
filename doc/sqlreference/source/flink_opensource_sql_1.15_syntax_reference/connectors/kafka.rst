:original_name: dli_08_15058.html

.. _dli_08_15058:

Kafka
=====

Function
--------

The Kafka connector allows for reading data from and writing data into Kafka topics.

Apache Kafka is a fast, scalable, and fault-tolerant distributed message publishing and subscription system. It delivers high throughput and built-in partitions and provides data replicas and fault tolerance. Apache Kafka is applicable to scenarios of handling massive messages.

.. table:: **Table 1** Supported types

   +-----------------------------------+--------------------------------------+
   | Type                              | Description                          |
   +===================================+======================================+
   | Supported Table Types             | Source table and result table        |
   +-----------------------------------+--------------------------------------+
   | Supported Data Formats            | :ref:`CSV <dli_08_15019>`            |
   |                                   |                                      |
   |                                   | :ref:`JSON <dli_08_15021>`           |
   |                                   |                                      |
   |                                   | :ref:`Apache Avro <dli_08_15016>`    |
   |                                   |                                      |
   |                                   | :ref:`Confluent Avro <dli_08_15018>` |
   |                                   |                                      |
   |                                   | :ref:`Debezium CDC <dli_08_15020>`   |
   |                                   |                                      |
   |                                   | :ref:`Canal CDC <dli_08_15017>`      |
   |                                   |                                      |
   |                                   | :ref:`Maxwell CDC <dli_08_15022>`    |
   |                                   |                                      |
   |                                   | :ref:`OGG CDC <dli_08_15023>`        |
   |                                   |                                      |
   |                                   | :ref:`Raw <dli_08_15026>`            |
   +-----------------------------------+--------------------------------------+

Prerequisites
-------------

-  You have created a Kafka cluster.
-  An enhanced datasource connection has been created for DLI to connect to Kafka clusters, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

Caveats
-------

-  For details, see `Apache Kafka SQL Connector <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/kafka/>`__.
-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .
-  Fields in the **with** parameter can only be enclosed in single quotes.
-  For details about how to use data types when creating tables, see :ref:`Format <dli_08_15014>`.
-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .

Syntax
------

::

   create table kafkaSource(
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
     (',' WATERMARK FOR rowtime_column_name AS watermark-strategy_expression)
   )
   with (
     'connector' = 'kafka',
     'topic' = '',
     'properties.bootstrap.servers' = '',
     'properties.group.id' = '',
     'scan.startup.mode' = '',
     'format' = ''
   );

Source Table Parameter Description
----------------------------------

.. table:: **Table 2** Source table parameters

   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                               | Mandatory                                    | Default Value | Data Type                          | Description                                                                                                                                                                                                                               |
   +=========================================+==============================================+===============+====================================+===========================================================================================================================================================================================================================================+
   | connector                               | Yes                                          | None          | String                             | Specify what connector to use, for Kafka use **kafka**.                                                                                                                                                                                   |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | topic                                   | No                                           | None          | String                             | Topic name(s) to read data from when the table is used as source. It also supports topic list for source by separating topic by semicolon like **topic-1;topic-2**.                                                                       |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | Note, only one of **topic-pattern** and **topic** can be specified for sources.                                                                                                                                                           |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | When the table is used as sink, the topic name is the topic to write data to. Note topic list is not supported for sinks.                                                                                                                 |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | topic-pattern                           | No                                           | None          | String                             | The regular expression for a pattern of topic names to read from.                                                                                                                                                                         |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | All topics with names that match the specified regular expression will be subscribed by the consumer when the job starts running.                                                                                                         |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | Note, only one of **topic-pattern** and **topic** can be specified for sources.                                                                                                                                                           |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | For more information, see :ref:`Topic and Partition Discovery <dli_08_15058__section12233124102>`.                                                                                                                                        |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.bootstrap.servers            | Yes                                          | None          | String                             | Comma separated list of Kafka brokers.                                                                                                                                                                                                    |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.group.id                     | optional for source, not applicable for sink | None          | String                             | The ID of the consumer group for Kafka source. If group ID is not specified, an automatically generated ID **KafkaSource-**\ *{tableIdentifier}* will be used.                                                                            |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.\*                           | No                                           | None          | String                             | This can set and pass arbitrary Kafka configurations.                                                                                                                                                                                     |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | -  Suffix names must match the configuration key defined in `Apache Kafka <https://kafka.apache.org/documentation/#configuration>`__.                                                                                                     |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    |    Flink will remove the **properties.** key prefix and pass the transformed key and values to the underlying KafkaClient. For example, you can disable automatic topic creation via **'properties.allow.auto.create.topics' = 'false'**. |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | -  But there are some configurations that do not support to set, because Flink will override them, e.g. **key.deserializer** and **value.deserializer**.                                                                                  |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format                                  | Yes                                          | None          | String                             | The format used to deserialize and serialize the value part of Kafka messages.                                                                                                                                                            |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | Either this parameter or the **value.format** parameter is required.                                                                                                                                                                      |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | -  For details about the message key and body of Kafka messages, see :ref:`Key and Value Formats <dli_08_15058__section9256199230>`.                                                                                                      |
   |                                         |                                              |               |                                    | -  Refer to :ref:`Format <dli_08_15014>` for more details and format parameters.                                                                                                                                                          |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.format                              | No                                           | None          | String                             | The format used to deserialize and serialize the key part of Kafka messages.                                                                                                                                                              |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | -  If a key format is defined, the **key.fields** parameter is required as well. Otherwise the Kafka records will have an empty key.                                                                                                      |
   |                                         |                                              |               |                                    | -  Refer to :ref:`Format <dli_08_15014>` for more details and format parameters.                                                                                                                                                          |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.fields                              | No                                           | []            | List<String>                       | Defines an explicit list of physical columns from the table schema that configure the data type for the key format.                                                                                                                       |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | By default, this list is empty and thus a key is undefined. The list should look like **field1;field2**.                                                                                                                                  |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.fields-prefix                       | No                                           | None          | String                             | Defines a custom prefix for all fields of the key format to avoid name clashes with fields of the value format. By default, the prefix is empty.                                                                                          |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | If a custom prefix is defined, both the table schema and **key.fields** will work with prefixed names.                                                                                                                                    |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | When constructing the data type of the key format, the prefix will be removed and the non-prefixed names will be used within the key format.                                                                                              |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | Note that this parameter requires that **value.fields-include** must be set to **EXCEPT_KEY**.                                                                                                                                            |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.format                            | No                                           | None          | String                             | The format used to deserialize and serialize the value part of Kafka messages.                                                                                                                                                            |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | -  Either this parameter or the **format** parameter is required. If two parameters are configured, a conflict occurs.                                                                                                                    |
   |                                         |                                              |               |                                    | -  Refer to :ref:`Format <dli_08_15014>` for more details and format parameters.                                                                                                                                                          |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.fields-include                    | No                                           | ALL           | Enum                               | Defines a strategy how to deal with key columns in the data type of the value format.                                                                                                                                                     |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               | Possible values: [ALL, EXCEPT_KEY] | By default, **ALL** physical columns of the table schema will be included in the value format which means that key columns appear in the data type for both the key and value format.                                                     |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.startup.mode                       | No                                           | group-offsets | String                             | Startup mode for Kafka consumer.                                                                                                                                                                                                          |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | Valid values are:                                                                                                                                                                                                                         |
   |                                         |                                              |               |                                    |                                                                                                                                                                                                                                           |
   |                                         |                                              |               |                                    | -  **earliest-offset**: start from the earliest offset possible.                                                                                                                                                                          |
   |                                         |                                              |               |                                    | -  **latest-offset**: start from the latest offset.                                                                                                                                                                                       |
   |                                         |                                              |               |                                    | -  **group-offsets**: start from committed offsets in ZooKeeper/Kafka brokers of a specific consumer group.                                                                                                                               |
   |                                         |                                              |               |                                    | -  **timestamp**: start from user-supplied timestamp for each partition.                                                                                                                                                                  |
   |                                         |                                              |               |                                    | -  **specific-offsets**: start from user-supplied specific offsets for each partition, and the position is specified by **scan.startup.specific-offsets**.                                                                                |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.startup.specific-offsets           | No                                           | None          | String                             | Specify offsets for each partition in case of **specific-offsets** startup mode, e.g. **partition:0,offset:42;partition:1,offset:300**.                                                                                                   |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.startup.timestamp-millis           | No                                           | None          | Long                               | Start from the specified epoch timestamp (milliseconds) used in case of **timestamp** startup mode.                                                                                                                                       |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.topic-partition-discovery.interval | No                                           | None          | Duration                           | Interval for consumer to discover dynamically created Kafka topics and partitions periodically.                                                                                                                                           |
   +-----------------------------------------+----------------------------------------------+---------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Result Table Parameters
-----------------------

.. table:: **Table 3** Result table parameters

   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                    | Mandatory   | Default Value | Data Type                          | Description                                                                                                                                                                                                                                 |
   +==============================+=============+===============+====================================+=============================================================================================================================================================================================================================================+
   | connector                    | Yes         | None          | String                             | Specify what connector to use, for Kafka use **kafka**.                                                                                                                                                                                     |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | topic                        | No          | None          | String                             | Topic name(s) to read data from when the table is used as source. It also supports topic list for source by separating topic by semicolon like **topic-1;topic-2**.                                                                         |
   |                              |             |               |                                    |                                                                                                                                                                                                                                             |
   |                              |             |               |                                    | Note, only one of **topic-pattern** and **topic** can be specified for sources.                                                                                                                                                             |
   |                              |             |               |                                    |                                                                                                                                                                                                                                             |
   |                              |             |               |                                    | When the table is used as sink, the topic name is the topic to write data to. Note topic list is not supported for sinks.                                                                                                                   |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.bootstrap.servers | Yes         | None          | String                             | Comma separated list of Kafka brokers.                                                                                                                                                                                                      |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.\*                | No          | None          | String                             | This can set and pass arbitrary Kafka configurations.                                                                                                                                                                                       |
   |                              |             |               |                                    |                                                                                                                                                                                                                                             |
   |                              |             |               |                                    | -  Suffix names must match the configuration key defined in `Apache Kafka <https://kafka.apache.org/documentation/#configuration>`__.                                                                                                       |
   |                              |             |               |                                    |                                                                                                                                                                                                                                             |
   |                              |             |               |                                    |    Flink will remove the **properties.** key prefix and pass the transformed key and values to the underlying KafkaClient. For example, you can disable automatic topic creation via **'properties.allow.auto.create.topics' = 'false'**.   |
   |                              |             |               |                                    |                                                                                                                                                                                                                                             |
   |                              |             |               |                                    | -  But there are some configurations that do not support to set, because Flink will override them, e.g. **key.deserializer** and **value.deserializer**.                                                                                    |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format                       | Yes         | None          | String                             | The format used to deserialize and serialize the value part of Kafka messages. Note, either this parameter or the **value.format** parameter is required.                                                                                   |
   |                              |             |               |                                    |                                                                                                                                                                                                                                             |
   |                              |             |               |                                    | -  For details about the message key and body of Kafka messages, see :ref:`Key and Value Formats <dli_08_15058__section9256199230>`.                                                                                                        |
   |                              |             |               |                                    | -  Refer to :ref:`Format <dli_08_15014>` for more details and format parameters.                                                                                                                                                            |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.format                   | No          | None          | String                             | The format used to deserialize and serialize the key part of Kafka messages.                                                                                                                                                                |
   |                              |             |               |                                    |                                                                                                                                                                                                                                             |
   |                              |             |               |                                    | -  If a key format is defined, the **key.fields** parameter is required as well. Otherwise the Kafka records will have an empty key.                                                                                                        |
   |                              |             |               |                                    | -  Refer to :ref:`Format <dli_08_15014>` for more details and format parameters.                                                                                                                                                            |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.fields                   | No          | []            | List<String>                       | Defines an explicit list of physical columns from the table schema that configure the data type for the key format.                                                                                                                         |
   |                              |             |               |                                    |                                                                                                                                                                                                                                             |
   |                              |             |               |                                    | By default, this list is empty and thus a key is undefined. The list should look like **field1;field2**.                                                                                                                                    |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.fields-prefix            | No          | None          | String                             | Defines a custom prefix for all fields of the key format to avoid name clashes with fields of the value format. By default, the prefix is empty.                                                                                            |
   |                              |             |               |                                    |                                                                                                                                                                                                                                             |
   |                              |             |               |                                    | If a custom prefix is defined, both the table schema and **key.fields** will work with prefixed names.                                                                                                                                      |
   |                              |             |               |                                    |                                                                                                                                                                                                                                             |
   |                              |             |               |                                    | When constructing the data type of the key format, the prefix will be removed and the non-prefixed names will be used within the key format. Note that this parameter requires that **value.fields-include** must be set to **EXCEPT_KEY**. |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.format                 | No          | None          | String                             | The format used to deserialize and serialize the value part of Kafka messages.                                                                                                                                                              |
   |                              |             |               |                                    |                                                                                                                                                                                                                                             |
   |                              |             |               |                                    | -  Either this parameter or the **format** parameter is required. If two parameters are configured, a conflict occurs.                                                                                                                      |
   |                              |             |               |                                    | -  Refer to :ref:`Format <dli_08_15014>` for more details and format parameters.                                                                                                                                                            |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.fields-include         | No          | ALL           | Enum                               | Defines a strategy how to deal with key columns in the data type of the value format.                                                                                                                                                       |
   |                              |             |               |                                    |                                                                                                                                                                                                                                             |
   |                              |             |               | Possible values: [ALL, EXCEPT_KEY] | By default, **ALL** physical columns of the table schema will be included in the value format which means that key columns appear in the data type for both the key and value format.                                                       |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.partitioner             | No          | 'default'     | String                             | Output partitioning from Flink's partitions into Kafka's partitions. Valid values are:                                                                                                                                                      |
   |                              |             |               |                                    |                                                                                                                                                                                                                                             |
   |                              |             |               |                                    | -  **default**: use the kafka default partitioner to partition records.                                                                                                                                                                     |
   |                              |             |               |                                    | -  **fixed**: each Flink partition ends up in at most one Kafka partition.                                                                                                                                                                  |
   |                              |             |               |                                    | -  **round-robin**: a Flink partition is distributed to Kafka partitions sticky round-robin. It only works when record's keys are not specified.                                                                                            |
   |                              |             |               |                                    | -  Custom **FlinkKafkaPartitioner** subclass: e.g. **org.mycompany.MyPartitioner**.                                                                                                                                                         |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.semantic                | No          | at-least-once | String                             | Defines the delivery semantic for the Kafka sink. Valid enumerationns are **at-least-once**, **exactly-once**, and **none**.                                                                                                                |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.parallelism             | No          | None          | Integer                            | Defines the parallelism of the Kafka sink operator. By default, the parallelism is determined by the framework: using the same parallelism as the upstream chained operator.                                                                |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_15058__section9326019161710:

Metadata
--------

You can define metadata in the source table to obtain the metadata of Kafka messages.

For example, if multiple topics are defined in the **WITH** parameter and metadata is defined in the Kafka source table, the data read by Flink is labeled with the topic from which the data is read.

.. table:: **Table 4** Metadata

   +-----------------+--------------------------------------------+-----------------+---------------------------------------------------------------------------+
   | Key             | Data Type                                  | R/W             | Description                                                               |
   +=================+============================================+=================+===========================================================================+
   | topic           | STRING NOT NULL                            | R               | Topic name of the Kafka record.                                           |
   +-----------------+--------------------------------------------+-----------------+---------------------------------------------------------------------------+
   | partition       | INT NOT NULL                               | R               | Partition ID of the Kafka record.                                         |
   +-----------------+--------------------------------------------+-----------------+---------------------------------------------------------------------------+
   | headers         | MAP<STRING, BYTES> NOT NULL                | R/W             | Headers of the Kafka record as a map of raw bytes.                        |
   +-----------------+--------------------------------------------+-----------------+---------------------------------------------------------------------------+
   | leader-epoch    | INT NULL                                   | R               | Leader epoch of the Kafka record if available.                            |
   +-----------------+--------------------------------------------+-----------------+---------------------------------------------------------------------------+
   | offset          | BIGINT NOT NULL                            | R               | Offset of the Kafka record in the partition.                              |
   +-----------------+--------------------------------------------+-----------------+---------------------------------------------------------------------------+
   | timestamp       | TIMESTAMP(3) WITH LOCAL TIME ZONE NOT NULL | R/W             | Timestamp of the Kafka record.                                            |
   +-----------------+--------------------------------------------+-----------------+---------------------------------------------------------------------------+
   | timestamp-type  | STRING NOT NULL                            | R               | Timestamp type of the Kafka record.                                       |
   |                 |                                            |                 |                                                                           |
   |                 |                                            |                 | -  **NoTimestampType**: No timestamp is defined in the message.           |
   |                 |                                            |                 | -  **CreateTime**: time when the message is generated.                    |
   |                 |                                            |                 | -  **LogAppendTime**: time when the message is added to the Kafka broker. |
   +-----------------+--------------------------------------------+-----------------+---------------------------------------------------------------------------+

.. _dli_08_15058__section9256199230:

Key and Value Formats
---------------------

Both the key and value part of a Kafka record can be serialized to and deserialized from raw bytes using one of the given `formats <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/formats/overview/>`__.

-  **Value Format**

   Since a key is optional in Kafka records, the following statement reads and writes records with a configured value format but without a key format. The **format** parameter is a synonym for **value.format**. All format options are prefixed with the format identifier.

   .. code-block::

      CREATE TABLE KafkaTable (
        `ts` TIMESTAMP(3) METADATA FROM 'timestamp',
        `user_id` BIGINT,
        `item_id` BIGINT,
        `behavior` STRING
      ) WITH (
        'connector' = 'kafka',
        ...

        'format' = 'json',
        'json.ignore-parse-errors' = 'true'
      )

   The value format will be configured with the following data type:

   .. code-block::

      ROW<`user_id` BIGINT, `item_id` BIGINT, `behavior` STRING>

-  **Key and Value Format**

   The following example shows how to specify and configure key and value formats. The format options are prefixed with either the **key** or **value** plus format identifier.

   .. code-block::

      CREATE TABLE KafkaTable (
        `ts` TIMESTAMP(3) METADATA FROM 'timestamp',
        `user_id` BIGINT,
        `item_id` BIGINT,
        `behavior` STRING
      ) WITH (
        'connector' = 'kafka',
        ...

        'key.format' = 'json',
        'key.json.ignore-parse-errors' = 'true',
        'key.fields' = 'user_id;item_id',

        'value.format' = 'json',
        'value.json.fail-on-missing-field' = 'false',
        'value.fields-include' = 'ALL'
      )

   The key format includes the fields listed in **key.fields** (using **;** as the delimiter) in the same order. Thus, it will be configured with the following data type:

   .. code-block::

      ROW<`user_id` BIGINT, `item_id` BIGINT>

   Since the value format is configured with **'value.fields-include' = 'ALL'**, key fields will also end up in the value format's data type:

   .. code-block::

      ROW<`user_id` BIGINT, `item_id` BIGINT, `behavior` STRING>

-  **Overlapping Format Fields**

   The connector cannot split the table's columns into key and value fields based on schema information if both key and value formats contain fields of the same name. The **key.fields-prefix** parameter allows to give key columns a unique name in the table schema while keeping the original names when configuring the key format.

   The following example shows a key and value format that both contain a version field:

   .. code-block::

      CREATE TABLE KafkaTable (
        `k_version` INT,
        `k_user_id` BIGINT,
        `k_item_id` BIGINT,
        `version` INT,
        `behavior` STRING
      ) WITH (
        'connector' = 'kafka',
        ...

        'key.format' = 'json',
        'key.fields-prefix' = 'k_',
        'key.fields' = 'k_version;k_user_id;k_item_id',

        'value.format' = 'json',
        'value.fields-include' = 'EXCEPT_KEY'
      )

   The value format must be configured in **EXCEPT_KEY** mode. The formats will be configured with the following data types:

   .. code-block::

      Key format:
      ROW<`version` INT, `user_id` BIGINT, `item_id` BIGINT>

      Value format:
      ROW<`version` INT, `behavior` STRING>

.. _dli_08_15058__section12233124102:

Topic and Partition Discovery
-----------------------------

The config parameters **topic** and **topic-pattern** specify the topics or topic pattern to consume for source. The config parameter **topic** can accept topic list using semicolon separator like **topic-1;topic-2**. The config parameter **topic-pattern** will use regular expression to discover the matched topic. For example, if the **topic-pattern** is **test-topic-[0-9]**, then all topics with names that match the specified regular expression (starting with test-topic- and ending with a single digit)) will be subscribed by the consumer when the job starts running.

To allow the consumer to discover dynamically created topics after the job started running, set a non-negative value for **scan.topic-partition-discovery.interval**. This allows the consumer to discover partitions of new topics with names that also match the specified pattern.

.. note::

   Note that topic list and topic pattern only work in sources. In sinks, Flink currently only supports a single topic.

Example 1: Reading DMS Kafka Metadata in CSV Format and Outputting It to a Kafka Sink (Applicable for Kafka Clusters Without SASL_SSL Enabled)
----------------------------------------------------------------------------------------------------------------------------------------------

#. Create an enhanced datasource connection in the VPC and subnet where Kafka locates, and bind the connection to the required Flink elastic resource pool.

#. Set Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Kafka address. If the connection passes the test, it is bound to the queue.

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

   When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

   .. code-block::

      CREATE TABLE kafkaSource(
        `topic` String metadata virtual,
        `partition` int metadata virtual,
        `headers` MAP<STRING, BYTES> metadata virtual,
        `leader-epoch` INT metadata virtual,
        `offset` bigint metadata virtual,
        `timestamp-type` string metadata virtual,
        `event_time` TIMESTAMP(3) metadata FROM 'timestamp',
        `message` string
      ) WITH (
        'connector' = 'kafka',
        'topic' = 'SourceKafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'csv',
        'csv.field-delimiter' = '\u0001',
        'csv.quote-character' = ''''
      );

      CREATE TABLE kafkaSink (
        `topic` String,
        `partition` int,
        `headers` MAP<STRING, BYTES>,
        `leader-epoch` INT,
        `offset` bigint,
        `timestampType` string,
        `event_time` TIMESTAMP(3),
        `message` string -- Indicates that data written by users is read from Kafka.
      ) WITH (
        'connector' = 'kafka',
        'topic' = 'SinkKafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'format' = 'json'
      );
      insert into kafkaSink select * from kafkaSource;

#. Send the following data to the topic of the source table in Kafka. The Kafka topic is kafkaSource.

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

#. Read the topic of the Kafka result table. The Kafka topic is kafkaSink.

   .. code-block::

      {"topic":"kafkaSource","partition":1,"headers":{},"leader-epoch":0,"offset":4,"timestampType":"LogAppendTime","event_time":"2023-11-16 11:16:30.369","message":"{\"order_id\":\"202103251202020001\", \"order_channel\":\"miniAppShop\", \"order_time\":\"2021-03-25 12:02:02\", \"pay_amount\":\"60.00\", \"real_pay\":\"60.00\", \"pay_time\":\"2021-03-25 12:03:00\", \"user_id\":\"0002\", \"user_name\":\"Bob\", \"area_id\":\"330110\"}"}

      {"topic":"kafkaSource","partition":0,"headers":{},"leader-epoch":0,"offset":6,"timestampType":"LogAppendTime","event_time":"2023-11-16 11:16:30.367","message":"{\"order_id\":\"202103241000000001\",\"order_channel\":\"webShop\",\"order_time\":\"2021-03-24 10:00:00\",\"pay_amount\":100.0,\"real_pay\":100.0,\"pay_time\":\"2021-03-24 10:02:03\",\"user_id\":\"0001\",\"user_name\":\"Alice\",\"area_id\":\"330106\"}"}

      {"topic":"kafkaSource","partition":2,"headers":{},"leader-epoch":0,"offset":5,"timestampType":"LogAppendTime","event_time":"2023-11-16 11:16:30.368","message":"{\"order_id\":\"202103241606060001\",\"order_channel\":\"appShop\",\"order_time\":\"2021-03-24 16:06:06\",\"pay_amount\":200.0,\"real_pay\":180.0,\"pay_time\":\"2021-03-24 16:10:06\",\"user_id\":\"0001\",\"user_name\":\"Alice\",\"area_id\":\"330106\"}"}

Example 2: Using DMS Kafka in JSON Format as the Source Table and Outputting It to a Kafka Sink (Applicable for Kafka Clusters Without SASL_SSL Enabled)
--------------------------------------------------------------------------------------------------------------------------------------------------------

**Use the Kafka source table and Kafka result table to read JSON data from Kafka and output it to the log file.**

#. Create an enhanced datasource connection in the VPC and subnet where Kafka locates, and bind the connection to the required Flink elastic resource pool.

#. Set Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Kafka address. If the connection passes the test, it is bound to the queue.

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

   When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

   .. code-block::

      CREATE TABLE kafkaSource(
        order_id string,
        order_channel string,
        order_time timestamp(3),
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string
      ) WITH (
        'connector' = 'kafka',
        'topic' = 'KafkaSourceTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );

      CREATE TABLE kafkaSink (
        order_id string,
        order_channel string,
        order_time timestamp(3),
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string
      ) WITH (
        'connector' = 'kafka',
        'topic' = 'KafkaSinkTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'format' = 'json'
      );
      insert into kafkaSink select * from kafkaSource;

#. Send the following data to the topic of the source table in Kafka:

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

#. Read the topic of the Kafka result table. The data results are as follows:

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

Example 3: Using DMS Kafka as the Source Table and Print as the Result Table (Applicable for Kafka Clusters with SASL_SSL Enabled)
----------------------------------------------------------------------------------------------------------------------------------

Create a Kafka cluster for DMS, enable SASL_SSL, download the SSL certificate, and upload the downloaded certificate **client.jks** to an OBS bucket.

The **properties.sasl.jaas.config** field contains account passwords encrypted using DEW.

.. code-block::

   CREATE TABLE ordersSource (
     order_id string,
     order_channel string,
     order_time timestamp(3),
     pay_amount double,
     real_pay double,
     pay_time string,
     user_id string,
     user_name string,
     area_id string
   ) WITH (
     'connector' = 'kafka',
     'topic' = 'KafkaTopic',
     'properties.bootstrap.servers' = 'KafkaAddress1:9093,KafkaAddress2:9093',
     'properties.group.id' = 'GroupId',
     'scan.startup.mode' = 'latest-offset',
     'properties.connector.auth.open' = 'true',
     'properties.ssl.truststore.location' = 'obs://xx/client.jks',  -- Location where the user uploads the certificate to
     'properties.sasl.mechanism' = 'PLAIN',
     'properties.security.protocol' = 'SASL_SSL',
     'properties.sasl.jaas.config' = 'xx',  -- Key in DEW secret management, whose value is like org.apache.kafka.common.security.plain.PlainLoginModule required username=xx password=xx;
     'format' = 'json',
     'dew.endpoint' = 'kms.xx.com', --Endpoint information for the DEW service being used
     'dew.csms.secretName' = 'xx', --Name of the DEW shared secret
     'dew.csms.decrypt.fields' = 'properties.sasl.jaas.config', --The properties.sasl.jaas.config field value must be decrypted and replaced using DEW secret management.
     'dew.csms.version' = 'v1'
   );

   CREATE TABLE ordersSink (
     order_id string,
     order_channel string,
     order_time timestamp(3),
     pay_amount double,
     real_pay double,
     pay_time string,
     user_id string,
     user_name string,
     area_id string
   ) WITH (
     'connector' = 'print'
   );
    insert into ordersSink select * from ordersSource;

Example 4: Using Kafka (MRS Cluster) as the Source Table and Print as the Result Table (Applicable for Kafka with SASL_SSL Enabled and MRS Using Kerberos Authentication)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-  Enable Kerberos authentication for the MRS cluster.

-  Click the **Components** tab and click **Kafka**. On the displayed page, click the **Service Configuration** tab, locate the **ssl.mode.enable**, set it to **true**, and restart Kafka.

-  Download the user credential. Log in to the MRS Manager of the MRS cluster and choose **System** > **Permission** > **User**. Locate the row that contains the target user, click **More**, and select **Download Authentication Credential**.

   Obtain the **truststore.jks** file using the authentication credential and store the credential and **truststore.jks** file in OBS.

-  If "Message stream modified (41)" is displayed, the JDK version may be incorrect. Change the JDK version in the sample code to a version earlier than 8u_242 or delete the **renew_lifetime = 0m** configuration item from the **krb5.conf** configuration file.

-  Set the port to the **sasl_ssl.port** configured in the Kafka service configuration. The default value is **21009**.

-  In the following statements, set **security.protocol** to **SASL_SSL**.

-  The **properties.ssl.truststore.password** field in the **with** parameter is encrypted using DEW.

.. code-block::

   CREATE TABLE ordersSource (
     order_id string,
     order_channel string,
     order_time timestamp(3),
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
     'properties.sasl.kerberos.service.name' = 'kafka', -- Value configured in the MRS cluster
     'properties.connector.auth.open' = 'true',
     'properties.connector.kerberos.principal' = 'xx', --Username
     'properties.connector.kerberos.krb5' = 'obs://xx/krb5.conf',
     'properties.connector.kerberos.keytab' = 'obs://xx/user.keytab',
     'properties.security.protocol' = 'SASL_SSL',
     'properties.ssl.truststore.location' = 'obs://xx/truststore.jks',
     'properties.ssl.truststore.password' = 'xx',  -- Key in the DEW secret
     'properties.sasl.mechanism' = 'GSSAPI',
     'format' = 'json',
     'dew.endpoint'='kms.xx.xx.com', --Endpoint information for the DEW service being used
     'dew.csms.secretName'='xx', --Name of the DEW shared secret
     'dew.csms.decrypt.fields'='properties.ssl.truststore.password', --The properties.ssl.truststore.password field value must be decrypted and replaced using DEW secret management.
     'dew.csms.version'='v1'
   );

   CREATE TABLE ordersSink (
     order_id string,
     order_channel string,
     order_time timestamp(3),
     pay_amount double,
     real_pay double,
     pay_time string,
     user_id string,
     user_name string,
     area_id string
   ) WITH (
     'connector' = 'print'
   );
    insert into ordersSink select * from ordersSource;

Example 5: Using Kafka (MRS Cluster) as the Source Table and Print as the Result Table (Applicable for Kafka with SASL_SSL Enabled and MRS Using SASL_PLAINTEXT with Kerberos Authentication)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-  Enable Kerberos authentication for the MRS cluster.
-  Click the **Components** tab and click **Kafka**. On the displayed page, click the **Service Configuration** tab, locate the **ssl.mode.enable**, set it to **true**, and restart Kafka.
-  Log in to the MRS Manager of the MRS cluster and download the user credential. Choose **System** > **Permission** > **User**. Locate the row that contains the target user, choose **More** > **Download Authentication Credential**. Upload the credential to OBS.
-  If error message "Message stream modified (41)" is displayed, the JDK version may be incorrect. Change the JDK version in the sample code to a version earlier than 8u_242 or delete the **renew_lifetime = 0m** configuration item from the **krb5.conf** configuration file.
-  Set the port to the **sasl.port** configured in the Kafka service configuration. The default value is **21007**.
-  In the following statements, set **security.protocol** to **SASL_PLAINTEXT**.

.. code-block::

   CREATE TABLE ordersSource (
     order_id string,
     order_channel string,
     order_time timestamp(3),
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
     'properties.sasl.kerberos.service.name' = 'kafka', -- Configured in the MRS cluster
     'properties.connector.auth.open' = 'true',
     'properties.connector.kerberos.principal' = 'xx',
     'properties.connector.kerberos.krb5' = 'obs://xx/krb5.conf',
     'properties.connector.kerberos.keytab' = 'obs://xx/user.keytab',
     'properties.security.protocol' = 'SASL_PLAINTEXT',
     'properties.sasl.mechanism' = 'GSSAPI',
     'format' = 'json'
   );

   CREATE TABLE ordersSink (
     order_id string,
     order_channel string,
     order_time timestamp(3),
     pay_amount double,
     real_pay double,
     pay_time string,
     user_id string,
     user_name string,
     area_id string
   ) WITH (
     'connector' = 'print'
   );
    insert into ordersSink select * from ordersSource;

Example 6: Using Kafka (MRS Cluster) as the Source Table and Print as the Result Table (Applicable for Kafka with SSL Enabled and MRS Without Kerberos Authentication Enabled)
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-  Do not enable Kerberos authentication for the MRS cluster.

-  Download the user credential. Log in to the MRS Manager of the MRS cluster and choose **System** > **Permission** > **User**. Locate the row that contains the target user, click **More**, and select **Download Authentication Credential**.

   Obtain the **truststore.jks** file using the authentication credential and store the credential and **truststore.jks** file in OBS.

-  Set the port to the **ssl.port** configured in the Kafka service configuration. The default value is **9093**.

-  Set **security.protocol** in the **with** parameter to **SSL**.

-  In the Kafka configuration of the MRS cluster, set **ssl.mode.enable** to **true** and restart Kafka.

-  The **properties.ssl.truststore.password** field in the **with** parameter is encrypted using DEW.

   .. code-block::

      CREATE TABLE ordersSource (
        order_id string,
        order_channel string,
        order_time timestamp(3),
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
        'properties.connector.auth.open' = 'true',
        'properties.ssl.truststore.location' = 'obs://xx/truststore.jks',
        'properties.ssl.truststore.password' = 'xx',  -- Key for DEW secret management, whose value is the password set when generating truststore.jks
        'properties.security.protocol' = 'SSL',
        'format' = 'json',
        'dew.endpoint' = 'kms.xx.com', --Endpoint information for the DEW service being used
        'dew.csms.secretName' = 'xx', --Name of the DEW shared secret
        'dew.csms.decrypt.fields' = 'properties.ssl.truststore.password', --The properties.ssl.truststore.password field value must be decrypted and replaced using DEW secret management.
        'dew.csms.version' = 'v1'
      );

      CREATE TABLE ordersSink (
        order_id string,
        order_channel string,
        order_time timestamp(3),
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string
      ) WITH (
        'connector' = 'print'
      );
       insert into ordersSink select * from ordersSource;

FAQ
---

-  **Q: What should I do if the Flink job execution fails and the log contains the following error information?**

   .. code-block::

      org.apache.kafka.common.errors.TimeoutException: Timeout expired while fetching topic metadata

   A: The datasource connection is not bound, the binding fails, or the security group of the Kafka cluster is not configured to allow access from the network segment of the DLI queue. Reconfigure the datasource connection or configure the security group of the Kafka cluster to allow access from the DLI queue.

-  **Q: What should I do if the Flink job execution fails and the log contains the following error information?**

   .. code-block::

      Caused by: java.lang.RuntimeException: RealLine:45;Table 'default_catalog.default_database.printSink' declares persistable metadata columns, but the underlying DynamicTableSink doesn't implement the SupportsWritingMetadata interface. If the column should not be persisted, it can be declared with the VIRTUAL keyword.

   A: The metadata type is defined in the sink table, but the Print connector does not support deletion of matadata from the sink table.
