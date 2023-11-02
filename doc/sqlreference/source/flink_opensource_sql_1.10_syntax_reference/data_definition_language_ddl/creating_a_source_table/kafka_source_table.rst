:original_name: dli_08_0301.html

.. _dli_08_0301:

Kafka Source Table
==================

Function
--------

Create a source stream to obtain data from Kafka as input data for jobs.

Apache Kafka is a fast, scalable, and fault-tolerant distributed message publishing and subscription system. It delivers high throughput and built-in partitions and provides data replicas and fault tolerance. Apache Kafka is applicable to scenarios of handling massive messages.

Prerequisites
-------------

Kafka is an offline cluster. You have built an enhanced datasource connection to connect Flink jobs to Kafka. You have set security group rules as required.

Precautions
-----------

SASL_SSL cannot be enabled for the interconnected Kafka cluster.

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
     'connector.type' = 'kafka',
     'connector.version' = '',
     'connector.topic' = '',
     'connector.properties.bootstrap.servers' = '',
     'connector.properties.group.id' = '',
     'connector.startup-mode' = '',
     'format.type' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                              | Mandatory             | Description                                                                                                                                                                       |
   +========================================+=======================+===================================================================================================================================================================================+
   | connector.type                         | Yes                   | Connector type. Set this parameter to **kafka**.                                                                                                                                  |
   +----------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.version                      | Yes                   | Kafka version. The value can be '**0.10'** or **'0.11'**, which corresponds to Kafka 2.11 to 2.4.0 and other historical versions, respectively.                                   |
   +----------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format.type                            | Yes                   | Data deserialization format. The value can be **csv**, **json**, or **avro**.                                                                                                     |
   +----------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format.field-delimiter                 | No                    | Attribute delimiter. You can customize the attribute delimiter only when the encoding format is CSV. The default delimiter is a comma (,).                                        |
   +----------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.topic                        | Yes                   | Kafka topic name. Either this parameter or **connector.topic-pattern** is used.                                                                                                   |
   +----------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.topic-pattern                | No                    | Regular expression for matching the Kafka topic name. Either this parameter or **connector.topic** is used.                                                                       |
   |                                        |                       |                                                                                                                                                                                   |
   |                                        |                       | Example:                                                                                                                                                                          |
   |                                        |                       |                                                                                                                                                                                   |
   |                                        |                       | 'topic.*'                                                                                                                                                                         |
   |                                        |                       |                                                                                                                                                                                   |
   |                                        |                       | '(topic-c|topic-d)'                                                                                                                                                               |
   |                                        |                       |                                                                                                                                                                                   |
   |                                        |                       | '(topic-a|topic-b|topic-\\\\d*)'                                                                                                                                                  |
   |                                        |                       |                                                                                                                                                                                   |
   |                                        |                       | '(topic-a|topic-b|topic-[0-9]*)'                                                                                                                                                  |
   +----------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.properties.bootstrap.servers | Yes                   | Kafka broker addresses. Use commas (,) to separated them.                                                                                                                         |
   +----------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.properties.group.id          | No                    | Consumer group name                                                                                                                                                               |
   +----------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.startup-mode                 | No                    | Consumer startup mode. The value can be **earliest-offset**, **latest-offset**, **group-offsets**, **specific-offsets** or **timestamp**. The default value is **group-offsets**. |
   +----------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.specific-offsets             | No                    | Consumption offset. This parameter is mandatory when **startup-mode** is **specific-offsets**. The value is in the 'partition:0,offset:42;partition:1,offset:300' format.         |
   +----------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.startup-timestamp-millis     | No                    | Consumption start timestamp. This parameter is mandatory when **startup-mode** is **timestamp**.                                                                                  |
   +----------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.properties.\*                | No                    | Native Kafka property                                                                                                                                                             |
   +----------------------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

-  Create table **kafkaSource** and read data encoded in CSV format from Kafka.

   ::

      create table kafkaSource(
        car_id STRING,
        car_owner STRING,
        car_brand STRING,
        car_speed INT)
      with (
        'connector.type' = 'kafka',
        'connector.version' = '0.11',
        'connector.topic' = 'test-topic',
        'connector.properties.bootstrap.servers' = 'xx.xx.xx.xx:9092',
        'connector.properties.group.id' = 'test-group',
        'connector.startup-mode' = 'latest-offset',
        'format.type' = 'csv'
      );

-  Create table **kafkaSource** and read data in non-nested JSON strings from Kafka.

   Assume that the non-nested JSON strings are as follows:

   .. code-block::

      {"car_id": 312, "car_owner": "wang", "car_brand": "tang"}
      {"car_id": 313, "car_owner": "li", "car_brand": "lin"}
      {"car_id": 314, "car_owner": "zhao", "car_brand": "han"}

   You can create the table as follows:

   ::

      create table kafkaSource(
        car_id STRING,
        car_owner STRING,
        car_brand STRING
      )
      with (
        'connector.type' = 'kafka',
        'connector.version' = '0.11',
        'connector.topic' = 'test-topic',
        'connector.properties.bootstrap.servers' = 'xx.xx.xx.xx:9092',
        'connector.properties.group.id' = 'test-group',
        'connector.startup-mode' = 'latest-offset',
        'format.type' = 'json'
      );

-  Create table **kafkaSource** and read the nested JSON data from Kafka.

   Assume that the JSON data is as follows:

   .. code-block::

      {
          "id":"1",
          "type":"online",
          "data":{
              "patient_id":1234,
              "name":"bob1234",
              "age":"Bob",
              "gmt_create":"Bob",
              "gmt_modify":"Bob"
          }
      }

   You can create the table as follows:

   .. code-block::

      CREATE table kafkaSource(
        id STRING,
        type STRING,
        data ROW(
          patient_id STRING,
          name STRING,
          age STRING,
          gmt_create STRING,
          gmt_modify STRING)
      )
      with (
        'connector.type' = 'kafka',
        'connector.version' = '0.11',
        'connector.topic' = 'test-topic',
        'connector.properties.bootstrap.servers' = 'xx.xx.xx.xx:9092',
        'connector.properties.group.id' = 'test-group',
        'connector.startup-mode' = 'latest-offset',
        'format.type' = 'json'
      );
