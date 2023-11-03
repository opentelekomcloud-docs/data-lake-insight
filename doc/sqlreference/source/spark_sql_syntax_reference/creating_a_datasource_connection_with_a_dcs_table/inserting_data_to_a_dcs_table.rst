:original_name: dli_08_0227.html

.. _dli_08_0227:

Inserting Data to a DCS Table
=============================

Function
--------

This statement is used to insert data in a DLI table to the DCS key.

Syntax
------

-  Insert the SELECT query result into a table.

   ::

      INSERT INTO DLI_TABLE
        SELECT field1,field2...
        [FROM DLI_TEST]
        [WHERE where_condition]
        [LIMIT num]
        [GROUP BY field]
        [ORDER BY field] ...;

-  Insert a data record into a table.

   ::

      INSERT INTO DLI_TABLE
        VALUES values_row [, values_row ...];

Keywords
--------

For details about the SELECT keywords, see :ref:`Basic SELECT Statements <dli_08_0150>`.

Parameter description
---------------------

.. table:: **Table 1** Parameter description

   +-------------------------+----------------------------------------------------------------------------------------------------+
   | Parameter               | Description                                                                                        |
   +=========================+====================================================================================================+
   | DLI_TABLE               | Name of the DLI table for which a datasource connection has been created.                          |
   +-------------------------+----------------------------------------------------------------------------------------------------+
   | DLI_TEST                | indicates the table that contains the data to be queried.                                          |
   +-------------------------+----------------------------------------------------------------------------------------------------+
   | field1,field2..., field | Column values in the DLI_TEST table must match the column values and types in the DLI_TABLE table. |
   +-------------------------+----------------------------------------------------------------------------------------------------+
   | where_condition         | Query condition.                                                                                   |
   +-------------------------+----------------------------------------------------------------------------------------------------+
   | num                     | Limit the query result. The num parameter supports only the INT type.                              |
   +-------------------------+----------------------------------------------------------------------------------------------------+
   | values_row              | Value to be inserted to a table. Use commas (,) to separate columns.                               |
   +-------------------------+----------------------------------------------------------------------------------------------------+

Precautions
-----------

-  The target DLI table must exist.

-  When creating a DLI table, you need to specify the schema information.

-  If **key.column** is specified during table creation, the value of the specified field is used as a part of the Redis key name. The following is an example:

   ::

      create table test_redis(name string, age int) using redis options(
        'host' = '192.168.4.199',
        'port' = '6379',
        'password' = '******',
        'table' = 'test_with_key_column',
        'key.column' = 'name'
      );
      insert into test_redis values("James", 35), ("Michael", 22);

   The Redis database contains two tables, naming **test_with_key_column:James** and **test_with_key_column:Michael** respectively.

   |image1|

   |image2|

-  If **key.column** is not specified during table creation, the key name in Redis uses the UUID. The following is an example:

   ::

      create table test_redis(name string, age int) using redis options(
        'host' = '192.168.7.238',
        'port' = '6379',
        'password' = '******',
        'table' = 'test_without_key_column'
      );
      insert into test_redis values("James", 35), ("Michael", 22);

   In Redis, there are two tables named **test_without_key_column:uuid**.

   |image3|

   |image4|

Example
-------

::

   INSERT INTO test_redis
     VALUES("James", 35), ("Michael", 22);

.. |image1| image:: /_static/images/en-us_image_0223994226.png
.. |image2| image:: /_static/images/en-us_image_0223994227.png
.. |image3| image:: /_static/images/en-us_image_0223994228.png
.. |image4| image:: /_static/images/en-us_image_0223994229.png
