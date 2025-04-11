:original_name: dli_08_0398.html

.. _dli_08_0398:

Kafka Result Table
==================

Function
--------

DLI outputs the Flink job output data to Kafka through the Kafka result table.

Apache Kafka is a fast, scalable, and fault-tolerant distributed message publishing and subscription system. It delivers high throughput and built-in partitions and provides data replicas and fault tolerance. Apache Kafka is applicable to scenarios of handling massive messages.

Prerequisites
-------------

-  You have created a Kafka cluster.
-  An enhanced datasource connection has been created for DLI to connect to Kafka clusters, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

Precautions
-----------

-  When creating a Flink OpenSource SQL job, you need to set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.
-  For details about how to use data types, see section :ref:`Format <dli_08_0407>`.

Syntax
------

::

   create table kafkaSink(
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
     'connector' = 'kafka',
     'topic' = '',
     'properties.bootstrap.servers' = '',
     'format' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                    | Mandatory   | Default Value | Data Type                          | Description                                                                                                                                                                                                                                       |
   +==============================+=============+===============+====================================+===================================================================================================================================================================================================================================================+
   | connector                    | Yes         | None          | string                             | Connector to be used. Set this parameter to **kafka**.                                                                                                                                                                                            |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | topic                        | Yes         | None          | string                             | Topic name of the Kafka result table.                                                                                                                                                                                                             |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.bootstrap.servers | Yes         | None          | string                             | Kafka broker address. The value is in the format of **host:port,host:port,host:port**. Multiple **host:port** pairs are separated with commas (,).                                                                                                |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format                       | Yes         | None          | string                             | Format used by the Flink Kafka connector to serialize Kafka messages. Either this parameter or the **value.format** parameter is required.                                                                                                        |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | The following formats are supported:                                                                                                                                                                                                              |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | -  csv                                                                                                                                                                                                                                            |
   |                              |             |               |                                    | -  json                                                                                                                                                                                                                                           |
   |                              |             |               |                                    | -  avro                                                                                                                                                                                                                                           |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | Refer to :ref:`Format <dli_08_0407>` for more details and format parameters.                                                                                                                                                                      |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | topic-pattern                | No          | None          | String                             | Regular expression for matching the Kafka topic name.                                                                                                                                                                                             |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | Only one of **topic** and **topic-pattern** can be specified.                                                                                                                                                                                     |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | Example: 'topic.*'                                                                                                                                                                                                                                |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | '(topic-c|topic-d)'                                                                                                                                                                                                                               |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | '(topic-a|topic-b|topic-\\\\d*)'                                                                                                                                                                                                                  |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | '(topic-a|topic-b|topic-[0-9]*)'                                                                                                                                                                                                                  |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | properties.\*                | No          | None          | String                             | This parameter can set and pass arbitrary Kafka configurations.                                                                                                                                                                                   |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | Note:                                                                                                                                                                                                                                             |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | -  Suffix names must match the configuration key defined in `Apache Kafka <https://kafka.apache.org/documentation/#configuration>`__.                                                                                                             |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    |    For example, you can disable automatic topic creation via **'properties.allow.auto.create.topics' = 'false'**.                                                                                                                                 |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | -  Some configurations are not supported, for example, **'key.deserializer'** and **'value.deserializer'**.                                                                                                                                       |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.format                   | No          | None          | String                             | Format used to deserialize and serialize the key part of Kafka messages.                                                                                                                                                                          |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | Note:                                                                                                                                                                                                                                             |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | -  If a key format is defined, the **key.fields** parameter is required as well. Otherwise, the Kafka records will have an empty key.                                                                                                             |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | -  Possible values are:                                                                                                                                                                                                                           |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    |    csv                                                                                                                                                                                                                                            |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    |    json                                                                                                                                                                                                                                           |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    |    avro                                                                                                                                                                                                                                           |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    |    debezium-json                                                                                                                                                                                                                                  |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    |    canal-json                                                                                                                                                                                                                                     |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    |    maxwell-json                                                                                                                                                                                                                                   |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    |    avro-confluent                                                                                                                                                                                                                                 |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    |    raw                                                                                                                                                                                                                                            |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    |    Refer to :ref:`Format <dli_08_0407>` for more details and format parameters.                                                                                                                                                                   |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.fields                   | No          | []            | List<String>                       | Defines the columns in the table as the list of keys. This parameter must be configured in pair with **key.format**.                                                                                                                              |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | This parameter is left empty by default. Therefore, no key is defined.                                                                                                                                                                            |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | The format is like **field1;field2**.                                                                                                                                                                                                             |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.fields-prefix            | No          | None          | String                             | Defines a custom prefix for all fields of the key format to avoid name clashes with fields of the value format.                                                                                                                                   |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.format                 | Yes         | None          | String                             | Format used to deserialize and serialize the value part of Kafka messages.                                                                                                                                                                        |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | Note:                                                                                                                                                                                                                                             |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | -  Either this parameter or the **format** parameter is required. If two parameters are configured, a conflict occurs.                                                                                                                            |
   |                              |             |               |                                    | -  Refer to :ref:`Format <dli_08_0407>` for more details and format parameters.                                                                                                                                                                   |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value.fields-include         | No          | ALL           | Enum                               | Whether to contain the key field when parsing the message body.                                                                                                                                                                                   |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               | Possible values: [ALL, EXCEPT_KEY] | Possible values are:                                                                                                                                                                                                                              |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | -  **ALL** (default): All defined fields are included in the value of Kafka messages.                                                                                                                                                             |
   |                              |             |               |                                    | -  **EXCEPT_KEY**: All the fields except those defined by **key.fields** are included in the value of Kafka messages.                                                                                                                             |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.partitioner             | No          | None          | string                             | Mapping from Flink's partitions into Kafka's partitions. Valid values are as follows:                                                                                                                                                             |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | -  **fixed** (default): Each Flink partition ends up in at most one Kafka partition.                                                                                                                                                              |
   |                              |             |               |                                    | -  **round-robin**: A Flink partition is distributed to Kafka partitions in a round-robin manner.                                                                                                                                                 |
   |                              |             |               |                                    | -  **Custom FlinkKafkaPartitioner subclass**: If **fixed** and **round-robin** do not meet your requirements, you can create subclass **FlinkKafkaPartitioner** to customize the partition mapping, for example, **org.mycompany.MyPartitioner**. |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.semantic                | No          | at-least-once | String                             | Defines the delivery semantic for the Kafka sink.                                                                                                                                                                                                 |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | Valid values are as follows:                                                                                                                                                                                                                      |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | -  at-least-once                                                                                                                                                                                                                                  |
   |                              |             |               |                                    | -  exactly-once                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | -  none                                                                                                                                                                                                                                           |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.parallelism             | No          | None          | Integer                            | Defines the parallelism of the Kafka sink operator.                                                                                                                                                                                               |
   |                              |             |               |                                    |                                                                                                                                                                                                                                                   |
   |                              |             |               |                                    | By default, the parallelism is determined by the framework using the same parallelism of the upstream chained operator.                                                                                                                           |
   +------------------------------+-------------+---------------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example (SASL_SSL Disabled for the Kafka Cluster)
-------------------------------------------------

In this example, data is read from a Kafka topic and written to another using a Kafka result table.

#. Create an enhanced datasource connection in the VPC and subnet where Kafka locates, and bind the connection to the required Flink elastic resource pool.

#. Set Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Kafka address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

   When you create a job, set **Flink Version** to **1.12** on the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

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
        'topic' = 'KafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        "format" = "json"
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
        'topic' = 'KafkaSinkTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        "format" = "json"
      );

      insert into kafkaSink select * from kafkaSource;

