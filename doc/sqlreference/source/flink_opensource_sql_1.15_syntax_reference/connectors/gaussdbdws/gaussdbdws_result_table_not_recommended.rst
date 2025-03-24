:original_name: dli_08_15105.html

.. _dli_08_15105:

GaussDB(DWS) Result Table (Not Recommended)
===========================================

Function
--------

DLI outputs the Flink job output data to GaussDB(DWS). GaussDB(DWS) database kernel is compliant with PostgreSQL. The PostgreSQL database can store data of more complex types and deliver space information services, multi-version concurrent control (MVCC), and high concurrency. It applies to location applications, financial insurance, and e-Commerce.

GaussDB(DWS) is an online data processing database based on the cloud infrastructure and platform and helps you mine and analyze massive sets of data.

.. note::

   You are advised to use GaussDB(DWS) self-developed GaussDB(DWS) connector.

Prerequisites
-------------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  You have created a GaussDB(DWS) cluster. For details about how to create a GaussDB(DWS) cluster, see **Creating a Cluster** in the *Data Warehouse Service Management Guide*.
-  You have created a GaussDB(DWS) database table.
-  An enhanced datasource connection has been created for DLI to connect to GaussDB(DWS) clusters, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

Precautions
-----------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .
-  Fields in the **with** parameter can only be enclosed in single quotes.

-  To use the upsert mode, you must define the primary key for both the GaussDB(DWS) result table and the GaussDB(DWS) table connected to the result table.

-  If tables with the same name exist in different GaussDB(DWS) schemas, you need to specify the schemas in the Flink open source SQL statements.

-  If you use the gsjdbc4 driver for connection, set **driver** to **org.postgresql.Driver**. You can omit this parameter because the gsjdbc4 driver is the default one.

   For example, run the following statements to use the gsjdbc4 driver to write data to GaussDB(DWS) in upsert mode:

   ::

      create table dwsSink(
        car_id STRING,
        car_owner STRING,
        car_brand STRING,
        car_speed INT
      ) with (
        'connector' = 'gaussdb',
        'url' = 'jdbc:postgresql://DwsAddress:DwsPort/DwsDatabase',
        'table-name' = 'car_info',
        'username' = 'DwsUserName',
        'password' = 'DwsPasswrod',
        'write.mode' = 'upsert'
      );

Syntax
------

.. note::

   Do not set all attributes in a GaussDB(DWS) result table to **PRIMARY KEY**.

