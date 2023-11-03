:original_name: dli_08_0409.html

.. _dli_08_0409:

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

Parameters
----------

.. table:: **Table 1** Parameter description

   +--------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                            | Mandatory   | Default Value | Type        | Description                                                                                                                                                                             |
   +======================================+=============+===============+=============+=========================================================================================================================================================================================+
   | format                               | Yes         | None          | String      | Format to be used. In this example.Set this parameter to **canal-json**.                                                                                                                |
   +--------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | canal-json.ignore-parse-errors       | No          | false         | Boolean     | Whether fields and rows with parse errors will be skipped or failed. The default value is **false**, indicating that an error will be thrown. Fields are set to null in case of errors. |
   +--------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | canal-json.timestamp-format.standard | No          | 'SQL'         | String      | Input and output timestamp formats. Currently supported values are **SQL** and **ISO-8601**:                                                                                            |
   |                                      |             |               |             |                                                                                                                                                                                         |
   |                                      |             |               |             | -  **SQL** will parse input timestamp in "yyyy-MM-dd HH:mm:ss.s{precision}" format, for example **2020-12-30 12:13:14.123** and output timestamp in the same format.                    |
   |                                      |             |               |             | -  **ISO-8601** will parse input timestamp in "yyyy-MM-ddTHH:mm:ss.s{precision}" format, for example **2020-12-30T12:13:14.123** and output timestamp in the same format.               |
   +--------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | canal-json.map-null-key.mode         | No          | 'FALL'        | String      | Handling mode when serializing null keys for map data. Available values are as follows:                                                                                                 |
   |                                      |             |               |             |                                                                                                                                                                                         |
   |                                      |             |               |             | -  **FAIL** will throw exception when encountering map value with null key.                                                                                                             |
   |                                      |             |               |             | -  **DROP** will drop null key entries for map data.                                                                                                                                    |
   |                                      |             |               |             | -  **LITERAL** replaces the empty key value in the map with a string constant. The string literal is defined by **canal-json.map-null-key.literal** option.                             |
   +--------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | canal-json.map-null-key.literal      | No          | 'null'        | String      | String literal to replace null key when **canal-json.map-null-key.mode** is **LITERAL**.                                                                                                |
   +--------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | canal-json.database.include          | No          | None          | String      | An optional regular expression to only read the specific databases changelog rows by regular matching the **database** meta field in the Canal record.                                  |
   +--------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | canal-json.table.include             | No          | None          | String      | An optional regular expression to only read the specific tables changelog rows by regular matching the **table** meta field in the Canal record.                                        |
   +--------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Supported Connectors
--------------------

-  Kafka

Example
-------

Use Kafka to send data and output the data to print.

#. Create a datasource connection for the communication with the VPC and subnet where Kafka locates and bind the connection to the queue. Set a security group and inbound rule to allow access of the queue and test the connectivity of the queue using the Kafka IP address. For example, locate a general-purpose queue where the job runs and choose **More** > **Test Address Connectivity** in the **Operation** column. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job and select Flink 1.12. Copy the following statement and submit the job:

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

#. Insert the following data to the corresponding topic in Kafka:

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

#. View the output through either of the following methods:

   -  Method 1: Locate the job and click **More** > **FlinkUI**. Choose **Task Managers** > **Stdout**.
   -  Method 2: If you allow DLI to save job logs in OBS, view the output in the **taskmanager.out** file.

   .. code-block::

      -U(111,scooter,Big2-wheel scooter,5.15)
      +U(111,scooter,Big2-wheel scooter,5.18)