#. Connect to the Kafka cluster and insert the following test data into the source topic in Kafka:

   .. code-block::

      {"order_id":"202103241000000001","order_channel":"webShop","order_time":"2021-03-24 10:00:00","pay_amount":100.0,"real_pay":100.0,"pay_time":"2021-03-24 10:02:03","user_id":"0001","user_name":"Alice","area_id":"330106"}

      {"order_id":"202103241606060001","order_channel":"appShop","order_time":"2021-03-24 16:06:06","pay_amount":200.0,"real_pay":180.0,"pay_time":"2021-03-24 16:10:06","user_id":"0001","user_name":"Alice","area_id":"330106"}

#. Connect to the Kafka cluster and read data from the sink topic of Kafka.

   .. code-block::

      {"order_id":"202103241000000001","order_channel":"webShop","order_time":"2021-03-24 10:00:00","pay_amount":100.0,"real_pay":100.0,"pay_time":"2021-03-24 10:02:03","user_id":"0001","user_name":"Alice","area_id":"330106"}

      {"order_id":"202103241606060001","order_channel":"appShop","order_time":"2021-03-24 16:06:06","pay_amount":200.0,"real_pay":180.0,"pay_time":"2021-03-24 16:10:06","user_id":"0001","user_name":"Alice","area_id":"330106"}

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

   -  Download the user credential. Log in to the MRS Manager of the MRS cluster and choose **System** > **Permission** > **User**. Locate the row that contains the target user, click **More**, and select **Download Authentication Credential**.

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
   -  Log in to the MRS Manager of the MRS cluster and download the user credential. Choose **System** > **Permission** > **User**. Locate the row that contains the target user, choose **More** > **Download Authentication Credential**. Upload the credential to OBS.
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

   -  Download the user credential. Log in to the MRS Manager of the MRS cluster and choose **System** > **Permission** > **User**. Locate the row that contains the target user, click **More**, and select **Download Authentication Credential**.

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
