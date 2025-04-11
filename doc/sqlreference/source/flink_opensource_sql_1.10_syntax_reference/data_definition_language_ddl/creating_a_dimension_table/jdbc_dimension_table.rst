:original_name: dli_08_0318.html

.. _dli_08_0318:

JDBC Dimension Table
====================

Create a JDBC dimension table to connect to the source stream.

Prerequisites
-------------

-  You have created a JDBC instance for your account.

Syntax
------

::

   CREATE TABLE  table_id (
     attr_name attr_type
     (',' attr_name attr_type)*
   )
     WITH (
     'connector.type' = 'jdbc',
     'connector.url' = '',
     'connector.table' = '',
     'connector.username' = '',
     'connector.password' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                            | Mandatory             | Description                                                                                                                                                                                                                               |
   +======================================+=======================+===========================================================================================================================================================================================================================================+
   | connector.type                       | Yes                   | Data source type. Set this parameter to **jdbc**.                                                                                                                                                                                         |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.url                        | Yes                   | Database URL                                                                                                                                                                                                                              |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.table                      | Yes                   | Name of the table where the data to be read from the database is located                                                                                                                                                                  |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.driver                     | No                    | Driver required for connecting to the database If you do not set this parameter, the automatically extracted URL will be used.                                                                                                            |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.username                   | No                    | Database authentication user name. This parameter must be configured in pair with **connector.password**.                                                                                                                                 |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.password                   | No                    | Database authentication password. This parameter must be configured in pair with **connector.username**.                                                                                                                                  |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.partition.column      | No                    | Name of the column used to partition the input                                                                                                                                                                                            |
   |                                      |                       |                                                                                                                                                                                                                                           |
   |                                      |                       | This parameter is mandatory if **connector.read.partition.lower-bound**, **connector.read.partition.upper-bound**, and                                                                                                                    |
   |                                      |                       |                                                                                                                                                                                                                                           |
   |                                      |                       | **connector.read.partition.num** are configured.                                                                                                                                                                                          |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.partition.lower-bound | No                    | Lower bound of values to be fetched for the first partition                                                                                                                                                                               |
   |                                      |                       |                                                                                                                                                                                                                                           |
   |                                      |                       | This parameter is mandatory if **connector.read.partition.column**, **connector.read.partition.upper-bound**, and                                                                                                                         |
   |                                      |                       |                                                                                                                                                                                                                                           |
   |                                      |                       | **connector.read.partition.num** are configured.                                                                                                                                                                                          |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.partition.upper-bound | No                    | Upper bound of values to be fetched for the last partition                                                                                                                                                                                |
   |                                      |                       |                                                                                                                                                                                                                                           |
   |                                      |                       | This parameter is mandatory if **connector.read.partition.column**, **connector.read.partition.lower-bound**, and                                                                                                                         |
   |                                      |                       |                                                                                                                                                                                                                                           |
   |                                      |                       | **connector.read.partition.num** are configured.                                                                                                                                                                                          |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.partition.num         | No                    | Number of partitions                                                                                                                                                                                                                      |
   |                                      |                       |                                                                                                                                                                                                                                           |
   |                                      |                       | This parameter is mandatory if **connector.read.partition.column**, **connector.read.partition.upper-bound**, and                                                                                                                         |
   |                                      |                       |                                                                                                                                                                                                                                           |
   |                                      |                       | **connector.read.partition.upper-bound** are configured.                                                                                                                                                                                  |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.fetch-size            | No                    | Number of rows fetched from the database each time. The default value is **0**, indicating the hint is ignored.                                                                                                                           |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.lookup.cache.max-rows      | No                    | Maximum number of cached rows in a dimension table. When the rows exceed this value, the data that is added first will be marked as expired. The value **-1** indicates that data cache disabled.                                         |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.lookup.cache.ttl           | No                    | Time To Live (TTL) of dimension table cache. Caches exceeding the TTL will be deleted. The format is {length value}{time unit label}, for example, **123ms, 321s**. The supported time units include d, h, min, s, and ms (default unit). |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.lookup.max-retries         | No                    | Maximum number of attempts to obtain data from the dimension table. The default value is **3**.                                                                                                                                           |
   +--------------------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

The RDS table is used to connect to the source stream.

::

   CREATE TABLE car_infos (
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_price INT,
     proctime as PROCTIME()
   )
     WITH (
     'connector.type' = 'dis',
     'connector.region' = '',
     'connector.channel' = 'disInput',
     'format.type' = 'csv'
     );

   CREATE TABLE  db_info (
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_price INT
   )
     WITH (
     'connector.type' = 'jdbc',
     'connector.url' = 'jdbc:mysql://xx.xx.xx.xx:3306/xx',
     'connector.table' = 'jdbc_table_name',
     'connector.driver' = 'com.mysql.jdbc.Driver',
     'connector.username' = 'xxx',
     'connector.password' = 'xxxxx'
   );

   CREATE TABLE audi_cheaper_than_30w (
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_price INT
   )
     WITH (
     'connector.type' = 'dis',
     'connector.region' = '',
     'connector.channel' = 'disOutput',
     'connector.partition-key' = 'car_id,car_owner',
     'format.type' = 'csv'
     );

   INSERT INTO audi_cheaper_than_30w
   SELECT a.car_id, b.car_owner, b.car_brand, b.car_price
   FROM car_infos as a join db_info FOR SYSTEM_TIME AS OF a.proctime AS b on a.car_id = b.car_id;
