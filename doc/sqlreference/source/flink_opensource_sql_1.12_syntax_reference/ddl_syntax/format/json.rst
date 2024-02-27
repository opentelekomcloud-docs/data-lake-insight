:original_name: dli_08_0413.html

.. _dli_08_0413:

JSON
====

Function
--------

The JSON format allows you to read and write JSON data based on a JSON schema. Currently, the JSON schema is derived from table schema.

Supported Connectors
--------------------

-  Kafka
-  Upsert Kafka
-  Elasticsearch

Parameters
----------

.. table:: **Table 1**

   +--------------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                      | Mandatory   | Default Value | Type        | Description                                                                                                                                                                                                                                                                                                                  |
   +================================+=============+===============+=============+==============================================================================================================================================================================================================================================================================================================================+
   | format                         | Yes         | None          | String      | Format to be used. Set this parameter to **json**.                                                                                                                                                                                                                                                                           |
   +--------------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json.fail-on-missing-field     | No          | false         | Boolean     | Whether to skip the field or row or throws an error when a field to be parsed is missing. The default value is **false**, indicating that no error will be thrown.                                                                                                                                                           |
   +--------------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json.ignore-parse-errors       | No          | false         | Boolean     | Whether fields and rows with parse errors will be skipped or failed. The default value is **false**, indicating that an error will be thrown. Fields are set to null in case of errors.                                                                                                                                      |
   +--------------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json.timestamp-format.standard | No          | 'SQL'         | String      | Input and output timestamp format for TIMESTAMP and TIMESTAMP WITH LOCAL TIME ZONE.                                                                                                                                                                                                                                          |
   |                                |             |               |             |                                                                                                                                                                                                                                                                                                                              |
   |                                |             |               |             | Currently supported values are **SQL** and **ISO-8601**:                                                                                                                                                                                                                                                                     |
   |                                |             |               |             |                                                                                                                                                                                                                                                                                                                              |
   |                                |             |               |             | -  **SQL** will parse the input TIMESTAMP values in "yyyy-MM-dd HH:mm:ss.s{precision}" format, for example, **2020-12-30 12:13:14.123**, parse TIMESTAMP WITH LOCAL TIME ZONE values in "yyyy-MM-dd HH:mm:ss.s{precision}'Z'" format, for example, **2020-12-30 12:13:14.123Z** and output timestamp in the same format.     |
   |                                |             |               |             | -  **ISO-8601** will parse the input TIMESTAMP values in "yyyy-MM-ddTHH:mm:ss.s{precision}" format, for example, **2020-12-30T12:13:14.123** parse TIMESTAMP WITH LOCAL TIME ZONE values in "yyyy-MM-ddTHH:mm:ss.s{precision}'Z'" format, for example, **2020-12-30T12:13:14.123Z** and output timestamp in the same format. |
   +--------------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json.map-null-key.mode         | No          | 'FALL'        | String      | Handling mode when serializing null keys for map data. Available values are as follows:                                                                                                                                                                                                                                      |
   |                                |             |               |             |                                                                                                                                                                                                                                                                                                                              |
   |                                |             |               |             | -  **FAIL** will throw exception when encountering map value with null key.                                                                                                                                                                                                                                                  |
   |                                |             |               |             | -  **DROP** will drop null key entries for map data.                                                                                                                                                                                                                                                                         |
   |                                |             |               |             | -  **LITERAL** replaces the empty key value in the map with a string constant. The string literal is defined by **json.map-null-key.literal** option.                                                                                                                                                                        |
   +--------------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json.map-null-key.literal      | No          | 'null'        | String      | String literal to replace null key when **json.map-null-key.mode** is **LITERAL**.                                                                                                                                                                                                                                           |
   +--------------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

In this example, data is read from a topic and written to another using a Kafka sink.

#. Create a datasource connection for the communication with the VPC and subnet where Kafka locates and bind the connection to the queue. Set an inbound rule for the security group to allow access of the queue and test the connectivity using the Kafka address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job, select Flink 1.12, and allow DLI to save job logs in OBS. Use the following statement in the job and submit it:

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
        'topic' = '<yourSourceTopic>',
        'properties.bootstrap.servers' = '<yourKafkaAddress>:<yourKafkaPort>',
        'properties.group.id' = '<yourGroupId>',
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
        'topic' = '<yourSinkTopic>',
        'properties.bootstrap.servers' = '<yourKafkaAddress>:<yourKafkaPort>',
        "format" = "json"
      );

      insert into kafkaSink select * from kafkaSource;

#. Insert the following data into the source Kafka topic:

   .. code-block::

      {"order_id":"202103241000000001","order_channel":"webShop","order_time":"2021-03-24 10:00:00","pay_amount":100.0,"real_pay":100.0,"pay_time":"2021-03-24 10:02:03","user_id":"0001","user_name":"Alice","area_id":"330106"}

      {"order_id":"202103241606060001","order_channel":"appShop","order_time":"2021-03-24 16:06:06","pay_amount":200.0,"real_pay":180.0,"pay_time":"2021-03-24 16:10:06","user_id":"0001","user_name":"Alice","area_id":"330106"}

#. Read data from the sink topic. The result is as follows:

   .. code-block::

      {"order_id":"202103241000000001","order_channel":"webShop","order_time":"2021-03-24 10:00:00","pay_amount":100.0,"real_pay":100.0,"pay_time":"2021-03-24 10:02:03","user_id":"0001","user_name":"Alice","area_id":"330106"}

      {"order_id":"202103241606060001","order_channel":"appShop","order_time":"2021-03-24 16:06:06","pay_amount":200.0,"real_pay":180.0,"pay_time":"2021-03-24 16:10:06","user_id":"0001","user_name":"Alice","area_id":"330106"}
