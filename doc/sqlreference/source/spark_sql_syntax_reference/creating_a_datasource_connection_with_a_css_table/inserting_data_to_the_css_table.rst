:original_name: dli_08_0202.html

.. _dli_08_0202:

Inserting Data to the CSS Table
===============================

Function
--------

This statement is used to insert data in a DLI table to the associated CSS table.

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

-  A DLI table is available.
-  When creating the DLI table, you need to specify the **schema** information. If the number and type of fields selected in the **SELECT** clause or in **Values** do not match the **Schema** information in the CSS table, the system reports an error.
-  Inconsistent types may not always cause error reports. For example, if the data of the **int** type is inserted, but the **text** type is saved in the CSS **Schema**, the **int** type will be converted to the **text** type and no error will be reported.
-  You are advised not to concurrently insert data into a table. If you concurrently insert data into a table, there is a possibility that conflicts occur, leading to failed data insertion.

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
