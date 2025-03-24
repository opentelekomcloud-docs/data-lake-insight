:original_name: dli_08_15022.html

.. _dli_08_15022:

Maxwell
=======

Function
--------

Maxwell is a Changelog Data Capture (CDC) tool that can stream changes in real-time from MySQL into Kafka and other streaming connectors. Maxwell provides a unified format schema for changelog and supports to serialize messages using JSON.

Flink supports to interpret Maxwell JSON messages as INSERT/UPDATE/DELETE messages into Flink SQL system. This is useful in many cases to leverage this feature,

such as:

-  Synchronizing incremental data from databases to other systems
-  Auditing logs
-  Real-time materialized views on databases
-  Temporal join changing history of a database table and so on

Flink also supports to encode the INSERT/UPDATE/DELETE messages in Flink SQL as Maxwell JSON messages, and emit to external systems like Kafka. However, currently Flink cannot combine UPDATE_BEFORE and UPDATE_AFTER into a single UPDATE message. Therefore, Flink encodes UPDATE_BEFORE and UDPATE_AFTER as DELETE and INSERT Maxwell messages.

For details, see `Maxwell Format <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/formats/maxwell/>`__.

Supported Connectors
--------------------

-  Kafka
-  FileSystem

Caveats
-------

The Maxwell application allows to deliver every change event exactly-once. Flink works pretty well when consuming Maxwell produced events in this situation. If Maxwell application works in at-least-once delivery, it may deliver duplicate change events to Kafka and Flink will get the duplicate events. This may cause Flink query to get wrong results or unexpected exceptions. Thus, it is recommended setting job configuration **table.exec.source.cdc-events-duplicate** to **true** and define **PRIMARY KEY** on the source in this situation. Framework will generate an additional stateful operator, and use the primary key to deduplicate the change events and produce a normalized changelog stream.

Parameters
----------

.. table:: **Table 1** Parameters

   +---------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                   | Mandatory   | Default Value | Type        | Description                                                                                                                                                                                                                                                                               |
   +=============================================+=============+===============+=============+===========================================================================================================================================================================================================================================================================================+
   | format                                      | Yes         | None          | String      | Format to be used. Set this parameter to **'maxwell-json'**.                                                                                                                                                                                                                              |
   +---------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | maxwell-json.ignore-parse-errors            | No          | false         | Boolean     | Whether fields and rows with parse errors will be skipped or failed. Fields are set to null in case of errors.                                                                                                                                                                            |
   +---------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | maxwell-json.timestamp-format.standard      | No          | 'SQL'         | String      | Specify the input and output timestamp format. Currently supported values are **SQL** and **ISO-8601**:                                                                                                                                                                                   |
   |                                             |             |               |             |                                                                                                                                                                                                                                                                                           |
   |                                             |             |               |             | -  **SQL** will parse input timestamp in "yyyy-MM-dd HH:mm:ss.s{precision}" format, e.g '2020-12-30 12:13:14.123' and output timestamp in the same format.                                                                                                                                |
   |                                             |             |               |             | -  **ISO-8601** will parse input timestamp in "yyyy-MM-ddTHH:mm:ss.s{precision}" format, e.g '2020-12-30T12:13:14.123' and output timestamp in the same format.                                                                                                                           |
   +---------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | maxwell-json.map-null-key.mode              | No          | 'FAIL'        | String      | Specify the handling mode when serializing null keys for map data. Currently supported values are **FAIL**, **DROP**, and **LITERAL**:                                                                                                                                                    |
   |                                             |             |               |             |                                                                                                                                                                                                                                                                                           |
   |                                             |             |               |             | -  **FAIL** will throw exception when encountering map with null key.                                                                                                                                                                                                                     |
   |                                             |             |               |             | -  **DROP** will drop null key entries for map data.                                                                                                                                                                                                                                      |
   |                                             |             |               |             | -  **LITERAL** will replace null key with string literal. The string literal is defined by **maxwell-json.map-null-key.literal**.                                                                                                                                                         |
   +---------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | maxwell-json.map-null-key.literal           | No          | 'null'        | String      | Specify string literal to replace null key when **maxwell-json.map-null-key.mode** is **LITERAL**.                                                                                                                                                                                        |
   +---------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | maxwell-json.encode.decimal-as-plain-number | No          | false         | Boolean     | Encode all decimals as plain numbers instead of possible scientific notations. By default, decimals may be written using scientific notation. For example, **0.000000027** is encoded as **2.7E-8** by default, and will be written as **0.000000027** if set this parameter to **true**. |
   +---------------------------------------------+-------------+---------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Metadata
--------

The following format metadata can be exposed as read-only (VIRTUAL) columns in DDL.

.. table:: **Table 2** Metadata

   +---------------------+-----------------------+------------------------------------------------------------------------------------------------------------------+
   | Key                 | Data Type             | Description                                                                                                      |
   +=====================+=======================+==================================================================================================================+
   | database            | STRING NULL           | The originating database. Corresponds to the **database** field in the Maxwell record if available.              |
   +---------------------+-----------------------+------------------------------------------------------------------------------------------------------------------+
   | table               | STRING NULL           | The originating database table. Corresponds to the **table** field in the Maxwell record if available.           |
   +---------------------+-----------------------+------------------------------------------------------------------------------------------------------------------+
   | primary-key-columns | ARRAY<STRING> NULL    | Array of primary key names. Corresponds to the **primary_key_columns** field in the Maxwell record if available. |
   +---------------------+-----------------------+------------------------------------------------------------------------------------------------------------------+
   | ingestion-timestamp | TIMESTAMP_LTZ(3) NULL | The timestamp at which the connector processed the event. Corresponds to the **ts** field in the Maxwell record. |
   +---------------------+-----------------------+------------------------------------------------------------------------------------------------------------------+

The following is an example of using metadata:

.. code-block::

   CREATE TABLE KafkaTable (
     origin_database STRING METADATA FROM 'value.database' VIRTUAL,
     origin_table STRING METADATA FROM 'value.table' VIRTUAL,
     origin_primary_key_columns ARRAY<STRING> METADATA FROM 'value.primary-key-columns' VIRTUAL,
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
     'value.format' = 'maxwell-json'
   );

Example
-------

Use Kafka to send data and output the data to Print.

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
        'format' = 'maxwell-json'
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

#. Insert the data below into the appropriate Kafka topics (for details about the meaning of each field, see `Maxwell documentation <http://maxwells-daemon.io/dataformat/>`__):

   .. code-block::

      {
         "database":"test",
         "table":"e",
         "type":"insert",
         "ts":1477053217,
         "xid":23396,
         "commit":true,
         "position":"master.000006:800911",
         "server_id":23042,
         "thread_id":108,
         "primary_key": [1, "2016-10-21 05:33:37.523000"],
         "primary_key_columns": ["id", "c"],
         "data":{
           "id":111,
           "name":"scooter",
           "description":"Big 2-wheel scooter",
           "weight":5.15
         },
         "old":{
           "weight":5.18
         }
      }

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **.out** file, and view result logs.

   .. code-block::

      +I[111, scooter, Big 2-wheel scooter, 5.15]