::

   create table dwsSink (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
     'connector' = 'gaussdb',
     'url' = '',
     'table-name' = '',
     'driver' = '',
     'username' = '',
     'password' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------------+-------------+-----------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory   | Default Value         | Data Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                        |
   +============================+=============+=======================+=============+====================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | connector                  | Yes         | None                  | String      | Connector to be used. Set this parameter to **gaussdb**.                                                                                                                                                                                                                                                                                                                                                                           |
   +----------------------------+-------------+-----------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | url                        | Yes         | None                  | String      | JDBC connection address.                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                            |             |                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             | If you use the gsjdbc4 driver, set the value in jdbc:postgresql://${ip}:${port}/${dbName} format.                                                                                                                                                                                                                                                                                                                                  |
   |                            |             |                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             | If you use the gsjdbc200 driver, set the value in jdbc:gaussdb://${ip}:${port}/${dbName} format.                                                                                                                                                                                                                                                                                                                                   |
   +----------------------------+-------------+-----------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table-name                 | Yes         | None                  | String      | Name of the table to be operated. If the GaussDB(DWS) table is in a schema, the format is **schema\\".\\"**\ *Table name*. For details, see :ref:`FAQ <dli_08_15105__en-us_topic_0000001310095785_section92924310151>`.                                                                                                                                                                                                            |
   +----------------------------+-------------+-----------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | driver                     | No          | org.postgresql.Driver | String      | JDBC connection driver. The default value is **org.postgresql.Driver**.                                                                                                                                                                                                                                                                                                                                                            |
   +----------------------------+-------------+-----------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username                   | No          | None                  | String      | Username for GaussDB(DWS) database authentication. This parameter must be configured in pair with **password**.                                                                                                                                                                                                                                                                                                                    |
   +----------------------------+-------------+-----------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                   | No          | None                  | String      | Password for GaussDB(DWS) database authentication. This parameter must be configured in pair with **username**.                                                                                                                                                                                                                                                                                                                    |
   +----------------------------+-------------+-----------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | write.mode                 | No          | None                  | String      | Data write mode. The value can be **copy**, **insert**, or **upsert**. The default value is **upsert**.                                                                                                                                                                                                                                                                                                                            |
   |                            |             |                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             | This parameter must be configured depending on **primary key**.                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             | -  If **primary key** is not configured, data can be appended in **copy** and **insert** modes.                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             | -  If **primary key** is configured, all the three modes are available.                                                                                                                                                                                                                                                                                                                                                            |
   |                            |             |                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             | Note: GaussDB(DWS) does not support the update of distribution columns. The primary keys of columns to be updated must cover all distribution columns defined in the GaussDB(DWS) table.                                                                                                                                                                                                                                           |
   +----------------------------+-------------+-----------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.buffer-flush.max-rows | No          | 100                   | Integer     | Maximum number of rows to buffer for each write request.                                                                                                                                                                                                                                                                                                                                                                           |
   |                            |             |                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             | It can improve the performance of writing data, but may increase the latency.                                                                                                                                                                                                                                                                                                                                                      |
   |                            |             |                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             | You can set this parameter to **0** to disable it.                                                                                                                                                                                                                                                                                                                                                                                 |
   +----------------------------+-------------+-----------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.buffer-flush.interval | No          | 1s                    | Duration    | Interval for refreshing the buffer, during which data is refreshed by asynchronous threads.                                                                                                                                                                                                                                                                                                                                        |
   |                            |             |                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             | It can improve the performance of writing data to the database, but may increase the latency.                                                                                                                                                                                                                                                                                                                                      |
   |                            |             |                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             | You can set this parameter to **0** to disable it.                                                                                                                                                                                                                                                                                                                                                                                 |
   |                            |             |                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             | Note: If **sink.buffer-flush.max-size** and **sink.buffer-flush.max-rows** are both set to **0** and the buffer refresh interval is configured, the buffer is asynchronously refreshed.                                                                                                                                                                                                                                            |
   |                            |             |                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             | The format is {length value}{time unit label}, for example, **123ms, 321s**. The supported time units include d, h, min, s, and ms (default unit).                                                                                                                                                                                                                                                                                 |
   +----------------------------+-------------+-----------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.max-retries           | No          | 3                     | Integer     | Maximum number of write retries.                                                                                                                                                                                                                                                                                                                                                                                                   |
   +----------------------------+-------------+-----------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | write.escape-string-value  | No          | false                 | Boolean     | Whether to escape values of the string type. This parameter is used only when **write.mode** is set to **copy**.                                                                                                                                                                                                                                                                                                                   |
   +----------------------------+-------------+-----------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key-by-before-sink         | No          | false                 | Boolean     | Whether to partition by the specified primary key before the sink operator                                                                                                                                                                                                                                                                                                                                                         |
   |                            |             |                       |             |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |                       |             | This parameter aims to solve the problem of interlocking between two subtasks when they acquire row locks based on the primary key from GaussDB(DWS), multiple concurrent writes occur, and **write.mode** is **upsert**. This happens when a batch of data written to the sink by multiple subtasks has more than one record with the same primary key, and the order of these records with the same primary key is inconsistent. |
   +----------------------------+-------------+-----------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

In this example, data is read from the Kafka data source and written to the GaussDB(DWS) result table in insert mode. The procedure is as follows:

#. Create an enhanced datasource connection in the VPC and subnet where GaussDB(DWS) and Kafka locate, and bind the connection to the required Flink elastic resource pool.

#. Set GaussDB(DWS) and Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the GaussDB(DWS) and Kafka address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Connect to the GaussDB(DWS) database and create a table named **dws_order**.

   .. code-block::

      create table public.dws_order(
        order_id VARCHAR,
        order_channel VARCHAR,
        order_time VARCHAR,
        pay_amount FLOAT8,
        real_pay FLOAT8,
        pay_time VARCHAR,
        user_id VARCHAR,
        user_name VARCHAR,
        area_id VARCHAR);

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job. The job script uses the Kafka data source and the GaussDB(DWS) result table.

   When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

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
        'topic' = 'KafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );

      CREATE TABLE dwsSink (
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
        'connector' = 'gaussdb',
        'url' = 'jdbc:postgresql://DWSAddress:DWSPort/DWSdbName',
        'table-name' = 'dws_order',
        'driver' = 'org.postgresql.Driver',
        'username' = 'DWSUserName',
        'password' = 'DWSPassword',
        'write.mode' = 'insert'
      );

      insert into dwsSink select * from kafkaSource;

#. Connect to the Kafka cluster and enter the following test data to Kafka:

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

#. Run the following SQL statement in GaussDB(DWS) to view the data result:

   .. code-block::

       select * from dws_order

   The data result is as follows:

   .. code-block::

      202103241000000001    webShop 2021-03-24 10:00:00 100.0   100.0   2021-03-24 10:02:03 0001    Alice   330106

.. _dli_08_15105__en-us_topic_0000001310095785_section92924310151:

FAQ
---

-  Q: What should I do if the Flink job execution fails and the log contains the following error information?

   .. code-block::

      java.io.IOException: unable to open JDBC writer
      ...
      Caused by: org.postgresql.util.PSQLException: The connection attempt failed.
      ...
      Caused by: java.net.SocketTimeoutException: connect timed out

   A: The datasource connection is not bound or the binding fails.

-  Q: How can I configure a GaussDB(DWS) table that is in a schema?

   A: When GaussDB(DWS) table **test** is in schema **ads_game_sdk_base**, refer to the **'table-name'** parameter setting in the following example:

   .. code-block::

      CREATE TABLE ads_rpt_game_sdk_realtime_ada_reg_user_pay_mm (
        ddate DATE,
        dmin TIMESTAMP(3),
        game_appkey VARCHAR,
        channel_id VARCHAR,
        pay_user_num_1m bigint,
        pay_amt_1m bigint,
        PRIMARY KEY (ddate, dmin, game_appkey, channel_id) NOT ENFORCED
      ) WITH (
        'connector' = 'gaussdb',
        'url' = 'jdbc:postgresql://<yourDwsAddress>:<yourDwsPort>/dws_bigdata_db',
        'table-name' = 'ads_game_sdk_base.test',
        'username' = '<yourUsername>',
        'password' = '<yourPassword>',
        'write.mode' = 'upsert'
      );

-  Q: What can I do if a job is running properly but there is no data in GaussDB(DWS)?

   A: Check the following items:

   -  Check whether the JobManager and TaskManager logs contain error information. To view logs, perform the following steps:

      #. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
      #. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
      #. Go to the folder of the date, find the folder whose name contains **taskmanager** or **jobmanager**, download the **taskmanager.out** or **jobmanager.out** file, and view result logs.

   -  Check whether the datasource connection is correctly bound and whether a security group rule allows access of the queue.
   -  Check whether the GaussDB(DWS) table to which data is to be written exists in multiple schemas. If it does, specify the schemas in the Flink job.
