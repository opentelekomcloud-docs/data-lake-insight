:original_name: dli_08_0309.html

.. _dli_08_0309:

Upsert Kafka Result Table
=========================

Function
--------

DLI exports the output data of the Flink job to Kafka in upsert mode.

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
   )
   with (
     'connector.type' = 'upsert-kafka',
     'connector.version' = '',
     'connector.topic' = '',
     'connector.properties.bootstrap.servers' = '',
      'format.type' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                              | Mandatory | Description                                                                                                                                       |
   +========================================+===========+===================================================================================================================================================+
   | connector.type                         | Yes       | Connector type. Set this parameter to **upsert-kafka**.                                                                                           |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.version                      | No        | Kafka version. The value can only be **0.11**.                                                                                                    |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | format.type                            | Yes       | Data serialization format. The value can be **csv**, **json**, or **avro**.                                                                       |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.topic                        | Yes       | Kafka topic name                                                                                                                                  |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.properties.bootstrap.servers | Yes       | Kafka broker addresses. Use commas (,) to separated them.                                                                                         |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.sink-partitioner             | No        | Partitioner type. The value can be **fixed**, **round-robin**, or **custom**.                                                                     |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.sink-partitioner-class       | No        | Custom partitioner. This parameter is mandatory when **sink-partitioner** is **custom**, for example, **org.mycompany.MyPartitioner**.            |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.sink.ignore-retraction       | No        | Whether to ignore the retraction message. The default value is **false**, indicating that the retraction message is written to Kafka as **null**. |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | update-mode                            | No        | Data update mode. Three write modes are supported: **append**, **retract**, and **upsert**.                                                       |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.properties.\*                | No        | Native properties of Kafka                                                                                                                        |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

.. code-block::

   create table upsertKafkaSink(
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_speed INT,
     primary key (car_id) not enforced
   )
   with (
     'connector.type' = 'upsert-kafka',
     'connector.version' = '0.11',
     'connector.topic' = 'test-topic',
     'connector.properties.bootstrap.servers' = 'xx.xx.xx.xx:9092',
     'format.type' = 'csv'
   );
