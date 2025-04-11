:original_name: dli_08_15017.html

.. _dli_08_15017:

Canal
=====

Function
--------

Canal is a Changelog Data Capture (CDC) tool that can stream changes in real-time from MySQL into other systems. Canal provides a unified format schema for changelog and supports to serialize messages using JSON and protobuf (the default format for Canal).

Flink supports to interpret Canal JSON messages as INSERT, UPDATE, and DELETE messages into the Flink SQL system. This is useful in many cases to leverage this feature, such as:

-  synchronizing incremental data from databases to other systems
-  Auditing logs
-  Real-time materialized view on databases
-  Temporal join changing history of a database table, etc.

Flink also supports to encode the INSERT, UPDATE, and DELETE messages in Flink SQL as Canal JSON messages, and emit to storage like Kafka. However, currently Flink cannot combine UPDATE_BEFORE and UPDATE_AFTER into a single UPDATE message. Therefore, Flink encodes UPDATE_BEFORE and UPDATE_AFTER as DELETE and INSERT Canal messages.

For details, see `Canal Format <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/formats/canal/>`__.

Supported Connectors
--------------------

-  Kafka
-  FileSystem

Parameters
----------

.. table:: **Table 1** Parameter description

   +-------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                 | Mandatory   | Default Value | Type        | Description                                                                                                                                                                                                                                                                               |
   +===========================================+=============+===============+=============+===========================================================================================================================================================================================================================================================================================+
   | format                                    | Yes         | None          | String      | Format to be used. In this example.Set this parameter to **canal-json**.                                                                                                                                                                                                                  |
   +-------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | canal-json.ignore-parse-errors            | No          | false         | Boolean     | Whether fields and rows with parse errors will be skipped or failed. The default value is **false**, indicating that an error will be thrown. Fields are set to null in case of errors.                                                                                                   |
   +-------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | canal-json.timestamp-format.standard      | No          | 'SQL'         | String      | Input and output timestamp formats. Currently supported values are **SQL** and **ISO-8601**:                                                                                                                                                                                              |
   |                                           |             |               |             |                                                                                                                                                                                                                                                                                           |
   |                                           |             |               |             | -  **SQL** will parse input timestamp in "yyyy-MM-dd HH:mm:ss.s{precision}" format, for example **2020-12-30 12:13:14.123** and output timestamp in the same format.                                                                                                                      |
   |                                           |             |               |             | -  **ISO-8601** will parse input timestamp in "yyyy-MM-ddTHH:mm:ss.s{precision}" format, for example **2020-12-30T12:13:14.123** and output timestamp in the same format.                                                                                                                 |
   +-------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | canal-json.map-null-key.mode              | No          | 'FALL'        | String      | Handling mode when serializing null keys for map data. Available values are as follows:                                                                                                                                                                                                   |
   |                                           |             |               |             |                                                                                                                                                                                                                                                                                           |
   |                                           |             |               |             | -  **FAIL** will throw exception when encountering map value with null key.                                                                                                                                                                                                               |
   |                                           |             |               |             | -  **DROP** will drop null key entries for map data.                                                                                                                                                                                                                                      |
   |                                           |             |               |             | -  **LITERAL** replaces the empty key value in the map with a string constant. The string literal is defined by **canal-json.map-null-key.literal** option.                                                                                                                               |
   +-------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | canal-json.map-null-key.literal           | No          | 'null'        | String      | String literal to replace null key when **canal-json.map-null-key.mode** is **LITERAL**.                                                                                                                                                                                                  |
   +-------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | canal-json.encode.decimal-as-plain-number | No          | false         | Boolean     | Encode all decimals as plain numbers instead of possible scientific notations. By default, decimals may be written using scientific notation. For example, **0.000000027** is encoded as **2.7E-8** by default, and will be written as **0.000000027** if set this parameter to **true**. |
   +-------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | canal-json.database.include               | No          | None          | String      | An optional regular expression to only read the specific databases changelog rows by regular matching the "database" meta field in the Canal record. The pattern string is compatible with Java's `Pattern <https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html>`__.   |
   +-------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | canal-json.table.include                  | No          | None          | String      | An optional regular expression to only read the specific tables changelog rows by regular matching the "table" meta field in the Canal record. The pattern string is compatible with Java's `Pattern <https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html>`__.         |
   +-------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Metadata
--------

The following format metadata can be exposed as read-only (VIRTUAL) columns in DDL.

Format metadata fields are only available if the corresponding connector forwards format metadata. Currently, only the Kafka connector is able to expose metadata fields for its value format.

