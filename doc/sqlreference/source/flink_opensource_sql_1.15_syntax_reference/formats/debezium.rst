:original_name: dli_08_15020.html

.. _dli_08_15020:

Debezium
========

Function
--------

Debezium is a Changelog Data Capture (CDC) tool that can stream changes in real-time from MySQL, PostgreSQL, Oracle, Microsoft SQL Server and many other databases into Kafka. Debezium provides a unified format schema for changelog and supports to serialize messages using JSON and Apache Avro.

Flink supports to interpret Debezium JSON and Avro messages as INSERT/UPDATE/DELETE messages into Flink SQL system. This is useful in many cases to leverage this feature, such as

-  Synchronizing incremental data from databases to other systems
-  Auditing logs
-  Real-time materialized views on databases
-  Temporal join changing history of a database table

Flink also supports to encode the INSERT/UPDATE/DELETE messages in Flink SQL as Debezium JSON or Avro messages, and emit to external systems like Kafka. However, currently Flink cannot combine UPDATE_BEFORE and UPDATE_AFTER into a single UPDATE message. Therefore, Flink encodes UPDATE_BEFORE and UDPATE_AFTER as DELETE and INSERT Debezium messages.

For details, see `Debezium Format <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/formats/debezium/>`__.

Supported Connectors
--------------------

-  Kafka
-  FileSystem

Caveats
-------

-  Duplicate change events

   Under normal operating scenarios, the Debezium application delivers every change event exactly-once. Flink works pretty well when consuming Debezium produced events in this situation. However, Debezium application works in at-least-once delivery if any failover happens. That means, in the abnormal situations, Debezium may deliver duplicate change events to Kafka and Flink will get the duplicate events. This may cause Flink query to get wrong results or unexpected exceptions.

   Solution: Set `table.exec.source.cdc-events-duplicate <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/dev/table/config/#table-exec-source-cdc-events-duplicate>`__ to **true** and define a primary key on this source.

   Framework will generate an additional stateful operator, and use the primary key to deduplicate the change events and produce a normalized changelog stream.

   For more information, see `Debezium documentation <https://debezium.io/documentation/faq/#what_happens_when_an_application_stops_or_crashes>`__.

-  Consuming data produced by Debezium Postgres Connector

   If you are using Debezium Connector for PostgreSQL to capture the changes to Kafka, please make sure the `REPLICA IDENTITY <https://www.postgresql.org/docs/current/sql-altertable.html#SQL-CREATETABLE-REPLICA-IDENTITY>`__ configuration of the monitored PostgreSQL table has been set to **FULL** which is by default **DEFAULT**. Otherwise, Flink SQL currently will fail to interpret the Debezium data.

   In FULL strategy, the UPDATE and DELETE events will contain the previous values of all the table's columns.

   In other strategies, the **before** field of UPDATE and DELETE events will only contain primary key columns or null if no primary key.

   You can change the **REPLICA IDENTITY** by running **ALTER TABLE <your-table-name> REPLICA IDENTITY FULL**.

Parameter Description
---------------------

Flink provides **debezium-avro-confluent** and **debezium-json** formats to interpret Avro or Json messages produced by Debezium. Use format **debezium-avro-confluent** to interpret Debezium Avro messages and format **debezium-json** to interpret Debezium Json messages.

