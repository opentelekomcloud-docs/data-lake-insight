:original_name: dli_08_15021.html

.. _dli_08_15021:

JSON
====

Function
--------

The JSON format allows you to read and write JSON data based on a JSON schema. Currently, the JSON schema is derived from table schema. For details, see `JSON Format <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/formats/json/>`__.

Supported Connectors
--------------------

-  Kafka
-  Upsert Kafka
-  Elasticsearch

Parameters
----------

.. table:: **Table 1**

   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                           | Mandatory   | Default Value | Type        | Description                                                                                                                                                                                                                                                                                         |
   +=====================================+=============+===============+=============+=====================================================================================================================================================================================================================================================================================================+
   | format                              | Yes         | None          | String      | Format to be used. Set this parameter to **json**.                                                                                                                                                                                                                                                  |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json.fail-on-missing-field          | No          | false         | Boolean     | Whether missing fields and rows will be skipped or failed. The default value is **false**, indicating that an error will be thrown.                                                                                                                                                                 |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json.ignore-parse-errors            | No          | false         | Boolean     | Whether fields and rows with parse errors will be skipped or failed. The default value is **false**, indicating that an error will be thrown. Fields are set to null in case of errors.                                                                                                             |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json.timestamp-format.standard      | No          | 'SQL'         | String      | Specify the input and output timestamp format for **TIMESTAMP** and **TIMESTAMP_LTZ** type. Currently supported values are **SQL** and **ISO-8601**:                                                                                                                                                |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                                                     |
   |                                     |             |               |             | -  Option **SQL** will parse input **TIMESTAMP** values in "yyyy-MM-dd HH:mm:ss.s{precision}" format, e.g "2020-12-30 12:13:14.123", parse input **TIMESTAMP_LTZ** values in "yyyy-MM-dd HH:mm:ss.s{precision}'Z'" format, e.g "2020-12-30 12:13:14.123Z", and output timestamp in the same format. |
   |                                     |             |               |             | -  Option **ISO-8601** will parse input **TIMESTAMP** in "yyyy-MM-ddTHH:mm:ss.s{precision}" format, e.g "2020-12-30T12:13:14.123", parse input **TIMESTAMP_LTZ** in "yyyy-MM-ddTHH:mm:ss.s{precision}'Z'" format, e.g "2020-12-30T12:13:14.123Z", and output timestamp in the same format.          |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json.map-null-key.mode              | No          | 'FALL'        | String      | Handling mode when serializing null keys for map data. Available values are as follows:                                                                                                                                                                                                             |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                                                     |
   |                                     |             |               |             | -  **FAIL** will throw exception when encountering map value with null key.                                                                                                                                                                                                                         |
   |                                     |             |               |             | -  **DROP** will drop null key entries for map data.                                                                                                                                                                                                                                                |
   |                                     |             |               |             | -  **LITERAL** replaces the empty key value in the map with a string constant. The string literal is defined by **json.map-null-key.literal** option.                                                                                                                                               |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json.map-null-key.literal           | No          | 'null'        | String      | String literal to replace null key when **json.map-null-key.mode** is **LITERAL**.                                                                                                                                                                                                                  |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json.encode.decimal-as-plain-number | No          | false         | Boolean     | Encode all decimals as plain numbers instead of possible scientific notations. For example, **0.000000027** is encoded as **2.7E-8** by default, and will be written as **0.000000027** if set this parameter to **true**.                                                                          |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Data Type Mapping
-----------------

Currently, the JSON schema is always derived from table schema. Explicitly defining a JSON schema is not supported yet.

Flink JSON format uses `jackson databind API <https://github.com/FasterXML/jackson-databind>`__ to parse and generate JSON string.

The following table lists the type mapping from Flink type to JSON type.

.. table:: **Table 2** Data type mapping

   +--------------------------------+----------------------------------------------------+
   | Flink SQL Type                 | JSON Type                                          |
   +================================+====================================================+
   | CHAR/VARCHAR/STRING            | String                                             |
   +--------------------------------+----------------------------------------------------+
   | BOOLEAN                        | Boolean                                            |
   +--------------------------------+----------------------------------------------------+
   | BINARY/VARBINARY               | string with encoding: base64                       |
   +--------------------------------+----------------------------------------------------+
   | DECIMAL                        | Number                                             |
   +--------------------------------+----------------------------------------------------+
   | TINYINT                        | Number                                             |
   +--------------------------------+----------------------------------------------------+
   | SMALLINT                       | Number                                             |
   +--------------------------------+----------------------------------------------------+
   | INT                            | Number                                             |
   +--------------------------------+----------------------------------------------------+
   | BIGINT                         | Number                                             |
   +--------------------------------+----------------------------------------------------+
   | FLOAT                          | Number                                             |
   +--------------------------------+----------------------------------------------------+
   | DOUBLE                         | Number                                             |
   +--------------------------------+----------------------------------------------------+
   | DATE                           | string with format: date                           |
   +--------------------------------+----------------------------------------------------+
   | TIME                           | string with format: time                           |
   +--------------------------------+----------------------------------------------------+
   | TIMESTAMP                      | string with format: date-time                      |
   +--------------------------------+----------------------------------------------------+
   | TIMESTAMP_WITH_LOCAL_TIME_ZONE | string with format: date-time (with UTC time zone) |
   +--------------------------------+----------------------------------------------------+
   | INTERVAL                       | Number                                             |
   +--------------------------------+----------------------------------------------------+
   | ARRAY                          | array                                              |
   +--------------------------------+----------------------------------------------------+
   | MAP / MULTISET                 | object                                             |
   +--------------------------------+----------------------------------------------------+
   | ROW                            | object                                             |
   +--------------------------------+----------------------------------------------------+

Example
-------

In this example, data is read from a topic and written to another using a Kafka sink.

#. Create a datasource connection for the communication with the VPC and subnet where Kafka locates and bind the connection to the queue. Set an inbound rule for the security group to allow access of the queue and test the connectivity using the Kafka address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job, select Flink 1.15, and allow DLI to save job logs in OBS. Use the following statement in the job and submit it:

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
        'topic' = 'kafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );

      CREATE TABLE printSink (
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
      insert into printSink select * from kafkaSource;

#. Insert the following data into the source Kafka topic:

   .. code-block::

      {"order_id":"202103241000000001","order_channel":"webShop","order_time":"2021-03-24 10:00:00","pay_amount":100.0,"real_pay":100.0,"pay_time":"2021-03-24 10:02:03","user_id":"0001","user_name":"Alice","area_id":"330106"}

      {"order_id":"202103241606060001","order_channel":"appShop","order_time":"2021-03-24 16:06:06","pay_amount":200.0,"real_pay":180.0,"pay_time":"2021-03-24 16:10:06","user_id":"0001","user_name":"Alice","area_id":"330106"}

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.

   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.

   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **.out** file, and view result logs.

      .. code-block::

         +I[202103241000000001, webShop, 2021-03-24 10:00:00, 100.0, 100.0, 2021-03-24 10:02:03, 0001, Alice, 330106]
         +I[202103241606060001, appShop11, 2021-03-24 16:06:06, 200.0, 180.0, 2021-03-24 16:10:06, 0001, Alice, 330106]
