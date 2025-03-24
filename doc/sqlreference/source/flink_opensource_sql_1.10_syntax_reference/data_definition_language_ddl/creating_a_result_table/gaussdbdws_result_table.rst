:original_name: dli_08_0312.html

.. _dli_08_0312:

GaussDB(DWS) Result Table
=========================

Function
--------

DLI outputs the Flink job output data to GaussDB(DWS). GaussDB(DWS) database kernel is compliant with PostgreSQL. The PostgreSQL database can store data of more complex types and delivers space information services, multi-version concurrent control (MVCC), and high concurrency. It applies to location applications, financial insurance, and e-commerce.

GaussDB(DWS) is an online data processing database based on the cloud infrastructure and platform and helps you mine and analyze massive sets of data.

Prerequisites
-------------

-  Ensure that you have created a GaussDB(DWS) cluster using your account.

   For details about how to create a GaussDB(DWS) cluster, see "Creating a Cluster" in *Data Warehouse Service Management Guide*.

-  A GaussDB(DWS) database table has been created.

-  An enhanced datasource connection has been created for DLI to connect to GaussDB(DWS) clusters, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

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
     'connector.type' = 'gaussdb',
     'connector.url' = '',
     'connector.table' = '',
     'connector.driver' = '',
     'connector.username' = '',
     'connector.password' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +-------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                           | Mandatory             | Description                                                                                                                                                                                                                                                                                          |
   +=====================================+=======================+======================================================================================================================================================================================================================================================================================================+
   | connector.type                      | Yes                   | Connector type. Set this parameter to **gaussdb**.                                                                                                                                                                                                                                                   |
   +-------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.url                       | Yes                   | JDBC connection address. The format is jdbc:postgresql://${ip}:${port}/${dbName}.                                                                                                                                                                                                                    |
   +-------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.table                     | Yes                   | Name of the table to be operated. If the GaussDB(DWS) table is in a schema, the format is **schema\\".\\"**\ *Table name*. For details, see the :ref:`Example <dli_08_0312__en-us_topic_0000001119232084_en-us_topic_0000001127915813_dli_08_0252_section085022413338>`.                             |
   +-------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.driver                    | No                    | JDBC connection driver. The default value is **org.postgresql.Driver**.                                                                                                                                                                                                                              |
   +-------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.username                  | No                    | Database authentication user name. This parameter must be configured in pair with **connector.password**.                                                                                                                                                                                            |
   +-------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.password                  | No                    | Database authentication password. This parameter must be configured in pair with **connector.username**.                                                                                                                                                                                             |
   +-------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.mode                | No                    | Data write mode. The value can be **copy**, **insert**, or **upsert**. The default value is **upsert**.                                                                                                                                                                                              |
   |                                     |                       |                                                                                                                                                                                                                                                                                                      |
   |                                     |                       | This parameter must be configured depending on **primary key**.                                                                                                                                                                                                                                      |
   |                                     |                       |                                                                                                                                                                                                                                                                                                      |
   |                                     |                       | -  If **primary key** is not configured, data can be appended in **copy** and **insert** modes.                                                                                                                                                                                                      |
   |                                     |                       | -  If **primary key** is configured, all the three modes are available.                                                                                                                                                                                                                              |
   |                                     |                       |                                                                                                                                                                                                                                                                                                      |
   |                                     |                       | Note: GaussDB(DWS) does not support the update of distribution columns. The primary keys of columns to be updated must cover all distribution columns defined in the GaussDB(DWS) table.                                                                                                             |
   +-------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.flush.max-rows      | No                    | Maximum rows allowed for data flush. If the data size exceeds the value, data flush is triggered. The default value is **5000**.                                                                                                                                                                     |
   +-------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.flush.interval      | No                    | Data flush period. Data flush is triggered periodically. The format is {length value}{time unit label}, for example, **123ms, 321s**. The supported time units include d, h, min, s, and ms (default unit). If this parameter is not set, the value is not updated based on the interval by default. |
   +-------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.max-retries         | No                    | Maximum number of attempts to write data. The default value is **3**.                                                                                                                                                                                                                                |
   +-------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.merge.filter-key    | No                    | Column to be merged. This parameter takes effects only when PRIMARY KEY is configured and **connector.write.mode** is set to **copy**.                                                                                                                                                               |
   +-------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.escape-string-value | No                    | Whether to escape values of the string type. The default value is **false**.                                                                                                                                                                                                                         |
   +-------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

None

.. _dli_08_0312__en-us_topic_0000001119232084_en-us_topic_0000001127915813_dli_08_0252_section085022413338:

Example
-------

-  If you use the gsjdbc4 driver for connection, set **connector.driver** to **org.postgresql.Driver**. You can omit this parameter because the gsjdbc4 driver is the default one.

   -  Write data to GaussDB(DWS) in **upsert** mode.

      ::

         create table dwsSink(
           car_id STRING,
           car_owner STRING,
           car_brand STRING,
           car_speed INT
         ) with (
           'connector.type' = 'gaussdb',
           'connector.url' = 'jdbc:postgresql://xx.xx.xx.xx:8000/xx',
           'connector.table' = 'car_info',
           'connector.username' = 'xx',
           'connector.password' = 'xx',
           'connector.write.mode' = 'upsert',
           'connector.write.flush.interval' = '30s'
         );

      Create table **dwsSource** with data fetched from GaussDB(DWS) table **test** that is in a schema named **ads_game_sdk_base**:

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
           'connector.type' = 'gaussdb',
           'connector.url' = 'jdbc:postgresql://xx.xx.xx.xx:8000/dws_bigdata_db',
           'connector.table' = 'ads_game_sdk_base\".\"test',
           'connector.username' = 'xxxx',
           'connector.password' = 'xxxxx',
           'connector.write.mode' = 'upsert',
           'connector.write.flush.interval' = '30s'
         );