.. table:: **Table 2** Metadata

   +---------------------+-----------------------+----------------------------------------------------------------------------------------------------------------+
   | Key                 | Data Type             | Description                                                                                                    |
   +=====================+=======================+================================================================================================================+
   | database            | STRING NULL           | The originating database. Corresponds to the **database** field in the Canal record if available.              |
   +---------------------+-----------------------+----------------------------------------------------------------------------------------------------------------+
   | table               | STRING NULL           | The originating database table. Corresponds to the **table** field in the Canal record if available.           |
   +---------------------+-----------------------+----------------------------------------------------------------------------------------------------------------+
   | sql-type            | MAP<STRING, INT> NULL | Map of various sql types. Corresponds to the **sqlType** field in the Canal record if available.               |
   +---------------------+-----------------------+----------------------------------------------------------------------------------------------------------------+
   | pk-names            | ARRAY<STRING> NULL    | Array of primary key names. Corresponds to the **pkNames** field in the Canal record if available.             |
   +---------------------+-----------------------+----------------------------------------------------------------------------------------------------------------+
   | ingestion-timestamp | TIMESTAMP_LTZ(3) NULL | The timestamp at which the connector processed the event. Corresponds to the **ts** field in the Canal record. |
   +---------------------+-----------------------+----------------------------------------------------------------------------------------------------------------+

The following example shows how to access Canal metadata fields in Kafka:

.. code-block::

   CREATE TABLE KafkaTable (
     origin_database STRING METADATA FROM 'value.database' VIRTUAL,
     origin_table STRING METADATA FROM 'value.table' VIRTUAL,
     origin_sql_type MAP<STRING, INT> METADATA FROM 'value.sql-type' VIRTUAL,
     origin_pk_names ARRAY<STRING> METADATA FROM 'value.pk-names' VIRTUAL,
     origin_ts TIMESTAMP(3) METADATA FROM 'value.ingestion-timestamp' VIRTUAL,
     user_id BIGINT,
     item_id BIGINT,
     behavior STRING
   ) WITH (
     'connector' = 'kafka',
     'topic' = 'kafkaTopic',
     'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
     'properties.group.id' = 'GroupId',
     'scan.startup.mode' = 'earliest-offset',
     'value.format' = 'canal-json'
   );

Example
-------

Use canal-json to read Canal records in Kafka and output them to Print.

#. Create a datasource connection for the communication with the VPC and subnet where Kafka locates and bind the connection to the queue. Set a security group and inbound rule to allow access of the queue and test the connectivity of the queue using the Kafka IP address. For example, locate a general-purpose queue where the job runs and choose **More** > **Test Address Connectivity** in the **Operation** column. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job and select Flink 1.15. Copy the following statement and submit the job:

   .. code-block::

      create table kafkaSource(
        id bigint,
        name string,
        description string,
        weight DECIMAL(10, 2)
        ) with (
          'connector' = 'kafka',
          'topic' = '<yourTopic>',
          'properties.group.id' = '<yourGroupId>',
          'properties.bootstrap.servers' = '<yourKafkaAddress>:<yourKafkaPort>',
          'scan.startup.mode' = 'latest-offset',
          'format' = 'canal-json'
      );
      create table printSink(
        id bigint,
        name string,
        description string,
        weight DECIMAL(10, 2)
         ) with (
           'connector' = 'print'
         );
      insert into printSink select * from kafkaSource;

#. Insert the data below into the appropriate Kafka topics. The data shows that the MySQL products table has four columns: **id**, **name**, **description**, and **weight**. This JSON message is an update event on the products table, indicating that the value of the **weight** field has changed from 5.15 to 5.18 for the row with id = 111.

   .. code-block::

      {
        "data": [
          {
            "id": "111",
            "name": "scooter",
            "description": "Big 2-wheel scooter",
            "weight": "5.18"
          }
        ],
        "database": "inventory",
        "es": 1589373560000,
        "id": 9,
        "isDdl": false,
        "mysqlType": {
          "id": "INTEGER",
          "name": "VARCHAR(255)",
          "description": "VARCHAR(512)",
          "weight": "FLOAT"
        },
        "old": [
          {
            "weight": "5.15"
          }
        ],
        "pkNames": [
          "id"
        ],
        "sql": "",
        "sqlType": {
          "id": 4,
          "name": 12,
          "description": 12,
          "weight": 7
        },
        "table": "products",
        "ts": 1589373560798,
        "type": "UPDATE"
      }

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **.out** file, and view result logs.

   .. code-block::

      -U[111, scooter, Big 2-wheel scooter, 5.15]
      +U[111, scooter, Big 2-wheel scooter, 5.18]
