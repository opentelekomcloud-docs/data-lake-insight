:original_name: dli_08_15018.html

.. _dli_08_15018:

Confluent Avro
==============

Function
--------

The Avro Schema Registry (**avro-confluent**) format allows you to read records that were serialized by the **io.confluent.kafka.serializers.KafkaAvroSerializer** and to write records that can in turn be read by the **io.confluent.kafka.serializers.KafkaAvroDeserializer**.

When reading (deserializing) a record with this format the Avro writer schema is fetched from the configured Confluent Schema Registry based on the schema version ID encoded in the record while the reader schema is inferred from table schema.

When writing (serializing) a record with this format the Avro schema is inferred from the table schema and used to retrieve a schema ID to be encoded with the data The lookup is performed with in the configured Confluent Schema Registry under the `subject <https://docs.confluent.io/current/schema-registry/index.html#schemas-subjects-and-topics>`__. The subject is specified by the avro-confluent.subject parameter.

Supported Connectors
--------------------

-  kafka
-  upsert kafka

Parameters
----------

.. table:: **Table 1** Parameter description

   +-----------------------------------------------+-----------+---------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                     | Mandatory | Default Value | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                        |
   +===============================================+===========+===============+========+====================================================================================================================================================================================================================================================================================================================================================================================================+
   | format                                        | Yes       | None          | String | Format to be used. Set this parameter to **'avro-confluent'**.                                                                                                                                                                                                                                                                                                                                     |
   +-----------------------------------------------+-----------+---------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avro-confluent.basic-auth.credentials-source  | No        | None          | String | Basic auth credentials source for Schema Registry                                                                                                                                                                                                                                                                                                                                                  |
   +-----------------------------------------------+-----------+---------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avro-confluent.basic-auth.user-info           | No        | None          | String | Basic auth user info for schema registry                                                                                                                                                                                                                                                                                                                                                           |
   +-----------------------------------------------+-----------+---------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avro-confluent.bearer-auth.credentials-source | No        | None          | String | Bearer auth credentials source for Schema Registry                                                                                                                                                                                                                                                                                                                                                 |
   +-----------------------------------------------+-----------+---------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avro-confluent.bearer-auth.token              | No        | None          | String | Bearer auth token for Schema Registry                                                                                                                                                                                                                                                                                                                                                              |
   +-----------------------------------------------+-----------+---------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avro-confluent.properties                     | No        | None          | Map    | Properties map that is forwarded to the underlying Schema Registry. This is useful for options that are not officially exposed via Flink config options. However, note that Flink options have higher precedence.                                                                                                                                                                                  |
   +-----------------------------------------------+-----------+---------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avro-confluent.ssl.keystore.location          | No        | None          | String | Location/File of SSL keystore                                                                                                                                                                                                                                                                                                                                                                      |
   +-----------------------------------------------+-----------+---------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avro-confluent.ssl.keystore.password          | No        | None          | String | Password for SSL keystore                                                                                                                                                                                                                                                                                                                                                                          |
   +-----------------------------------------------+-----------+---------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avro-confluent.ssl.truststore.location        | No        | None          | String | Location/File of SSL truststore                                                                                                                                                                                                                                                                                                                                                                    |
   +-----------------------------------------------+-----------+---------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avro-confluent.ssl.truststore.password        | No        | None          | String | Password for SSL truststore                                                                                                                                                                                                                                                                                                                                                                        |
   +-----------------------------------------------+-----------+---------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avro-confluent.subject                        | No        | None          | String | The Confluent Schema Registry subject under which to register the schema used by this format during serialization. By default, 'kafka' and 'upsert-kafka' connectors use '<topic_name>-value' or '<topic_name>-key' as the default subject name if this format is used as the value or key format. But for other connectors (e.g. 'filesystem'), the subject option is required when used as sink. |
   +-----------------------------------------------+-----------+---------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avro-confluent.url                            | No        | None          | String | The URL of the Confluent Schema Registry to fetch/register schemas.                                                                                                                                                                                                                                                                                                                                |
   +-----------------------------------------------+-----------+---------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Data Type Mapping
-----------------

Currently, Apache Flink always uses the table schema to derive the Avro reader schema during deserialization and Avro writer schema during serialization. Explicitly defining an Avro schema is not supported yet. See :ref:`Avro <dli_08_15016>` for the mapping between Avro and Flink DataTypes.

In addition to the types listed there, Flink supports reading/writing nullable types. Flink maps nullable types to Avro **union(something, null)**, where **something** is the Avro type converted from Flink type.

Example
-------

Read JSON data from the source topic in Kafka and write the data in Confluent Avro format to the sink topic.

#. Create a datasource connection for the communication with the VPC and subnet where Kafka and ECS locate and bind the connection to the queue. Set a security group and inbound rule to allow access of the queue and test the connectivity of the queue using the Kafka and ECS IP addresses. For example, locate a general-purpose queue where the job runs and choose **More** > **Test Address Connectivity** in the **Operation** column. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Purchase an ECS cluster, download `Confluent <https://packages.confluent.io/archive/5.5/>`__ 5.5.2 and jdk1.8.0_232, and upload them to the ECS cluster. Run the following command to decompress the packages (assume that the decompression directories are **confluent-5.5.2** and **jdk1.8.0_232**):

   .. code-block::

      tar zxvf confluent-5.5.2-2.11.tar.gz
      tar zxvf jdk1.8.0_232.tar.gz

#. Run the following commands to install jdk1.8.0_232 in the current ECS cluster. You can run the **pwd** command in the **jdk1.8.0_232 folder** to view the value of **yourJdkPath**.

   .. code-block::

      export JAVA_HOME=<yourJdkPath>
      export PATH=$JAVA_HOME/bin:$PATH
      export CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib

#. Go to the **confluent-5.5.2/etc/schema-registry/** directory and modify the following configuration items in the **schema-registry.properties** file:

   .. code-block::

      listeners=http://<yourEcsIp>:8081
      kafkastore.bootstrap.servers=<yourKafkaAddress1>:<yourKafkaPort>,<yourKafkaAddress2>:<yourKafkaPort>

#. Switch to the **confluent-5.5.2** directory and run the following command to start Confluent:

   .. code-block::

      bin/schema-registry-start etc/schema-registry/schema-registry.properties

#. Create a Flink OpenSource SQL job, select the Flink 1.15 version, and allow DLI to save job logs in OBS. Add the following statement to the job and submit it:

   .. code-block::

      CREATE TABLE kafkaSource (
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
        'topic' = 'kafkaSourceTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );

      CREATE TABLE kafkaSink (
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
        'topic' = 'kafkaSinkTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'format' = 'avro-confluent',
        'avro-confluent.url' = 'http://EcsIp:8081'
      );
      insert into kafkaSink select * from kafkaSource;

#. Insert the following data into Kafka:

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

#. Read the data of the sink Kafka topic. You will find that the data has been written and the schema has been saved to the **\_schema** topic of Kafka.
