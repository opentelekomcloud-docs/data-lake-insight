:original_name: dli_08_0308.html

.. _dli_08_0308:

Kafka Result Table
==================

Function
--------

DLI exports the output data of the Flink job to Kafka.

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
     'connector.type' = 'kafka',
     'connector.version' = '',
     'connector.topic' = '',
     'connector.properties.bootstrap.servers' = '',
     'format.type' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                              | Mandatory | Description                                                                                                                                     |
   +========================================+===========+=================================================================================================================================================+
   | connector.type                         | Yes       | Connector type. Set this parameter to **kafka**.                                                                                                |
   +----------------------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.version                      | No        | Kafka version. The value can be '**0.10'** or **'0.11'**, which corresponds to Kafka 2.11 to 2.4.0 and other historical versions, respectively. |
   +----------------------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | format.type                            | Yes       | Data serialization format. The value can be **csv**, **json**, or **avro**.                                                                     |
   +----------------------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | format.field-delimiter                 | No        | Attribute delimiter. You can customize the attribute delimiter only when the encoding format is CSV. The default delimiter is a comma (,).      |
   +----------------------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.topic                        | Yes       | Kafka topic name.                                                                                                                               |
   +----------------------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.properties.bootstrap.servers | Yes       | Kafka broker addresses. Use commas (,) to separated them.                                                                                       |
   +----------------------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.sink-partitioner             | No        | Partitioner type. The value can be **fixed**, **round-robin**, or **custom**.                                                                   |
   +----------------------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.sink-partitioner-class       | No        | Custom partitioner. This parameter is mandatory when **sink-partitioner** is **custom**, for example, **org.mycompany.MyPartitioner**.          |
   +----------------------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | update-mode                            | No        | Data update mode. Three write modes are supported: **append**, **retract**, and **upsert**.                                                     |
   +----------------------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.properties.\*                | No        | Native properties of Kafka                                                                                                                      |
   +----------------------------------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Output the data in **kafkaSink** to Kafka.

::

   create table kafkaSink(
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_speed INT)
   with (
     'connector.type' = 'kafka',
     'connector.version' = '0.10',
     'connector.topic' = 'test-topic',
     'connector.properties.bootstrap.servers' = 'xx.xx.xx.xx:9092',
     'connector.sink-partitioner' = 'round-robin',
     'format.type' = 'csv'
   );
