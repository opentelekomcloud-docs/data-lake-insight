:original_name: dli_08_0414.html

.. _dli_08_0414:

Maxwell
=======

Function
--------

Flink supports to interpret Maxwell JSON messages as INSERT/UPDATE/DELETE messages into Flink SQL system. This is useful in many cases to leverage this feature,

such as:

-  Synchronizing incremental data from databases to other systems
-  Auditing logs
-  Real-time materialized views on databases
-  Temporal join changing history of a database table and so on

Flink also supports to encode the INSERT/UPDATE/DELETE messages in Flink SQL as Maxwell JSON messages, and emit to external systems like Kafka. However, currently Flink cannot combine UPDATE_BEFORE and UPDATE_AFTER into a single UPDATE message. Therefore, Flink encodes UPDATE_BEFORE and UDPATE_AFTER as DELETE and INSERT Maxwell messages.

Parameters
----------

.. table:: **Table 1**

   +----------------------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                              | Mandatory   | Default Value | Type        | Description                                                                                                                                                            |
   +========================================+=============+===============+=============+========================================================================================================================================================================+
   | format                                 | Yes         | None          | String      | Format to be used. Set this parameter to **maxwell-json**.                                                                                                             |
   +----------------------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | maxwell-json.ignore-parse-errors       | No          | false         | Boolean     | Whether fields and rows with parse errors will be skipped or failed. Fields are set to null in case of errors.                                                         |
   +----------------------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | maxwell-json.timestamp-format.standard | No          | 'SQL'         | String      | Input and output timestamp formats. Currently supported values are **SQL** and **ISO-8601**:                                                                           |
   |                                        |             |               |             |                                                                                                                                                                        |
   |                                        |             |               |             | **SQL** will parse input timestamp in "yyyy-MM-dd HH:mm:ss.s{precision}" format, for example, **2020-12-30 12:13:14.123** and output timestamp in the same format.     |
   |                                        |             |               |             |                                                                                                                                                                        |
   |                                        |             |               |             | **ISO-8601** will parse input timestamp in "yyyy-MM-ddTHH:mm:ss.s{precision}" format, for example **2020-12-30T12:13:14.123** and output timestamp in the same format. |
   +----------------------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | maxwell-json.map-null-key.mode         | No          | 'FAIL'        | String      | Handling mode when serializing null keys for map data. Currently supported values are 'FAIL', 'DROP' and 'LITERAL':                                                    |
   |                                        |             |               |             |                                                                                                                                                                        |
   |                                        |             |               |             | **FAIL** will throw exception when encountering map with null key.                                                                                                     |
   |                                        |             |               |             |                                                                                                                                                                        |
   |                                        |             |               |             | **DROP** will drop null key entries for map data.                                                                                                                      |
   |                                        |             |               |             |                                                                                                                                                                        |
   |                                        |             |               |             | **LITERAL** will replace null key with string literal. The string literal is defined by **maxwell-json.map-null-key.literal** option.                                  |
   +----------------------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | maxwell-json.map-null-key.literal      | No          | 'null'        | String      | String literal to replace null key when **maxwell-json.map-null-key.mode** is **LITERAL**.                                                                             |
   +----------------------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

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
          'properties.bootstrap.servers' = '<yourKafkaAddress1>:<yourKafkaPort>,<yourKafkaAddress2>:<yourKafkaPort>',
          'scan.startup.mode' = 'latest-offset',
          'format' = 'maxwell-json'
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

#. View the output through either of the following methods:

   -  Method 1: Locate the job and click **More** > **FlinkUI**. Choose **Task Managers** > **Stdout**.
   -  Method 2: If you allow DLI to save job logs in OBS, view the output in the **taskmanager.out** file.

   .. code-block::

      +I(111,scooter,Big 2-wheel scooter,5.15)
