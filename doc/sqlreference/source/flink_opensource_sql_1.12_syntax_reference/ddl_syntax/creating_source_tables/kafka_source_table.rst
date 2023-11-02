:original_name: dli_08_0386.html

.. _dli_08_0386:

Kafka Source Table
==================

Function
--------

Create a source stream to obtain data from Kafka as input data for jobs.

Apache Kafka is a fast, scalable, and fault-tolerant distributed message publishing and subscription system. It delivers high throughput and built-in partitions and provides data replicas and fault tolerance. Apache Kafka is applicable to scenarios of handling massive messages.

Prerequisites
-------------

-  You have created a Kafka cluster.
-  An enhanced datasource connection has been created for DLI to connect to Kafka clusters, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

Precautions
-----------

-  When creating a Flink OpenSource SQL job, you need to set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.
-  For details about how to use data types when creating tables, see :ref:`Format <dli_08_0407>`.
-  SASL_SSL cannot be enabled for the interconnected Kafka cluster.

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

Parameters
----------

.. table:: **Table 1** Parameter description

   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                               | Mandatory   | Default Value | Data Type                          | Description                                                                                                                                                                                             |
   +=========================================+=============+===============+====================================+=========================================================================================================================================================================================================+
   | connector                               | Yes         | None          | String                             | Connector to be used. Set this parameter to **kafka**.                                                                                                                                                  |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | topic                                   | Yes         | None          | String                             | Topic name of the Kafka record.                                                                                                                                                                         |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | Note:                                                                                                                                                                                                   |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | -  Only one of **topic** and **topic-pattern** can be specified.                                                                                                                                        |
   |                                         |             |               |                                    | -  If there are multiple topics, separate them with semicolons (;), for example, **topic-1;topic-2**.                                                                                                   |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | topic-pattern                           | No          | None          | String                             | Regular expression for a pattern of topic names to read from.                                                                                                                                           |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | Only one of **topic** and **topic-pattern** can be specified.                                                                                                                                           |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | For example:                                                                                                                                                                                            |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | 'topic.*'                                                                                                                                                                                               |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | '(topic-c|topic-d)'                                                                                                                                                                                     |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | '(topic-a|topic-b|topic-\\\\d*)'                                                                                                                                                                        |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | '(topic-a|topic-b|topic-[0-9]*)'                                                                                                                                                                        |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.bootstrap.servers            | Yes         | None          | String                             | Comma separated list of Kafka brokers.                                                                                                                                                                  |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.group.id                     | Yes         | None          | String                             | ID of the consumer group for the Kafka source.                                                                                                                                                          |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.\*                           | No          | None          | String                             | This parameter can set and pass arbitrary Kafka configurations.                                                                                                                                         |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | Note:                                                                                                                                                                                                   |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | -  The suffix to **properties.** must match the configuration key in `Apache Kafka <https://kafka.apache.org/documentation/#configuration>`__.                                                          |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    |    For example, you can disable automatic topic creation via **'properties.allow.auto.create.topics' = 'false'**.                                                                                       |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | -  Some configurations are not supported, for example, **'key.deserializer'** and **'value.deserializer'**.                                                                                             |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format                                  | Yes         | None          | String                             | Format used to deserialize and serialize the value part of Kafka messages. Note: Either this parameter or the **value.format** parameter is required.                                                   |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | Refer to :ref:`Format <dli_08_0407>` for more details and format parameters.                                                                                                                            |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.format                              | No          | None          | String                             | Format used to deserialize and serialize the key part of Kafka messages.                                                                                                                                |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | Note:                                                                                                                                                                                                   |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | -  If a key format is defined, the **key.fields** parameter is required as well. Otherwise, the Kafka records will have an empty key.                                                                   |
   |                                         |             |               |                                    | -  Refer to :ref:`Format <dli_08_0407>` for more details and format parameters.                                                                                                                         |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.fields                              | No          | []            | List<String>                       | Defines the columns in the table as the list of keys. This parameter must be configured in pair with **key.format**.                                                                                    |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | This parameter is left empty by default. Therefore, no key is defined.                                                                                                                                  |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | The format is like **field1;field2**.                                                                                                                                                                   |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.fields-prefix                       | No          | None          | String                             | Defines a custom prefix for all fields of the key format to avoid name clashes with fields of the value format.                                                                                         |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.format                            | Yes         | None          | String                             | Format used to deserialize and serialize the value part of Kafka messages.                                                                                                                              |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | Note:                                                                                                                                                                                                   |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | -  Either this parameter or the **format** parameter is required. If two parameters are configured, a conflict occurs.                                                                                  |
   |                                         |             |               |                                    | -  Refer to :ref:`Format <dli_08_0407>` for more details and format parameters.                                                                                                                         |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.fields-include                    | No          | ALL           | Enum                               | Whether to contain the key field when parsing the message body.                                                                                                                                         |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               | Possible values: [ALL, EXCEPT_KEY] | Possible values are:                                                                                                                                                                                    |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | -  **ALL** (default): All defined fields are included in the value of Kafka messages.                                                                                                                   |
   |                                         |             |               |                                    | -  **EXCEPT_KEY**: All the fields except those defined by **key.fields** are included in the value of Kafka messages.                                                                                   |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.startup.mode                       | No          | group-offsets | String                             | Start position for Kafka to read data.                                                                                                                                                                  |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | Possible values are:                                                                                                                                                                                    |
   |                                         |             |               |                                    |                                                                                                                                                                                                         |
   |                                         |             |               |                                    | -  **earliest-offset**: Data is read from the earliest Kafka offset.                                                                                                                                    |
   |                                         |             |               |                                    | -  **latest-offset**: Data is read from the latest Kafka offset.                                                                                                                                        |
   |                                         |             |               |                                    | -  **group-offsets** (default): Data is read based on the consumer group.                                                                                                                               |
   |                                         |             |               |                                    | -  **timestamp**: Data is read from a user-supplied timestamp. When setting this option, you also need to specify **scan.startup.timestamp-millis** in **WITH**.                                        |
   |                                         |             |               |                                    | -  **specific-offsets**: Data is read from user-supplied specific offsets for each partition. When setting this option, you also need to specify **scan.startup.specific-offsets** in **WITH**.         |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.startup.specific-offsets           | No          | None          | String                             | This parameter takes effect only when **scan.startup.mode** is set to **specific-offsets**. It specifies the offsets for each partition, for example, **partition:0,offset:42;partition:1,offset:300**. |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.startup.timestamp-millis           | No          | None          | Long                               | Startup timestamp. This parameter takes effect when **scan.startup.mode** is set to **timestamp**.                                                                                                      |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.topic-partition-discovery.interval | No          | None          | Duration                           | Interval for a consumer to periodically discover dynamically created Kafka topics and partitions.                                                                                                       |
   +-----------------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Metadata Column
