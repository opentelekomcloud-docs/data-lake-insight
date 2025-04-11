:original_name: dli_08_15019.html

.. _dli_08_15019:

CSV
===

Function
--------

The CSV format allows you to read and write CSV data based on a CSV schema. Currently, the CSV schema is derived from table schema. For details, see `CSV Format <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/formats/csv/>`__.

Supported Connectors
--------------------

-  Kafka
-  Upsert Kafka
-  FileSystem

Parameters
----------

.. table:: **Table 1** Description

   +-----------------------------+-----------+---------------+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                   | Mandatory | Default value | Type    | Description                                                                                                                                                                                                                                                                                                    |
   +=============================+===========+===============+=========+================================================================================================================================================================================================================================================================================================================+
   | format                      | Yes       | None          | String  | Format to be used. Set the value to **csv**.                                                                                                                                                                                                                                                                   |
   +-----------------------------+-----------+---------------+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.field-delimiter         | No        | ,             | String  | Field delimiter character, which must be a single character. You can use backslash to specify special characters, for example, **\\t** represents the tab character. You can also use unicode to specify them in plain SQL, for example, **'csv.field-delimiter' = U&'\\0001'** represents the 0x01 character. |
   +-----------------------------+-----------+---------------+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.disable-quote-character | No        | false         | Boolean | Disabled quote character for enclosing field values. If you set this parameter to **true**, **csv.quote-character** cannot be set.                                                                                                                                                                             |
   +-----------------------------+-----------+---------------+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.quote-character         | No        | ''            | String  | Quote character for enclosing field values.                                                                                                                                                                                                                                                                    |
   +-----------------------------+-----------+---------------+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.allow-comments          | No        | false         | Boolean | Ignore comment lines that start with **#**. If you set this parameter to **true**, make sure to also ignore parse errors to allow empty rows.                                                                                                                                                                  |
   +-----------------------------+-----------+---------------+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.ignore-parse-errors     | No        | false         | Boolean | Whether fields and rows with parse errors will be skipped or failed. The default value is **false**, indicating that an error will be thrown. Fields are set to null in case of errors.                                                                                                                        |
   +-----------------------------+-----------+---------------+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.array-element-delimiter | No        | ;             | String  | Array element delimiter string for separating array and row element values.                                                                                                                                                                                                                                    |
   +-----------------------------+-----------+---------------+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.escape-character        | No        | None          | String  | Escape character for escaping values                                                                                                                                                                                                                                                                           |
   +-----------------------------+-----------+---------------+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | csv.null-literal            | No        | None          | String  | Null literal string that is interpreted as a null value.                                                                                                                                                                                                                                                       |
   +-----------------------------+-----------+---------------+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Data Type Mapping
-----------------

Currently, the CSV schema is always derived from table schema. Explicitly defining a CSV schema is not supported yet. Flink CSV format uses `jackson databind API <https://github.com/FasterXML/jackson-databind>`__ to parse and generate CSV string.

.. table:: **Table 2** Data type mapping

   =================== =============================
   Flink SQL Type      CSV Type
   =================== =============================
   CHAR/VARCHAR/STRING String
   BOOLEAN             Boolean
   BINARY/VARBINARY    string with encoding: base64
   DECIMAL             Number
   TINYINT             Number
   SMALLINT            Number
   INT                 Number
   BIGINT              Number
   FLOAT               Number
   DOUBLE              Number
   DATE                string with format: date
   TIME                string with format: time
   TIMESTAMP           string with format: date-time
   INTERVAL            Number
   ARRAY               array
   ROW                 object
   =================== =============================

Example
-------

Use Kafka to send data and output the data to Print.

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
        'topic' = 'kafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'csv'
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

      202103251505050001,appShop,2021-03-25 15:05:05,500.00,400.00,2021-03-25 15:10:00,0003,Cindy,330108

      202103241606060001,appShop,2021-03-24 16:06:06,200.00,180.00,2021-03-24 16:10:06,0001,Alice,330106

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **.out** file, and view result logs.

   .. code-block::

      +I[202103251505050001, appShop, 2021-03-25 15:05:05, 500.0, 400.0, 2021-03-25 15:10:00, 0003, Cindy, 330108]
      +I[202103241606060001, appShop, 2021-03-24 16:06:06, 200.0, 180.0, 2021-03-24 16:10:06, 0001, Alice, 330106]
