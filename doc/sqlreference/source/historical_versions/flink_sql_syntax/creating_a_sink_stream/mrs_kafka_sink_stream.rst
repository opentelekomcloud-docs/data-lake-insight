:original_name: dli_08_0254.html

.. _dli_08_0254:

MRS Kafka Sink Stream
=====================

Function
--------

DLI exports the output data of the Flink job to Kafka.

Apache Kafka is a fast, scalable, and fault-tolerant distributed message publishing and subscription system. It delivers high throughput and built-in partitions and provides data replicas and fault tolerance. Apache Kafka is applicable to scenarios of handling massive messages. Kafka clusters are deployed and hosted on MRS that is powered on Apache Kafka.

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

Keywords
--------

.. table:: **Table 1** Keywords

   +-------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory             | Description                                                                                                                                                                                                                                                                                                                                       |
   +=========================+=======================+===================================================================================================================================================================================================================================================================================================================================================+
   | type                    | Yes                   | Output channel type. **kafka** indicates that data is exported to Kafka.                                                                                                                                                                                                                                                                          |
   +-------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_bootstrap_servers | Yes                   | Port that connects DLI to Kafka. Use enhanced datasource connections to connect DLI queues with Kafka clusters.                                                                                                                                                                                                                                   |
   +-------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_topic             | Yes                   | Kafka topic into which DLI writes data.                                                                                                                                                                                                                                                                                                           |
   +-------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode                  | Yes                   | Encoding format. Currently, **json** and **user_defined** are supported.                                                                                                                                                                                                                                                                          |
   |                         |                       |                                                                                                                                                                                                                                                                                                                                                   |
   |                         |                       | **encode_class_name** and **encode_class_parameter** must be specified if this parameter is set to **user_defined**.                                                                                                                                                                                                                              |
   +-------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode_class_name       | No                    | If **encode** is set to **user_defined**, you need to set this parameter to the name of the user-defined decoding class (including the complete package path). The class must inherit the **DeserializationSchema** class.                                                                                                                        |
   +-------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode_class_parameter  | No                    | If **encode** is set to **user_defined**, you can set this parameter to specify the input parameter of the user-defined decoding class. Only one parameter of the string type is supported.                                                                                                                                                       |
   +-------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | krb_auth                | No                    | Authentication name for creating a datasource connection authentication. This parameter is mandatory when Kerberos authentication is enabled. If Kerberos authentication is not enabled for the created MRS cluster, ensure that the **/etc/hosts** information of the master node in the MRS cluster is added to the host file of the DLI queue. |
   +-------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_properties        | No                    | This parameter is used to configure the native attributes of Kafka. The format is **key1=value1;key2=value2**.                                                                                                                                                                                                                                    |
   +-------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_certificate_name  | No                    | Specifies the name of the datasource authentication information. This parameter is valid only when the datasource authentication type is set to **Kafka_SSL**.                                                                                                                                                                                    |
   |                         |                       |                                                                                                                                                                                                                                                                                                                                                   |
   |                         |                       | .. note::                                                                                                                                                                                                                                                                                                                                         |
   |                         |                       |                                                                                                                                                                                                                                                                                                                                                   |
   |                         |                       |    -  If this parameter is specified, the service loads only the specified file and password under the authentication. The system automatically sets this parameter to **kafka_properties**.                                                                                                                                                      |
   |                         |                       |    -  Other configuration information required for Kafka SSL authentication needs to be manually configured in the **kafka_properties** attribute.                                                                                                                                                                                                |
   +-------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

None

Example
-------

Output data to Kafka.

-  Example 1:

   ::

      CREATE SINK STREAM kafka_sink (name STRING)
        WITH (
          type="kafka",
          kafka_bootstrap_servers =  "ip1:port1,ip2:port2",
          kafka_topic = "testsink",
          encode = "json"
        );

-  Example 2:

   ::

      CREATE SINK STREAM kafka_sink (
        a1 string,
        a2 string,
        a3 string,
        a4 INT
        ) // Output Field
        WITH (
          type="kafka",
          kafka_bootstrap_servers =  "192.x.x.x:9093, 192.x.x.x:9093, 192.x.x.x:9093",
      kafka_topic = "testflink", // Written topic
        encode = "csv", // Encoding format, which can be JSON or CSV.
          kafka_certificate_name = "Flink",
          kafka_properties_delimiter = ",",
          kafka_properties = "sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username=\"xxx\" password=\"xxx\";,sasl.mechanism=PLAIN,security.protocol=SASL_SSL"
        );