---------------

You can define metadata columns in the source table to obtain the metadata of Kafka messages. For example, if multiple topics are defined in the **WITH** parameter and the metadata column is defined in the Kafka source table, the data read by Flink is labeled with the topic from which the data is read.

.. table:: **Table 2** Metadata column

   +-----------------+--------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------+
   | Key             | Data Type                                  | R/W             | Description                                                                                        |
   +=================+============================================+=================+====================================================================================================+
   | topic           | STRING NOT NULL                            | R               | Topic name of the Kafka record.                                                                    |
   +-----------------+--------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------+
   | partition       | INT NOT NULL                               | R               | Partition ID of the Kafka record.                                                                  |
   +-----------------+--------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------+
   | headers         | MAP<STRING, BYTES> NOT NULL                | R/W             | Headers of Kafka messages.                                                                         |
   +-----------------+--------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------+
   | leader-epoch    | INT NULL                                   | R               | Leader epoch of the Kafka record.                                                                  |
   |                 |                                            |                 |                                                                                                    |
   |                 |                                            |                 | :ref:`For details, see example 1. <dli_08_0386__en-us_topic_0000001310095781_li47891356134710>`    |
   +-----------------+--------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------+
   | offset          | BIGINT NOT NULL                            | R               | Offset of the Kafka record.                                                                        |
   +-----------------+--------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------+
   | timestamp       | TIMESTAMP(3) WITH LOCAL TIME ZONE NOT NULL | R/W             | Timestamp of the Kafka record.                                                                     |
   +-----------------+--------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------+
   | timestamp-type  | STRING NOT NULL                            | R               | Timestamp type of the Kafka record. The options are as follows:                                    |
   |                 |                                            |                 |                                                                                                    |
   |                 |                                            |                 | -  **NoTimestampType**: No timestamp is defined in the message.                                    |
   |                 |                                            |                 |                                                                                                    |
   |                 |                                            |                 | -  **CreateTime**: time when the message is generated.                                             |
   |                 |                                            |                 |                                                                                                    |
   |                 |                                            |                 | -  **LogAppendTime**: time when the message is added to the Kafka broker.                          |
   |                 |                                            |                 |                                                                                                    |
   |                 |                                            |                 |    :ref:`For details, see example 1. <dli_08_0386__en-us_topic_0000001310095781_li47891356134710>` |
   +-----------------+--------------------------------------------+-----------------+----------------------------------------------------------------------------------------------------+

