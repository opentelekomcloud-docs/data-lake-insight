:original_name: dli_08_0305.html

.. _dli_08_0305:

Redis Source Table
==================

Function
--------

Create a source stream to obtain data from Redis as input for jobs.

Prerequisites
-------------

An enhanced datasource connection with Redis has been established, so that you can configure security group rules as required.

Syntax
------

::

   create table dwsSource (
     attr_name attr_type
     (',' attr_name attr_type)*
     (',' watermark for rowtime_column_name as watermark-strategy_expression)
   )
   with (
     'connector.type' = 'redis',
     'connector.host' = '',
     'connector.port' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +-------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                     | Mandatory             | Description                                                                                                                                                                                                                                                                |
   +===============================+=======================+============================================================================================================================================================================================================================================================================+
   | connector.type                | Yes                   | Connector type. Set this parameter to **redis**.                                                                                                                                                                                                                           |
   +-------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.host                | Yes                   | Redis connector address                                                                                                                                                                                                                                                    |
   +-------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.port                | Yes                   | Redis connector port                                                                                                                                                                                                                                                       |
   +-------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.password            | No                    | Redis authentication password                                                                                                                                                                                                                                              |
   +-------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.deploy-mode         | No                    | Redis deployment mode. The value can be **standalone** or **cluster**. The default value is **standalone**.                                                                                                                                                                |
   +-------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.table-name          | No                    | Name of the table stored in the Redis. This parameter is mandatory in the Redis Hashmap storage pattern. In this pattern, data is stored to Redis in hashmaps. The hash key is **${table-name}:${ext-key}**, and the field name is the column name.                        |
   |                               |                       |                                                                                                                                                                                                                                                                            |
   |                               |                       | .. note::                                                                                                                                                                                                                                                                  |
   |                               |                       |                                                                                                                                                                                                                                                                            |
   |                               |                       |    Table storage pattern: **connector.table-name** and **connector.key-column** are used as Redis keys. For the Redis hash type, each key corresponds to a hashmap. A hash key is a field name of the source table, and a hash value is a field value of the source table. |
   +-------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.use-internal-schema | No                    | Whether to use the existing schema in the Redis. This parameter is optional in the Redis Hashmap storage pattern. The default value is **false**.                                                                                                                          |
   +-------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.key-column          | No                    | This parameter is optional in table storage pattern. The value is used as the value of ext-key in the Redis. If this parameter is not set, the value of ext-key is the generated UUID.                                                                                     |
   +-------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Reads data from Redis.

.. code-block::

   create table redisSource(
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_speed INT
   ) with (
    'connector.type' = 'redis',
     'connector.host' = 'xx.xx.xx.xx',
     'connector.port' = '6379',
     'connector.password' = 'xx',
     'connector.table-name' = 'car_info'
   );