.. table:: **Table 1** Debezium Avro parameters

   +--------------------------------------------------------+-----------+---------------+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                              | Mandatory | Default Value | Data Type | Description                                                                                                                                                                                                                                                                                                                                                                                        |
   +========================================================+===========+===============+===========+====================================================================================================================================================================================================================================================================================================================================================================================================+
   | format                                                 | Yes       | None          | String    | Format to be used. Set this parameter to **'debezium-avro-confluent'**.                                                                                                                                                                                                                                                                                                                            |
   +--------------------------------------------------------+-----------+---------------+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-avro-confluent.basic-auth.credentials-source  | No        | None          | String    | Basic auth credentials source for Schema Registry                                                                                                                                                                                                                                                                                                                                                  |
   +--------------------------------------------------------+-----------+---------------+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-avro-confluent.basic-auth.user-info           | No        | None          | String    | Basic auth user info for schema registry                                                                                                                                                                                                                                                                                                                                                           |
   +--------------------------------------------------------+-----------+---------------+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-avro-confluent.bearer-auth.credentials-source | No        | None          | String    | Bearer auth credentials source for Schema Registry                                                                                                                                                                                                                                                                                                                                                 |
   +--------------------------------------------------------+-----------+---------------+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-avro-confluent.bearer-auth.token              | No        | None          | String    | Bearer auth token for Schema Registry                                                                                                                                                                                                                                                                                                                                                              |
   +--------------------------------------------------------+-----------+---------------+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-avro-confluent.properties                     | No        | None          | Map       | Properties map that is forwarded to the underlying Schema Registry. This is useful for options that are not officially exposed via Flink config options. However, note that Flink options have higher precedence.                                                                                                                                                                                  |
   +--------------------------------------------------------+-----------+---------------+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-avro-confluent.ssl.keystore.location          | No        | None          | String    | Location/File of SSL keystore                                                                                                                                                                                                                                                                                                                                                                      |
   +--------------------------------------------------------+-----------+---------------+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-avro-confluent.ssl.keystore.password          | No        | None          | String    | Password for SSL keystore                                                                                                                                                                                                                                                                                                                                                                          |
   +--------------------------------------------------------+-----------+---------------+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-avro-confluent.ssl.truststore.location        | No        | None          | String    | Location/File of SSL truststore                                                                                                                                                                                                                                                                                                                                                                    |
   +--------------------------------------------------------+-----------+---------------+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-avro-confluent.ssl.truststore.password        | No        | None          | String    | Password for SSL truststore                                                                                                                                                                                                                                                                                                                                                                        |
   +--------------------------------------------------------+-----------+---------------+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-avro-confluent.subject                        | No        | None          | String    | The Confluent Schema Registry subject under which to register the schema used by this format during serialization. By default, 'kafka' and 'upsert-kafka' connectors use '<topic_name>-value' or '<topic_name>-key' as the default subject name if this format is used as the value or key format. But for other connectors (e.g. 'filesystem'), the subject option is required when used as sink. |
   +--------------------------------------------------------+-----------+---------------+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-avro-confluent.url                            | No        | None          | String    | The URL of the Confluent Schema Registry to fetch/register schemas.                                                                                                                                                                                                                                                                                                                                |
   +--------------------------------------------------------+-----------+---------------+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. table:: **Table 2** Debezium JSON parameters

   +----------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                    | Mandatory   | Default Value | Mandatory   | Description                                                                                                                                                                                                                |
   +==============================================+=============+===============+=============+============================================================================================================================================================================================================================+
   | format                                       | Yes         | None          | String      | Format to be used. In this example.Set this parameter to **debezium-json**.                                                                                                                                                |
   +----------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-json.schema-include                 | No          | false         | Boolean     | Whether the Debezium JSON messages contain the schema. When setting up Debezium Kafka Connect, enable the Kafka configuration **value.converter.schemas.enable** to include the schema in the message.                     |
   +----------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-json.ignore-parse-errors            | No          | false         | Boolean     | Whether fields and rows with parse errors will be skipped or failed. The default value is **false**, indicating that an error will be thrown. Fields are set to null in case of errors.                                    |
   +----------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-json.timestamp-format.standard      | No          | 'SQL'         | String      | Input and output timestamp formats. Currently supported values are **SQL** and **ISO-8601**.                                                                                                                               |
   |                                              |             |               |             |                                                                                                                                                                                                                            |
   |                                              |             |               |             | -  **SQL** will parse input timestamp in "yyyy-MM-dd HH:mm:ss.s{precision}" format, for example **2020-12-30 12:13:14.123** and output timestamp in the same format.                                                       |
   |                                              |             |               |             | -  **ISO-8601** will parse input timestamp in "yyyy-MM-ddTHH:mm:ss.s{precision}" format, for example **2020-12-30T12:13:14.123** and output timestamp in the same format.                                                  |
   +----------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-json.map-null-key.mode              | No          | 'FAIL'        | String      | Handling mode when serializing null keys for map data. Available values are as follows:                                                                                                                                    |
   |                                              |             |               |             |                                                                                                                                                                                                                            |
   |                                              |             |               |             | -  **FAIL** will throw exception when encountering map value with null key.                                                                                                                                                |
   |                                              |             |               |             | -  **DROP** will drop null key entries for map data.                                                                                                                                                                       |
   |                                              |             |               |             | -  **LITERAL** replaces the empty key value in the map with a string constant. The string literal is defined by **debezium-json.map-null-key.literal** option.                                                             |
   +----------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-json.map-null-key.literal           | No          | 'null'        | String      | String literal to replace null key when **debezium-json.map-null-key.mode** is **LITERAL**.                                                                                                                                |
   +----------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-json.encode.decimal-as-plain-number | No          | false         | Boolean     | Encode all decimals as plain numbers instead of possible scientific notations. For example, **0.000000027** is encoded as **2.7E-8** by default, and will be written as **0.000000027** if set this parameter to **true**. |
   +----------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Metadata
