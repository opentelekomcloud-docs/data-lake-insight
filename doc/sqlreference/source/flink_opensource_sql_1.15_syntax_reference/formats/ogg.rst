:original_name: dli_08_15023.html

.. _dli_08_15023:

Ogg
===

Function
--------

`Oracle GoldenGate <https://www.oracle.com/integration/goldengate/>`__ (a.k.a ogg) is a comprehensive software package for real-time data capture and replication in heterogeneous IT environments. The product set enables high availability solutions, real-time data integration, transactional change data capture, data replication, transformations, and verification between operational and analytical enterprise systems. Ogg provides a format schema for changelog and supports to serialize messages using JSON.

Flink supports to interpret Ogg JSON as INSERT/UPDATE/DELETE messages into Flink SQL system. This is useful in many cases to leverage this feature, such as:

-  Synchronizing incremental data from databases to other systems
-  Auditing logs
-  Real-time materialized views on databases
-  Temporal join changing history of a database table and so on.

Flink also supports to encode the INSERT/UPDATE/DELETE messages in Flink SQL as Ogg JSON, and emit to external systems like Kafka. However, currently Flink cannot combine UPDATE_BEFORE and UPDATE_AFTER into a single UPDATE message. Therefore, Flink encodes UPDATE_BEFORE and UPDATE_AFTER as DELETE and INSERT Ogg messages.

Supported Connectors
--------------------

-  Kafka
-  FileSystem

Parameter Description
---------------------

.. table:: **Table 1** Parameters

   +-----------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                               | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                                             |
   +=========================================+=============+===============+=============+=========================================================================================================================================================================================+
   | format                                  | Yes         | (none)        | String      | Specify what format to use, here should be **ogg-json**.                                                                                                                                |
   +-----------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ogg-json.ignore-parse-errors            | No          | false         | Boolean     | Whether fields and rows with parse errors will be skipped or failed. The default value is **false**, indicating that an error will be thrown. Fields are set to null in case of errors. |
   +-----------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-json.timestamp-format.standard | No          | 'SQL'         | String      | Input and output timestamp formats. Currently supported values are **SQL** and **ISO-8601**:                                                                                            |
   |                                         |             |               |             |                                                                                                                                                                                         |
   |                                         |             |               |             | -  **SQL** will parse input timestamp in "yyyy-MM-dd HH:mm:ss.s{precision}" format, for example **2020-12-30 12:13:14.123** and output timestamp in the same format.                    |
   |                                         |             |               |             | -  **ISO-8601** will parse input timestamp in "yyyy-MM-ddTHH:mm:ss.s{precision}" format, for example **2020-12-30T12:13:14.123** and output timestamp in the same format.               |
   +-----------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ogg-json.map-null-key.mode              | No          | 'FAIL'        | String      | Handling mode when serializing null keys for map data. Currently supported values are **FAIL**, **DROP**, and **LITERAL**:                                                              |
   |                                         |             |               |             |                                                                                                                                                                                         |
   |                                         |             |               |             | -  Option **FAIL** will throw exception when encountering map with null key.                                                                                                            |
   |                                         |             |               |             | -  Option **DROP** will drop null key entries for map data.                                                                                                                             |
   |                                         |             |               |             | -  Option **LITERAL** will replace null key with string literal. The string literal is defined by **ogg-json.map-null-key.literal**.                                                    |
   +-----------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ogg-json.map-null-key.literal           | No          | 'null'        | String      | Specify string literal to replace null key when **ogg-json.map-null-key.mode** is **LITERAL**.                                                                                          |
   +-----------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Metadata
--------

.. table:: **Table 2** Metadata

   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | Key                   | Data Type             | Description                                                                                                                             |
   +=======================+=======================+=========================================================================================================================================+
   | table                 | STRING NULL           | Contains fully qualified table name. The format of the fully qualified table name is **CATALOG NAME.SCHEMA NAME.TABLE NAME**.           |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | primary-keys          | ARRAY<STRING> NULL    | An array variable holding the column names of the primary keys of the source table.                                                     |
   |                       |                       |                                                                                                                                         |
   |                       |                       | The **primary-keys** field is only included in the JSON output if the **includePrimaryKeys** configuration property is set to **true**. |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | ingestion-timestamp   | TIMESTAMP_LTZ(6) NULL | The timestamp at which the connector processed the event. Corresponds to the **current_ts** field in the Ogg record.                    |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | event-timestamp       | TIMESTAMP_LTZ(6) NULL | The timestamp at which the source system created the event. Corresponds to the **op_ts** field in the Ogg record.                       |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+

The following example shows how to access Canal metadata fields in Kafka:

.. code-block::

   CREATE TABLE KafkaTable (
     origin_ts TIMESTAMP(3) METADATA FROM 'value.ingestion-timestamp' VIRTUAL,
     event_time TIMESTAMP(3) METADATA FROM 'value.event-timestamp' VIRTUAL,
     origin_table STRING METADATA FROM 'value.table' VIRTUAL,
     primary_keys ARRAY<STRING> METADATA FROM 'value.primary_keys' VIRTUAL,
     user_id BIGINT,
     item_id BIGINT,
     behavior STRING
   ) WITH (
     'connector' = 'kafka',
     'topic' = 'kafkaTopic',
     'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
     'properties.group.id' = 'GroupId',
     'scan.startup.mode' = 'earliest-offset',
     'value.format' = 'ogg-json'
   );

Example
-------

Use ogg-json to read Ogg records in Kafka and output them to Print.

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
        'format' = 'ogg-json'
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

#. Insert the data below into the appropriate Kafka topics. The data shows that the Oracle PRODUCTS table has four columns: **id**, **name**, **description**, and **weight**. This JSON message represents an update event on the PRODUCTS table, where the **weight** value of the row with id = 111 has been changed from 5.18 to 5.15.

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
        "op_type": "U",
        "op_ts": "2020-05-13 15:40:06.000000",
        "current_ts": "2020-05-13 15:40:07.000000",
        "primary_keys": [
          "id"
        ],
        "pos": "00000000000000000000143",
        "table": "PRODUCTS"
      }

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **.out** file, and view result logs.

   .. code-block::

      -U[111, scooter, Big 2-wheel scooter, 5.18]
      +U[111, scooter, Big 2-wheel scooter, 5.15]
