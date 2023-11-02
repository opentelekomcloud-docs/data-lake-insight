:original_name: dli_08_0257.html

.. _dli_08_0257:

Open-Source Kafka Sink Stream
=============================

Function
--------

DLI exports the output data of the Flink job to Kafka.

Apache Kafka is a fast, scalable, and fault-tolerant distributed message publishing and subscription system. It delivers high throughput and built-in partitions and provides data replicas and fault tolerance. Apache Kafka is applicable to scenarios of handling massive messages.

Prerequisites
-------------

-  If the Kafka server listens on the port using hostname, you need to add the mapping between the hostname and IP address of the Kafka Broker node to the DLI queue. Contact the Kafka service deployment personnel to obtain the hostname and IP address of the Kafka Broker node. For details about how to add an IP-domain mapping, see **Enhanced Datasource Connections** > **Modifying the Host Information** in the *Data Lake Insight User Guide*.

-  Kafka is an offline cluster. You need to use the enhanced datasource connection function to connect Flink jobs to Kafka. You can also set security group rules as required.

   For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

   For details about how to configure security group rules, see **Security Group** in the *Virtual Private Cloud User Guide*.

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH(
       type = "kafka",
       kafka_bootstrap_servers = "",
       kafka_topic = "",
       encode = "json"
     )

Keyword
-------

.. table:: **Table 1** Keyword description

   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory             | Description                                                                                                                                                                                                                |
   +=========================+=======================+============================================================================================================================================================================================================================+
   | type                    | Yes                   | Output channel type. **kafka** indicates that data is exported to Kafka.                                                                                                                                                   |
   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_bootstrap_servers | Yes                   | Port that connects DLI to Kafka. Use enhanced datasource connections to connect DLI queues with Kafka clusters.                                                                                                            |
   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_topic             | Yes                   | Kafka topic into which DLI writes data.                                                                                                                                                                                    |
   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode                  | Yes                   | Data encoding format. The value can be **csv**, **json**, or **user_defined**.                                                                                                                                             |
   |                         |                       |                                                                                                                                                                                                                            |
   |                         |                       | -  **field_delimiter** must be specified if this parameter is set to **csv**.                                                                                                                                              |
   |                         |                       | -  **encode_class_name** and **encode_class_parameter** must be specified if this parameter is set to **user_defined**.                                                                                                    |
   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | filed_delimiter         | No                    | If **encode** is set to **csv**, you can use this parameter to specify the separator between CSV fields. By default, the comma (,) is used.                                                                                |
   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode_class_name       | No                    | If **encode** is set to **user_defined**, you need to set this parameter to the name of the user-defined decoding class (including the complete package path). The class must inherit the **DeserializationSchema** class. |
   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode_class_parameter  | No                    | If **encode** is set to **user_defined**, you can set this parameter to specify the input parameter of the user-defined decoding class. Only one parameter of the string type is supported.                                |
   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_properties        | No                    | This parameter is used to configure the native attributes of Kafka. The format is **key1=value1;key2=value2**.                                                                                                             |
   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_certificate_name  | No                    | Name of the datasource authentication information. This parameter is valid only when the datasource authentication type is set to **Kafka_SSL**.                                                                           |
   |                         |                       |                                                                                                                                                                                                                            |
   |                         |                       | .. note::                                                                                                                                                                                                                  |
   |                         |                       |                                                                                                                                                                                                                            |
   |                         |                       |    -  If this parameter is specified, the service loads only the specified file and password under the authentication. The system automatically sets this parameter to **kafka_properties**.                               |
   |                         |                       |    -  Other configuration information required for Kafka SSL authentication needs to be manually configured in the **kafka_properties** attribute.                                                                         |
   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

None

Example
-------

Output the data in the kafka_sink stream to Kafka.

::

   CREATE SINK STREAM kafka_sink (name STRING)
     WITH (
       type="kafka",
       kafka_bootstrap_servers =  "ip1:port1,ip2:port2",
       kafka_topic = "testsink",
       encode = "json"
     );
