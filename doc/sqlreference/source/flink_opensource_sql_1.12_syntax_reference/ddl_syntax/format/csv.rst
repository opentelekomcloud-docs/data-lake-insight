:original_name: dli_08_0411.html

.. _dli_08_0411:

CSV
===

Function
--------

The CSV format allows you to read and write CSV data based on a CSV schema. Currently, the CSV schema is derived from table schema.

Supported Connectors
--------------------

-  Kafka
-  Upsert Kafka

Parameters
----------

.. table:: **Table 1**

   +-----------------------------+-----------+---------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                   | Mandatory | Default value | Type    | Description                                                                                                                                                                                                                                                                                                   |
   +=============================+===========+===============+=========+===============================================================================================================================================================================================================================================================================================================+
   | format                      | Yes       | None          | String  | Format to be used. Set the value to **csv**.                                                                                                                                                                                                                                                                  |
   +-----------------------------+-----------+---------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.field-delimiter         | No        | ,             | String  | Field delimiter character, which must be a single character. You can use backslash to specify special characters, for example, **\\t** represents the tab character. You can also use unicode to specify them in plain SQL, for example, **'csv.field-delimiter' = '\\u0001'** represents the 0x01 character. |
   +-----------------------------+-----------+---------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.disable-quote-character | No        | false         | Boolean | Disabled quote character for enclosing field values. If you set this parameter to **true**, **csv.quote-character** cannot be set.                                                                                                                                                                            |
   +-----------------------------+-----------+---------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.quote-character         | No        | ''            | String  | Quote character for enclosing field values.                                                                                                                                                                                                                                                                   |
   +-----------------------------+-----------+---------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.allow-comments          | No        | false         | Boolean | Ignore comment lines that start with **#**. If you set this parameter to **true**, make sure to also ignore parse errors to allow empty rows.                                                                                                                                                                 |
   +-----------------------------+-----------+---------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.ignore-parse-errors     | No        | false         | Boolean | Whether fields and rows with parse errors will be skipped or failed. The default value is **false**, indicating that an error will be thrown. Fields are set to null in case of errors.                                                                                                                       |
   +-----------------------------+-----------+---------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.array-element-delimiter | No        | ;             | String  | Array element delimiter string for separating array and row element values.                                                                                                                                                                                                                                   |
   +-----------------------------+-----------+---------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.escape-character        | No        | None          | String  | Escape character for escaping values                                                                                                                                                                                                                                                                          |
   +-----------------------------+-----------+---------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.null-literal            | No        | None          | String  | Null literal string that is interpreted as a null value.                                                                                                                                                                                                                                                      |
   +-----------------------------+-----------+---------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Use Kafka to send data and output the data to print.

#. Create a datasource connection for the communication with the VPC and subnet where Kafka locates and bind the connection to the queue. Set a security group and inbound rule to allow access of the queue and test the connectivity of the queue using the Kafka IP address. For example, locate a general-purpose queue where the job runs and choose **More** > **Test Address Connectivity** in the **Operation** column. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job. Copy the following statement and submit the job:

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
        "format" = "csv"
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
        "format" = "csv"
      );

      insert into kafkaSink select * from kafkaSource;

#. Insert the following data into the source Kafka topic:

   .. code-block::

      202103251505050001,qqShop,2021-03-25 15:05:05,500.00,400.00,2021-03-25 15:10:00,0003,Cindy,330108

      202103241606060001,appShop,2021-03-24 16:06:06,200.00,180.00,2021-03-24 16:10:06,0001,Alice,330106

#. Read data from the sink Kafka topic. The result is as follows:

   .. code-block::

      202103251505050001,qqShop,"2021-03-25 15:05:05",500.0,400.0,"2021-03-25 15:10:00",0003,Cindy,330108

      202103241606060001,appShop,"2021-03-24 16:06:06",200.0,180.0,"2021-03-24 16:10:06",0001,Alice,330106