--------

The following format metadata can be exposed as read-only (VIRTUAL) columns in DDL.

Format metadata fields are only available if the corresponding connector forwards format metadata. Currently, only the Kafka connector is able to expose metadata fields for its value format.

.. table:: **Table 3** Metadata

   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | Key                 | Data Type                | Description                                                                                                                             |
   +=====================+==========================+=========================================================================================================================================+
   | schema              | STRING NULL              | JSON string describing the schema of the payload. **Null** if the schema is not included in the Debezium record.                        |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | ingestion-timestamp | TIMESTAMP_LTZ(3) NULL    | The timestamp at which the connector processed the event. Corresponds to the **ts_ms** field in the Debezium record.                    |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | source.timestamp    | TIMESTAMP_LTZ(3) NULL    | The timestamp at which the source system created the event. Corresponds to the **source.ts_ms** field in the Debezium record.           |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | source.database     | STRING NULL              | The originating database. Corresponds to the **source.db** field in the Debezium record if available.                                   |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | source.schema       | STRING NULL              | The originating database schema. Corresponds to the **source.schema** field in the Debezium record if available.                        |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | source.table        | STRING NULL              | The originating database table. Corresponds to the **source.table** or **source.collection** field in the Debezium record if available. |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | source.properties   | MAP<STRING, STRING> NULL | Map of various source properties. Corresponds to the **source** field in the Debezium record.                                           |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+

The following example shows how to access Canal metadata fields in Kafka:

.. code-block::

   CREATE TABLE KafkaTable (
     origin_ts TIMESTAMP(3) METADATA FROM 'value.ingestion-timestamp' VIRTUAL,
     event_time TIMESTAMP(3) METADATA FROM 'value.source.timestamp' VIRTUAL,
     origin_database STRING METADATA FROM 'value.source.database' VIRTUAL,
     origin_schema STRING METADATA FROM 'value.source.schema' VIRTUAL,
     origin_table STRING METADATA FROM 'value.source.table' VIRTUAL,
     origin_properties MAP<STRING, STRING> METADATA FROM 'value.source.properties' VIRTUAL,
     user_id BIGINT,
     item_id BIGINT,
     behavior STRING
   ) WITH (
     'connector' = 'kafka',
     'topic' = 'kafkaTopic',
     'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
     'properties.group.id' = 'GroupId',
     'scan.startup.mode' = 'earliest-offset',
     'value.format' = 'debezium-json'
   );

Example
-------

Use Kafka to parse Debezium JSON data and output the result to Print.

#. Create a datasource connection for the communication with the VPC and subnet where Kafka locates and bind the connection to the queue. Set a security group and inbound rule to allow access of the queue and test the connectivity of the queue using the Kafka IP address. For example, locate a general-purpose queue where the job runs and choose **More** > **Test Address Connectivity** in the **Operation** column. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job and select Flink 1.15. Copy the following statement and submit the job:

   .. code-block::

      CREATE TABLE kafkaSource (
        id bigint,
        name string,
        description string,
        weight DECIMAL(10, 2)
      ) WITH (
        'connector' = 'kafka',
        'topic' = 'kafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'debezium-json'
      );
      CREATE TABLE printSink (
        id bigint,
        name string,
        description string,
        weight DECIMAL(10, 2)
      ) WITH (
        'connector' = 'print'
      );
      insert into printSink select * from kafkaSource;

#. Insert the data below into the appropriate Kafka topics. The data shows that the MySQL products table has four columns: **id**, **name**, **description**, and **weight**. This JSON message represents an update event on the products table, where the **weight** value of the row with id = 111 has been changed from 5.18 to 5.15.

   .. code-block::

      {
        "before": {
          "id": 111,
          "name": "scooter",
          "description": "Big 2-wheel scooter",
          "weight": 5.18
        },
        "after": {
          "id": 111,
          "name": "scooter",
          "description": "Big 2-wheel scooter",
          "weight": 5.15
        },
        "source": {
          "version": "0.9.5.Final",
          "connector": "mysql",
          "name": "fullfillment",
          "server_id" :1,
          "ts_sec": 1629607909,
          "gtid": "mysql-bin.000001",
          "pos": 2238,"row": 0,
          "snapshot": false,
          "thread": 7,
          "db": "inventory",
          "table": "test",
          "query": null},
        "op": "u",
        "ts_ms": 1589362330904,
        "transaction": null
      }

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **.out** file, and view result logs.

   .. code-block::

      -U[111, scooter, Big 2-wheel scooter, 5.18]
      +U[111, scooter, Big 2-wheel scooter, 5.15]
