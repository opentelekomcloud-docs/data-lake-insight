:original_name: dli_08_0226.html

.. _dli_08_0226:

Creating a DLI Table and Associating It with DCS
================================================

Function
--------

This statement is used to create a DLI table and associate it with an existing DCS key.

.. note::

   In Spark cross-source development scenarios, there is a risk of password leakage if datasource authentication information is directly configured. You are advised to use the datasource authentication provided by DLI.

Prerequisites
-------------

Before creating a DLI table and associating it with DCS, you need to create a datasource connection and bind it to a queue. For details about operations on the management console, see

Syntax
------

-  Specified key

   ::

      CREATE TABLE [IF NOT EXISTS] TABLE_NAME(
          FIELDNAME1 FIELDTYPE1,
          FIELDNAME2 FIELDTYPE2)
        USING REDIS OPTIONS (
        'host'='xx',
        'port'='xx',
        'passwdauth' = 'xxx',
        'encryption' = 'true',
        'table'='namespace_in_redis:key_in_redis',
        'key.column'= 'FIELDNAME1'
      );

-  Wildcard key

   ::

      CREATE TABLE [IF NOT EXISTS] TABLE_NAME(
          FIELDNAME1 FIELDTYPE1,
          FIELDNAME2 FIELDTYPE2)
        USING REDIS OPTIONS (
        'host'='xx',
        'port'='xx',
        'passwdauth' = 'xxx',
        'encryption' = 'true',
        'keys.pattern'='key*:*',
        'key.column'= 'FIELDNAME1'
      );

Keywords
--------

.. table:: **Table 1** CREATE TABLE keywords

   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                               |
   +===================================+===========================================================================================================================================================================================================+
   | host                              | To connect to DCS, you need to create a datasource connection first.                                                                                                                                      |
   |                                   |                                                                                                                                                                                                           |
   |                                   | After creating an enhanced datasource connection, use the connection address provided by DCS. If there are multiple connection addresses, select one of them.                                             |
   |                                   |                                                                                                                                                                                                           |
   |                                   | .. note::                                                                                                                                                                                                 |
   |                                   |                                                                                                                                                                                                           |
   |                                   |    Currently, only enhanced datasource is supported.                                                                                                                                                      |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | port                              | DCS connection port, for example, 6379.                                                                                                                                                                   |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                          | Password entered during DCS cluster creation. You do not need to set this parameter when accessing a non-secure Redis cluster.                                                                            |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | passwdauth                        | Datasource password authentication name. For details about how to create datasource authentication, see Datasource Authentication in the *Data Lake Insight User Guide*.                                  |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encryption                        | Set this parameter to **true** when datasource password authentication is used.                                                                                                                           |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table                             | The key or hash key in Redis.                                                                                                                                                                             |
   |                                   |                                                                                                                                                                                                           |
   |                                   | -  This parameter is mandatory when Redis data is inserted.                                                                                                                                               |
   |                                   | -  Either this parameter or the **keys.pattern** parameter when Redis data is queried.                                                                                                                    |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | keys.pattern                      | Use a regular expression to match multiple keys or hash keys. This parameter is used only for query. Either this parameter or **table** is used to query Redis data.                                      |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key.column                        | (Optional) Specifies a field in the schema as the key ID in Redis. This parameter is used together with the **table** parameter when data is inserted.                                                    |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | partitions.number                 | Number of concurrent tasks during data reading.                                                                                                                                                           |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.count                        | Number of data records read in each batch. The default value is **100**. If the CPU usage of the Redis cluster still needs to be improved during data reading, increase the value of this parameter.      |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | iterator.grouping.size            | Number of data records inserted in each batch. The default value is **100**. If the CPU usage of the Redis cluster still needs to be improved during the insertion, increase the value of this parameter. |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | timeout                           | Timeout interval for connecting to the Redis, in milliseconds. The default value is **2000** (2 seconds).                                                                                                 |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. note::

   When connecting to DCS, complex data types such as Array, Struct, and Map are not supported.

   The following methods can be used to process complex data:

   -  Place the fields of the next level in the Schema field of the same level.
   -  Write and read data in binary mode, and encode and decode it using user-defined functions.

Example
-------

-  Specifying a table

::

   create table test_redis(name string, age int) using redis options(
     'host' = '192.168.4.199',
     'port' = '6379',
     'passwdauth' = 'xxx',
     'encryption' = 'true',
     'table' = 'person'
   );

-  Wildcarding the table name

::

   create table test_redis_keys_patten(id string, name string, age int) using redis options(
     'host' = '192.168.4.199',
     'port' = '6379',
     'passwdauth' = 'xxx',
     'encryption' = 'true',
     'keys.pattern' = 'p*:*',
     'key.column' = 'id'
   );
