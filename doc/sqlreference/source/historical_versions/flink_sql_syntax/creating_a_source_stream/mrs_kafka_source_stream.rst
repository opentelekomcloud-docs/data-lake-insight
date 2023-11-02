:original_name: dli_08_0238.html

.. _dli_08_0238:

MRS Kafka Source Stream
=======================

Function
--------

Create a source stream to obtain data from Kafka as input data for jobs.

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

   CREATE SOURCE STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "kafka",
       kafka_bootstrap_servers = "",
       kafka_group_id = "",
       kafka_topic = "",
       encode = "json"
     );

Keyword
-------

.. table:: **Table 1** Keyword description

   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory             | Description                                                                                                                                                                                                                          |
   +=========================+=======================+======================================================================================================================================================================================================================================+
   | type                    | Yes                   | Data source type. **Kafka** indicates that the data source is Kafka.                                                                                                                                                                 |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_bootstrap_servers | Yes                   | Port that connects DLI to Kafka. Use enhanced datasource connections to connect DLI queues with Kafka clusters.                                                                                                                      |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_group_id          | No                    | Group ID                                                                                                                                                                                                                             |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_topic             | Yes                   | Kafka topic to be read. Currently, only one topic can be read at a time.                                                                                                                                                             |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode                  | Yes                   | Data encoding format. The value can be **csv**, **json**, **blob**, or **user_defined**.                                                                                                                                             |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | -  **field_delimiter** must be specified if this parameter is set to **csv**.                                                                                                                                                        |
   |                         |                       | -  **json_config** must be specified if this parameter is set to **json**.                                                                                                                                                           |
   |                         |                       | -  If this parameter is set to **blob**, the received data is not parsed, only one stream attribute exists, and the stream attribute is of the Array[TINYINT] type.                                                                  |
   |                         |                       | -  **encode_class_name** and **encode_class_parameter** must be specified if this parameter is set to **user_defined**.                                                                                                              |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode_class_name       | No                    | If **encode** is set to **user_defined**, you need to set this parameter to the name of the user-defined decoding class (including the complete package path). The class must inherit the **DeserializationSchema** class.           |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode_class_parameter  | No                    | If **encode** is set to **user_defined**, you can set this parameter to specify the input parameter of the user-defined decoding class. Only one parameter of the string type is supported.                                          |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | krb_auth                | No                    | The authentication name for creating a datasource connection authentication. This parameter is mandatory when Kerberos authentication is enabled.                                                                                    |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | .. note::                                                                                                                                                                                                                            |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       |    Ensure that the **/etc/hosts** information of the master node in the MRS cluster is added to the host file of the DLI queue.                                                                                                      |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json_config             | No                    | If **encode** is set to **json**, you can use this parameter to specify the mapping between JSON fields and stream attributes.                                                                                                       |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | The format is field1=json_field1;field2=json_field2.                                                                                                                                                                                 |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | **field1** and **field2** indicate the names of the created table fields. **json_field1** and **json_field2** are key fields of the JSON strings in the Kafka input data.                                                            |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | For details, see the :ref:`example <dli_08_0238__section1884612595314>`.                                                                                                                                                             |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | field_delimiter         | No                    | If **encode** is set to **csv**, you can use this parameter to specify the separator between CSV fields. By default, the comma (,) is used.                                                                                          |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | quote                   | No                    | Quoted symbol in a data format. The attribute delimiters between two quoted symbols are treated as common characters.                                                                                                                |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | -  If double quotation marks are used as the quoted symbol, set this parameter to **\\u005c\\u0022** for character conversion.                                                                                                       |
   |                         |                       | -  If a single quotation mark is used as the quoted symbol, set this parameter to a single quotation mark (').                                                                                                                       |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | .. note::                                                                                                                                                                                                                            |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       |    -  Currently, only the CSV format is supported.                                                                                                                                                                                   |
   |                         |                       |    -  After this parameter is specified, ensure that each field does not contain quoted symbols or contains an even number of quoted symbols. Otherwise, parsing will fail.                                                          |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | start_time              | No                    | Start time when Kafka data is ingested.                                                                                                                                                                                              |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | If this parameter is specified, DLI reads data read from the specified time. The format is **yyyy-MM-dd HH:mm:ss**. Ensure that the value of **start_time** is not later than the current time. Otherwise, no data will be obtained. |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_properties        | No                    | This parameter is used to configure the native attributes of Kafka. The format is **key1=value1;key2=value2**.                                                                                                                       |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_certificate_name  | No                    | Specifies the name of the datasource authentication information. This parameter is valid only when the datasource authentication type is set to **Kafka_SSL**.                                                                       |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | .. note::                                                                                                                                                                                                                            |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       |    -  If this parameter is specified, the service loads only the specified file and password under the authentication. The system automatically sets this parameter to **kafka_properties**.                                         |
   |                         |                       |    -  Other configuration information required for Kafka SSL authentication needs to be manually configured in the **kafka_properties** attribute.                                                                                   |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

When creating a source stream, you can specify a time model for subsequent calculation. Currently, DLI supports two time models: Processing Time and Event Time. For details about the syntax, see :ref:`Configuring Time Models <dli_08_0107>`.

.. _dli_08_0238__section1884612595314:

Example
-------

-  Read data from the Kafka topic **test**.

   ::

      CREATE SOURCE STREAM kafka_source (
        name STRING,
        age int
       )
        WITH (
          type = "kafka",
          kafka_bootstrap_servers = "ip1:port1,ip2:port2",
          kafka_group_id = "sourcegroup1",
          kafka_topic = "test",
          encode = "json"
      );

-  Read the topic whose object is **test** from Kafka and use **json_config** to map JSON data to table fields.

   The data encoding format is non-nested JSON.

   .. code-block::

      {"attr1": "lilei", "attr2": 18}

   The table creation statement is as follows:

   ::

      CREATE SOURCE STREAM kafka_source (name STRING, age int)
      WITH (
        type = "kafka",
        kafka_bootstrap_servers = "ip1:port1,ip2:port2",
        kafka_group_id = "sourcegroup1",
        kafka_topic = "test",
        encode = "json",
        json_config = "name=attr1;age=attr2"
      );
