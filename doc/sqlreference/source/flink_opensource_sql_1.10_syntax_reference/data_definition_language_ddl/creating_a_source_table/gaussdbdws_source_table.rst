:original_name: dli_08_0304.html

.. _dli_08_0304:

GaussDB(DWS) Source Table
=========================

Function
--------

DLI reads data of Flink jobs from GaussDB(DWS). GaussDB(DWS) database kernel is compliant with PostgreSQL. The PostgreSQL database can store data of more complex types and delivers space information services, multi-version concurrent control (MVCC), and high concurrency. It applies to location applications, financial insurance, and e-commerce.

GaussDB(DWS) is an online data processing database based on the cloud infrastructure and platform and helps you mine and analyze massive sets of data.

Prerequisites
-------------

-  Ensure that you have created a GaussDB(DWS) cluster using your account.

   For details about how to create a GaussDB(DWS) cluster, see "Creating a Cluster" in *Data Warehouse Service Management Guide*.

-  A GaussDB(DWS) database table has been created.

-  An enhanced datasource connection has been created for DLI to connect to GaussDB(DWS) clusters, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

Syntax
------

::

   create table dwsSource (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
     (',' watermark for rowtime_column_name as watermark-strategy_expression)
   )
   with (
     'connector.type' = 'gaussdb',
     'connector.url' = '',
     'connector.table' = '',
     'connector.username' = '',
     'connector.password' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +--------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                            | Mandatory             | Description                                                                                                                                                                                                                                                                                                            |
   +======================================+=======================+========================================================================================================================================================================================================================================================================================================================+
   | connector.type                       | Yes                   | Connector type. Set this parameter to **gaussdb**.                                                                                                                                                                                                                                                                     |
   +--------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.url                        | Yes                   | JDBC connection address. The format is jdbc:postgresql://${ip}:${port}/${dbName}. If the database version is later than 8.1.0, the value format is jdbc:gaussdb://${ip}:${port}/${dbName}.                                                                                                                             |
   +--------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.table                      | Yes                   | Name of the table to be operated. If the GaussDB(DWS) table is in a schema, the format is **schema\\".\\"**\ *Table name*. For details, see the :ref:`Example <dli_08_0304__en-us_topic_0000001119232080_en-us_topic_0000001127915811_dli_08_0277_en-us_topic_0113887276_en-us_topic_0111499973_section444391525220>`. |
   +--------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.driver                     | No                    | JDBC connection driver. The default value is **org.postgresql.Driver**.                                                                                                                                                                                                                                                |
   +--------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.username                   | No                    | Database authentication user name. This parameter must be configured in pair with **connector.password**.                                                                                                                                                                                                              |
   +--------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.password                   | No                    | Database authentication password. This parameter must be configured in pair with **connector.username**.                                                                                                                                                                                                               |
   +--------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.partition.column      | No                    | Name of the column used to partition the input                                                                                                                                                                                                                                                                         |
   |                                      |                       |                                                                                                                                                                                                                                                                                                                        |
   |                                      |                       | This parameter is mandatory if **connector.read.partition.lower-bound**, **connector.read.partition.upper-bound**, and                                                                                                                                                                                                 |
   |                                      |                       |                                                                                                                                                                                                                                                                                                                        |
   |                                      |                       | **connector.read.partition.num** are configured.                                                                                                                                                                                                                                                                       |
   +--------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.partition.lower-bound | No                    | Lower bound of values to be fetched for the first partition                                                                                                                                                                                                                                                            |
   |                                      |                       |                                                                                                                                                                                                                                                                                                                        |
   |                                      |                       | This parameter is mandatory if **connector.read.partition.column**, **connector.read.partition.upper-bound**, and                                                                                                                                                                                                      |
   |                                      |                       |                                                                                                                                                                                                                                                                                                                        |
   |                                      |                       | **connector.read.partition.num** are configured.                                                                                                                                                                                                                                                                       |
   +--------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.partition.upper-bound | No                    | Upper bound of values to be fetched for the last partition                                                                                                                                                                                                                                                             |
   |                                      |                       |                                                                                                                                                                                                                                                                                                                        |
   |                                      |                       | This parameter is mandatory if **connector.read.partition.column**, **connector.read.partition.lower-bound**, and                                                                                                                                                                                                      |
   |                                      |                       |                                                                                                                                                                                                                                                                                                                        |
   |                                      |                       | **connector.read.partition.num** are configured.                                                                                                                                                                                                                                                                       |
   +--------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.partition.num         | No                    | Number of partitions to be created                                                                                                                                                                                                                                                                                     |
   |                                      |                       |                                                                                                                                                                                                                                                                                                                        |
   |                                      |                       | This parameter is mandatory if **connector.read.partition.column**, **connector.read.partition.upper-bound**, and                                                                                                                                                                                                      |
   |                                      |                       |                                                                                                                                                                                                                                                                                                                        |
   |                                      |                       | **connector.read.partition.upper-bound** are configured.                                                                                                                                                                                                                                                               |
   +--------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.fetch-size            | No                    | Number of rows fetched from the database each time The default value is **0**, indicating the hint is ignored.                                                                                                                                                                                                         |
   +--------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_0304__en-us_topic_0000001119232080_en-us_topic_0000001127915811_dli_08_0277_en-us_topic_0113887276_en-us_topic_0111499973_section444391525220:

Example
-------

-  If you use the gsjdbc4 driver for connection, set **connector.driver** to **org.postgresql.Driver**. You can omit this parameter because the gsjdbc4 driver is the default one.

   Create table **dwsSource** with data fetched from the **car_info** table that is not in a schema:

   ::

      create table dwsSource(
        car_id STRING,
        car_owner STRING,
        car_brand STRING,
        car_speed INT
      ) with (
        'connector.type' = 'gaussdb',
        'connector.url' = 'jdbc:postgresql://xx.xx.xx.xx:8000/xx',
        'connector.table' = 'car_info',
        'connector.username' = 'xx',
        'connector.password' = 'xx'
      );

   Create table **dwsSource** with data fetched from GaussDB(DWS) table **test** that is in a schema named **test_schema**:

   ::

      create table dwsSource(
        car_id STRING,
        car_owner STRING,
        car_brand STRING,
        car_speed INT
      ) with (
        'connector.type' = 'gaussdb',
        'connector.url' = 'jdbc:postgresql://xx.xx.xx.xx:8000/xx',
        'connector.table' = 'test_schema\".\"test',
        'connector.username' = 'xx',
        'connector.password' = 'xx'
      );
