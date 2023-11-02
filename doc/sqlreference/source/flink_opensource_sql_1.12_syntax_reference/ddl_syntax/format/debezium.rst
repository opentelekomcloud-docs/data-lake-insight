:original_name: dli_08_0412.html

.. _dli_08_0412:

Debezium
========

Function
--------

Debezium is a Changelog Data Capture (CDC) tool that can stream changes in real-time from other databases into Kafka. Debezium provides a unified format schema for changelog and supports to serialize messages using JSON.

Flink supports to interpret Debezium JSON and Avro messages as INSERT/UPDATE/DELETE messages into Flink SQL system. This is useful in many cases to leverage this feature, such as:

-  synchronizing incremental data from databases to other systems
-  Auditing logs
-  Real-time materialized view on databases
-  Temporal join changing history of a database table, etc.

Parameters
----------

.. table:: **Table 1**

   +-----------------------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                               | Mandatory   | Default Value | Mandatory   | Description                                                                                                                                                                                            |
   +=========================================+=============+===============+=============+========================================================================================================================================================================================================+
   | format                                  | Yes         | None          | String      | Format to be used. In this example.Set this parameter to **debezium-json**.                                                                                                                            |
   +-----------------------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-json.schema-include            | No          | false         | Boolean     | Whether the Debezium JSON messages contain the schema. When setting up Debezium Kafka Connect, enable the Kafka configuration **value.converter.schemas.enable** to include the schema in the message. |
   +-----------------------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-json.ignore-parse-errors       | No          | false         | Boolean     | Whether fields and rows with parse errors will be skipped or failed. The default value is **false**, indicating that an error will be thrown. Fields are set to null in case of errors.                |
   +-----------------------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-json.timestamp-format.standard | No          | 'SQL'         | String      | Input and output timestamp formats. Currently supported values are **SQL** and **ISO-8601**.                                                                                                           |
   |                                         |             |               |             |                                                                                                                                                                                                        |
   |                                         |             |               |             | -  **SQL** will parse input timestamp in "yyyy-MM-dd HH:mm:ss.s{precision}" format, for example **2020-12-30 12:13:14.123** and output timestamp in the same format.                                   |
   |                                         |             |               |             | -  **ISO-8601** will parse input timestamp in "yyyy-MM-ddTHH:mm:ss.s{precision}" format, for example **2020-12-30T12:13:14.123** and output timestamp in the same format.                              |
   +-----------------------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-json.map-null-key.mode         | No          | 'FAIL'        | String      | Handling mode when serializing null keys for map data. Available values are as follows:                                                                                                                |
   |                                         |             |               |             |                                                                                                                                                                                                        |
   |                                         |             |               |             | -  **FAIL** will throw exception when encountering map value with null key.                                                                                                                            |
   |                                         |             |               |             | -  **DROP** will drop null key entries for map data.                                                                                                                                                   |
   |                                         |             |               |             | -  **LITERAL** replaces the empty key value in the map with a string constant. The string literal is defined by **debezium-json.map-null-key.literal** option.                                         |
   +-----------------------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium-json.map-null-key.literal      | No          | 'null'        | String      | String literal to replace null key when **debezium-json.map-null-key.mode** is **LITERAL**.                                                                                                            |
   +-----------------------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Supported Connectors
--------------------

-  Kafka

Example
-------

Use Kafka to send data and output the data to print.

#. Create a datasource connection for the communication with the VPC and subnet where Kafka locates and bind the connection to the queue. Set a security group and inbound rule to allow access of the queue and test the connectivity of the queue using the Kafka IP address. For example, locate a general-purpose queue where the job runs and choose **More** > **Test Address Connectivity** in the **Operation** column. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job. Copy the following statement and submit the job:

   .. code-block::

      create table kafkaSource(
        id BIGINT,
        name STRING,
        description STRING,
        weight DECIMAL(10, 2)
        ) with (
          'connector' = 'kafka',
          'topic' = '<yourTopic>',
          'properties.group.id' = '<yourGroupId>',
          'properties.bootstrap.servers' = '<yourKafkaAddress>:<yourKafkaPort>',
          'scan.startup.mode' = 'latest-offset',
          'format' = 'debezium-json'
      );
      create table printSink(
        id BIGINT,
        name STRING,
        description STRING,
        weight DECIMAL(10, 2)
         ) with (
           'connector' = 'print'
         );
      insert into printSink select * from kafkaSource;

#. Insert the following data to the corresponding topic in Kafka:

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

#. View the output through either of the following methods:

   -  Method 1: Locate the job and click **More** > **FlinkUI**. Choose **Task Managers** > **Stdout**.
   -  Method 2: If you allow DLI to save job logs in OBS, view the output in the **taskmanager.out** file.

   .. code-block::

      -U(111,scooter,Big2-wheel scooter,5.18)
      +U(111,scooter,Big2-wheel scooter,5.15)
