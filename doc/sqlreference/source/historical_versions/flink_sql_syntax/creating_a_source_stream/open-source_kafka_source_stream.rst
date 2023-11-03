:original_name: dli_08_0239.html

.. _dli_08_0239:

Open-Source Kafka Source Stream
===============================

Function
--------

Create a source stream to obtain data from Kafka as input data for jobs.

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

   CREATE SOURCE STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "kafka",
       kafka_bootstrap_servers = "",
       kafka_group_id = "",
       kafka_topic = "",
       encode = "json",
       json_config=""
     );

Keywords
--------

.. table:: **Table 1** Keyword description

   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory             | Description                                                                                                                                                                                                                          |
   +=========================+=======================+======================================================================================================================================================================================================================================+
   | type                    | Yes                   | Data source type. **Kafka** indicates that the data source is Kafka.                                                                                                                                                                 |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_bootstrap_servers | Yes                   | Port that connects DLI to Kafka. Use enhanced datasource connections to connect DLI queues with Kafka clusters.                                                                                                                      |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_group_id          | No                    | Group ID.                                                                                                                                                                                                                            |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_topic             | Yes                   | Kafka topic to be read. Currently, only one topic can be read at a time.                                                                                                                                                             |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode                  | Yes                   | Data encoding format. The value can be **csv**, **json**, **blob**, or **user_defined**.                                                                                                                                             |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | -  **field_delimiter** must be specified if this parameter is set to **csv**.                                                                                                                                                        |
   |                         |                       | -  **json_config** must be specified if this parameter is set to **json**.                                                                                                                                                           |
   |                         |                       | -  If this parameter is set to **blob**, the received data will not be parsed, and only one Array[TINYINT] field exists in the table.                                                                                                |
   |                         |                       | -  **encode_class_name** and **encode_class_parameter** must be specified if this parameter is set to **user_defined**.                                                                                                              |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode_class_name       | No                    | If **encode** is set to **user_defined**, you need to set this parameter to the name of the user-defined decoding class (including the complete package path). The class must inherit the **DeserializationSchema** class.           |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode_class_parameter  | No                    | If **encode** is set to **user_defined**, you can set this parameter to specify the input parameter of the user-defined decoding class. Only one parameter of the string type is supported.                                          |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json_config             | No                    | If **encode** is set to **json**, you can use this parameter to specify the mapping between JSON fields and stream attributes.                                                                                                       |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | The format is field1=json_field1;field2=json_field2.                                                                                                                                                                                 |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | **field1** and **field2** indicate the names of the created table fields. **json_field1** and **json_field2** are key fields of the JSON strings in the Kafka input data.                                                            |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | For details, see :ref:`Example <dli_08_0239__en-us_topic_0113887276_en-us_topic_0111499973_section444391525220>`.                                                                                                                    |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | .. note::                                                                                                                                                                                                                            |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       |    If the attribute names in the source stream are the same as those in JSON fields, you do not need to set this parameter.                                                                                                          |
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
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | If you set this parameter, only the data generated after the specified time for the Kafka topic will be read.                                                                                                                        |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_properties        | No                    | Native properties of Kafka. The format is **key1=value1;key2=value2**. For details about the property values, see the description in `Apache Kafka <https://kafka.apache.org/documentation/#configuration>`__.                       |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | kafka_certificate_name  | No                    | Name of the datasource authentication information. This parameter is valid only when the datasource authentication type is set to **Kafka_SSL**.                                                                                     |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       | .. note::                                                                                                                                                                                                                            |
   |                         |                       |                                                                                                                                                                                                                                      |
   |                         |                       |    -  If this parameter is specified, the service loads only the specified file and password under the authentication. The system automatically sets this parameter to **kafka_properties**.                                         |
   |                         |                       |    -  Other configuration information required for Kafka SSL authentication needs to be manually configured in the **kafka_properties** attribute.                                                                                   |
   +-------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

When creating a source stream, you can specify a time model for subsequent calculation. Currently, DLI supports two time models: Processing Time and Event Time. For details about the syntax, see :ref:`Configuring Time Models <dli_08_0107>`.

.. _dli_08_0239__en-us_topic_0113887276_en-us_topic_0111499973_section444391525220:

Example
-------

-  Read Kafka topic **test**. The data encoding format is non-nested JSON, for example, {"attr1": "lilei", "attr2": 18}.

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

-  Read Kafka topic **test**. The data is encoded in JSON format and nested. This example uses the complex data type ROW. For details about the syntax of ROW, see :ref:`Data Type <dli_08_0207>`.

   The test data is as follows:

   .. code-block::

      {
          "id":"1",
          "type2":"online",
          "data":{
              "patient_id":1234,
              "name":"bob1234"
          }
      }

   An example of the table creation statements is as follows:

   .. code-block::

      CREATE SOURCE STREAM kafka_source
      (
        id STRING,
        type2 STRING,
        data ROW<
          patient_id STRING,
          name STRING>
      )
      WITH (
        type = "kafka",
        kafka_bootstrap_servers = "ip1:port1,ip2:port2",
        kafka_group_id = "sourcegroup1",
        kafka_topic = "test",
        encode = "json"
      );

      CREATE SINK STREAM kafka_sink
      (
        id STRING,
        type2 STRING,
        patient_id STRING,
        name STRING
      )
        WITH (
          type="kafka",
          kafka_bootstrap_servers =  "ip1:port1,ip2:port2",
          kafka_topic = "testsink",
          encode = "csv"
        );

      INSERT INTO kafka_sink select id, type2, data.patient_id, data.name from kafka_source;