Example (SASL_SSL Disabled for the Kafka Cluster)
-------------------------------------------------

-  .. _dli_08_0386__en-us_topic_0000001310095781_li47891356134710:

   **Example 1: Read data from the Kafka metadata column and write it to the Print sink.**

   #. Create an enhanced datasource connection in the VPC and subnet where Kafka locates, and bind the connection to the required Flink elastic resource pool.

   #. Set Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Kafka address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

   #. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

      When you create a job, set **Flink Version** to **1.12** on the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

      .. code-block::

         CREATE TABLE orders (
           `topic` String metadata,
           `partition` int metadata,
           `headers` MAP<STRING, BYTES> metadata,
           `leaderEpoch` INT metadata from 'leader-epoch',
           `offset` bigint metadata,
           `timestamp` TIMESTAMP(3) metadata,
           `timestampType` string metadata from 'timestamp-type',
           `message` string
         ) WITH (
           'connector' = 'kafka',
           'topic' = 'KafkaTopic',
           'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
           'properties.group.id' = 'GroupId',
           'scan.startup.mode' = 'latest-offset',
           "format" = "csv",
           "csv.field-delimiter" = "\u0001",
           "csv.quote-character" = "''"
         );

         CREATE TABLE printSink (
           `topic` String,
           `partition` int,
           `headers` MAP<STRING, BYTES>,
           `leaderEpoch` INT,
           `offset` bigint,
           `timestamp` TIMESTAMP(3),
           `timestampType` string,
           `message` string -- Indicates that data written by users is read from Kafka.
         ) WITH (
           'connector' = 'print'
         );

         insert into printSink select * from orders;

      If you need to read the value of each field instead of the entire message, use the following statements:

      .. code-block::

         CREATE TABLE orders (
           `topic` String metadata,
           `partition` int metadata,
           `headers` MAP<STRING, BYTES> metadata,
           `leaderEpoch` INT metadata from 'leader-epoch',
           `offset` bigint metadata,
           `timestamp` TIMESTAMP(3) metadata,
           `timestampType` string metadata from 'timestamp-type',
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
           'topic' = '<yourTopic>',
           'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
           'properties.group.id' = 'GroupId',
           'scan.startup.mode' = 'latest-offset',
           "format" = "json"
         );

         CREATE TABLE printSink (
           `topic` String,
           `partition` int,
           `headers` MAP<STRING, BYTES>,
           `leaderEpoch` INT,
           `offset` bigint,
           `timestamp` TIMESTAMP(3),
           `timestampType` string,
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
           'connector' = 'print'
         );

         insert into printSink select * from orders;

   #. Send the following data to the corresponding topics in Kafka:

      .. code-block::

         {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

         {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

         {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

   #. Perform the following operations to view the output:

      a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
      b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
      c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **taskmanager.out** file, and view result logs.

      The data result is as follows:

      .. code-block::

         +I(fz-source-json,0,{},0,243,2021-12-27T09:23:32.253,CreateTime,{"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"})
         +I(fz-source-json,0,{},0,244,2021-12-27T09:23:39.655,CreateTime,{"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"})
         +I(fz-source-json,0,{},0,245,2021-12-27T09:23:48.405,CreateTime,{"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"})

-  **Example 2: Use the Kafka source table and Print result table to read JSON data from Kafka and output it to the log file.**

   #. Create an enhanced datasource connection in the VPC and subnet where Kafka locates, and bind the connection to the required Flink elastic resource pool.

   #. Set Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Kafka address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

   #. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

      When you create a job, set **Flink Version** to **1.12** on the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

      .. code-block::

         CREATE TABLE orders (
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
           'topic' = '<yourTopic>',
           'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
           'properties.group.id' = 'GroupId',
           'scan.startup.mode' = 'latest-offset',
           "format" = "json"
         );

         CREATE TABLE printSink (
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

         insert into printSink select * from orders;

   #. Send the following test data to the corresponding topics in Kafka:

      .. code-block::

         {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

         {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

         {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

   #. Perform the following operations to view the output:

      a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
      b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
      c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **taskmanager.out** file, and view result logs.

      The data result is as follows:

      .. code-block::

         +I(202103241000000001,webShop,2021-03-24T10:00,100.0,100.0,2021-03-2410:02:03,0001,Alice,330106)
         +I(202103241606060001,appShop,2021-03-24T16:06:06,200.0,180.0,2021-03-2416:10:06,0001,Alice,330106)
         +I(202103251202020001,miniAppShop,2021-03-25T12:02:02,60.0,60.0,2021-03-2512:03:00,0002,Bob,330110)

Example (SASL_SSL Enabled for the Kafka Cluster)
------------------------------------------------

-  **Example 1: Enable SASL_SSL authentication for the DMS cluster.**

   Create a Kafka cluster for DMS, enable SASL_SSL, download the SSL certificate, and upload the downloaded certificate **client.jks** to an OBS bucket.

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
        'topic' = 'xx',
        'properties.bootstrap.servers' = 'xx:9093,xx:9093,xx:9093',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'properties.connector.auth.open' = 'true',
        'properties.ssl.truststore.location' = 'obs://xx/xx.jks',  -- Location where the user uploads the certificate to
        'properties.sasl.mechanism' = 'PLAIN',  --  Value format: SASL_PLAINTEXT
        'properties.security.protocol' = 'SASL_SSL',
        'properties.sasl.jaas.config' = 'org.apache.kafka.common.security.plain.PlainLoginModule required username=\"xx\" password=\"xx\";', -- Account and password set when the Kafka cluster is created
        "format" = "json"
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
        'connector' = 'kafka',
        'topic' = 'xx',
        'properties.bootstrap.servers' = 'xx:9093,xx:9093,xx:9093',
        'properties.connector.auth.open' = 'true',
        'properties.ssl.truststore.location' = 'obs://xx/xx.jks',
        'properties.sasl.mechanism' = 'PLAIN',
        'properties.security.protocol' = 'SASL_SSL',
        'properties.sasl.jaas.config' = 'org.apache.kafka.common.security.plain.PlainLoginModule required username=\"xx\" password=\"xx\";',
        "format" = "json"
      );

      insert into ordersSink select * from ordersSource;

-  **Example 2: Enable Kafka SASL_SSL authentication for the MRS cluster.**

   -  Enable Kerberos authentication for the MRS cluster.

   -  Click the **Components** tab and click **Kafka**. In the displayed page, click the **Service Configuration** tab, locate the **security.protocol**, and set it to **SASL_SSL**.

   -  Log in to the FusionInsight Manager of the MRS cluster and download the user credential. Choose **System** > **Permission** > **User**. Locate the row that contains the target user, choose **More** > **Download Authentication Credential**.

      Obtain the **truststore.jks** file using the authentication credential and store the credential and **truststore.jks** file in OBS.

   -  If "Message stream modified (41)" is displayed, the JDK version may be incorrect. Change the JDK version in the sample code to a version earlier than 8u_242 or delete the **renew_lifetime = 0m** configuration item from the **krb5.conf** configuration file.

   -  Set the port to the **sasl_ssl.port** configured in the Kafka service configuration.

   -  In the following statements, set **security.protocol** to **SASL_SSL**.

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
        'topic' = 'xx',
        'properties.bootstrap.servers' = 'xx:21009,xx:21009',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'properties.sasl.kerberos.service.name' = 'kafka',
        'properties.connector.auth.open' = 'true',
        'properties.connector.kerberos.principal' = 'xx', --Username
        'properties.connector.kerberos.krb5' = 'obs://xx/krb5.conf',
        'properties.connector.kerberos.keytab' = 'obs://xx/user.keytab',
        'properties.security.protocol' = 'SASL_SSL',
        'properties.ssl.truststore.location' = 'obs://xx/truststore.jks',
        'properties.ssl.truststore.password' = 'xx',  -- Password set for generating truststore.jks
        'properties.sasl.mechanism' = 'GSSAPI',
        "format" = "json"
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
        'connector' = 'kafka',
        'topic' = 'xx',
        'properties.bootstrap.servers' = 'xx:21009,xx:21009',
        'properties.sasl.kerberos.service.name' = 'kafka',
        'properties.connector.auth.open' = 'true',
        'properties.connector.kerberos.principal' = 'xx',
        'properties.connector.kerberos.krb5' = 'obs://xx/krb5.conf',
        'properties.connector.kerberos.keytab' = 'obs://xx/user.keytab',
        'properties.ssl.truststore.location' = 'obs://xx/truststore.jks',
        'properties.ssl.truststore.password' = 'xx',
        'properties.security.protocol' = 'SASL_SSL',
        'properties.sasl.mechanism' = 'GSSAPI',
        "format" = "json"
      );

      insert into ordersSink select * from ordersSource;

-  **Example 3: Enable Kerberos SASL_PAINTEXT authentication for the MRS cluster**

   -  Enable Kerberos authentication for the MRS cluster.
   -  Click the **Components** tab and click **Kafka**. In the displayed page, click the **Service Configuration** tab, locate the **security.protocol**, and set it to **SASL_PLAINTEXT**.
   -  Log in to the FusionInsight Manager of the MRS cluster and download the user credential. Choose **System** > **Permission** > **User**. Locate the row that contains the target user, choose **More** > **Download Authentication Credential**. Upload the credential to OBS.
   -  If error message "Message stream modified (41)" is displayed, the JDK version may be incorrect. Change the JDK version in the sample code to a version earlier than 8u_242 or delete the **renew_lifetime = 0m** configuration item from the **krb5.conf** configuration file.
   -  Set the port to the **sasl.port** configured in the Kafka service configuration.
   -  In the following statements, set **security.protocol** to **SASL_PLAINTEXT**.

   .. code-block::

      CREATE TABLE ordersSources (
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
        'topic' = 'xx',
        'properties.bootstrap.servers' = 'xx:21007,xx:21007',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'properties.sasl.kerberos.service.name' = 'kafka',
        'properties.connector.auth.open' = 'true',
        'properties.connector.kerberos.principal' = 'xx',
        'properties.connector.kerberos.krb5' = 'obs://xx/krb5.conf',
        'properties.connector.kerberos.keytab' = 'obs://xx/user.keytab',
        'properties.security.protocol' = 'SASL_PLAINTEXT',
        'properties.sasl.mechanism' = 'GSSAPI',
        "format" = "json"
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
        'connector' = 'kafka',
        'topic' = 'xx',
        'properties.bootstrap.servers' = 'xx:21007,xx:21007',
        'properties.sasl.kerberos.service.name' = 'kafka',
        'properties.connector.auth.open' = 'true',
        'properties.connector.kerberos.principal' = 'xx',
        'properties.connector.kerberos.krb5' = 'obs://xx/krb5.conf',
        'properties.connector.kerberos.keytab' = 'obs://xx/user.keytab',
        'properties.security.protocol' = 'SASL_PLAINTEXT',
        'properties.sasl.mechanism' = 'GSSAPI',
        "format" = "json"
      );

      insert into ordersSink select * from ordersSource;

-  **Example 4: Use SSL for the MRS cluster**

   -  Do not enable Kerberos authentication for the MRS cluster.

   -  Log in to the FusionInsight Manager of the MRS cluster and download the user credential. Choose **System** > **Permission** > **User**. Locate the row that contains the target user, choose **More** > **Download Authentication Credential**.

      Obtain the **truststore.jks** file using the authentication credential and store the credential and **truststore.jks** file in OBS.

   -  Set the port to the **ssl.port** configured in the Kafka service configuration.

   -  In the following statements, set **security.protocol** to **SSL**.

   -  Set **ssl.mode.enable** to **true**.

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
           'topic' = 'xx',
           'properties.bootstrap.servers' = 'xx:9093,xx:9093,xx:9093',
           'properties.group.id' = 'GroupId',
           'scan.startup.mode' = 'latest-offset',
           'properties.connector.auth.open' = 'true',
           'properties.ssl.truststore.location' = 'obs://xx/truststore.jks',
           'properties.ssl.truststore.password' = 'xx',  -- Password set for generating truststore.jks
           'properties.security.protocol' = 'SSL',
           "format" = "json"
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

   A: The datasource connection is not bound, the binding fails, or the security group of the Kafka cluster is not configured to allow access from the network segment of the DLI queue. Configure the datasource connection or configure the security group of the Kafka cluster to allow access from the DLI queue.

-  **Q: What should I do if the Flink job execution fails and the log contains the following error information?**

   .. code-block::

      Caused by: java.lang.RuntimeException: RealLine:45;Table 'default_catalog.default_database.printSink' declares persistable metadata columns, but the underlying DynamicTableSink doesn't implement the SupportsWritingMetadata interface. If the column should not be persisted, it can be declared with the VIRTUAL keyword.

   A: The metadata type is defined in the sink table, but the Print connector does not support deletion of matadata from the sink table.
