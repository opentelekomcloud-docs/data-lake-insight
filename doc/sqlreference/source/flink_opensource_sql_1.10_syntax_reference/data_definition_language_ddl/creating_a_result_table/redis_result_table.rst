:original_name: dli_08_0313.html

.. _dli_08_0313:

Redis Result Table
==================

Function
--------

DLI exports the output data of the Flink job to Redis. Redis is a storage system that supports multiple types of data structures such as key-value. It can be used in scenarios such as caching, event pub/sub, and high-speed queuing. Redis supports direct read/write of strings, hashes, lists, queues, and sets. Redis works with in-memory dataset and provides persistence. For more information about Redis, visit https://redis.io/.

Prerequisites
-------------

An enhanced datasource connection with Redis has been established, so that you can configure security group rules as required.

Syntax
------

::

   create table dwsSink (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
   'connector.type' = 'redis',
     'connector.host' = '',
     'connector.port' = '',
     'connector.password' = '',
     'connector.table-name' = '',
     'connector.key-column' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +-----------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                   | Mandatory             | Description                                                                                                                                                                                                                                                                |
   +=============================+=======================+============================================================================================================================================================================================================================================================================+
   | connector.type              | Yes                   | Connector type. Set this parameter to **redis**.                                                                                                                                                                                                                           |
   +-----------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.host              | Yes                   | Redis connector address                                                                                                                                                                                                                                                    |
   +-----------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.port              | Yes                   | Redis connector port                                                                                                                                                                                                                                                       |
   +-----------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.password          | No                    | Redis authentication password                                                                                                                                                                                                                                              |
   +-----------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.deploy-mode       | No                    | Redis deployment mode. The value can be **standalone** or **cluster**. The default value is **standalone**.                                                                                                                                                                |
   +-----------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.table-name        | No                    | Name of the table stored in the Redis. This parameter is mandatory in the Redis Hashmap storage pattern. In this pattern, data is stored to Redis in hashmaps. The hash key is **${table-name}:${ext-key}**, and the field name is the column name.                        |
   |                             |                       |                                                                                                                                                                                                                                                                            |
   |                             |                       | .. note::                                                                                                                                                                                                                                                                  |
   |                             |                       |                                                                                                                                                                                                                                                                            |
   |                             |                       |    Table storage pattern: **connector.table-name** and **connector.key-column** are used as Redis keys. For the Redis hash type, each key corresponds to a hashmap. A hash key is a field name of the source table, and a hash value is a field value of the source table. |
   +-----------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.key-column        | No                    | This parameter is optional in table storage pattern. The value is used as the value of ext-key in the Redis. If this parameter is not set, the value of ext-key is the generated UUID.                                                                                     |
   +-----------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write-schema      | No                    | Whether to write the current schema to the Redis. This parameter is available in table storage pattern. The default value is **false**.                                                                                                                                    |
   +-----------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.data-type         | No                    | Data types for storage. This parameter is mandatory for a custom storage pattern. Supported values include string, list, hash, and set. In a string, list or set, the number of schema fields must be 2, and the number of hash fields must be 3.                          |
   +-----------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.ignore-retraction | No                    | Whether to ignore the retraction message. The default value is **false**.                                                                                                                                                                                                  |
   +-----------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

Either **connector.table-name** or **connector.data-type** must be set.

Example
-------

-  Configure the table storage pattern when you configure **connector.table-name**.

   In table storage pattern, data is stored in hash mode, which is different from the basic hash pattern in which the three fields of a table are used as the **key**, **hash_key**, and **hash_value**. The key in table pattern can be specified by **connector.table-name** and **connector.key-column** parameters, all field names in the table are used as **hash_key**, and the field values are written to the hash table as **hash_value**.

   .. code-block::

      create table redisSink(
        car_id STRING,
        car_owner STRING,
        car_brand STRING,
        car_speed INT
      ) with (
        'connector.type' = 'redis',
        'connector.host' = 'xx.xx.xx.xx',
        'connector.port' = '6379',
        'connector.password' = 'xx',
        'connector.table-name'='car_info',
        'connector.key-column'='car_id'
      );

      insert into redisSink
        (car_id,car_owner,car_brand,car_speed)
        VALUES
        ("A1234","OwnA","A1234",30);

-  The following example shows how to create a table when **connector.data-type** is set to **string**, **list**, **hash**, or **set**, respectively.

   -  String type

      The table contains two columns: key and value.

      .. code-block::

         create table redisSink(
           attr1 STRING,
           attr2 STRING
         ) with (
           'connector.type' = 'redis',
           'connector.host' = 'xx.xx.xx.xx',
           'connector.port' = '6379',
           'connector.password' = 'xx',
           'connector.data-type' = 'string'
         );

         insert into redisSink
           (attr1,attr2)
           VALUES
           ("car_id","A1234");

   -  List type

      The table contains two columns: key and value.

      ::

         create table redisSink(
           attr1 STRING,
           attr2 STRING
         ) with (
           'connector.type' = 'redis',
           'connector.host' = 'xx.xx.xx.xx',
           'connector.port' = '6379',
           'connector.password' = 'xx',
           'connector.data-type' = 'list'
         );

         insert into redisSink
           (attr1,attr2)
           VALUES
           ("car_id","A1234");

   -  Set type

      The table contains two columns: key and value.

      .. code-block::

         create table redisSink(
           attr1 STRING,
           attr2 STRING
         ) with (
           'connector.type' = 'redis',
           'connector.host' = 'xx.xx.xx.xx',
           'connector.port' = '6379',
           'connector.password' = 'xx',
           'connector.data-type' = 'set'
         );

         insert into redisSink
           (attr1,attr2)
           VALUES
           ("car_id","A1234");

   -  Hash type

      The table contains three columns: key, hash_key, and hash_value.

      .. code-block::

         create table redisSink(
           attr1 STRING,
           attr2 STRING,
           attr3 STRING
         ) with (
           'connector.type' = 'redis',
           'connector.host' = 'xx.xx.xx.xx',
           'connector.port' = '6379',
           'connector.password' = 'xx',
           'connector.data-type' = 'hash'
         );

         insert into redisSink
           (attr1,attr2,attr3)
           VALUES
           ("car_info","car_id","A1234");
