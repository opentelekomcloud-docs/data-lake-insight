:original_name: dli_08_0462.html

.. _dli_08_0462:

Inserting Data to an Oracle Table
=================================

Function
--------

This statement is used to insert data into an associated Oracle table.

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

-  Overwriting the inserted data

   ::

      INSERT OVERWRITE TABLE DLI_TABLE
        SELECT field1,field2...
        [FROM DLI_TEST]
        [WHERE where_condition]
        [LIMIT num]
        [GROUP BY field]
        [ORDER BY field] ...;

Keywords
--------

For details about the SELECT keywords, see :ref:`Basic SELECT Statements <dli_08_0150>`.

Parameters
----------

.. table:: **Table 1** Parameters

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

A DLI table is available.

Example
-------

-  Query data in the user table and insert the data into the test table.

   ::

      INSERT INTO test
        SELECT ATTR_EXPR
        FROM user
        WHERE user_name='cyz'
        LIMIT 3
        GROUP BY user_age

-  Insert data 1 into the test table.

   .. code-block::

      INSERT INTO test
        VALUES (1);
