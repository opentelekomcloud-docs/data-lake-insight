:original_name: dli_08_0090.html

.. _dli_08_0090:

Checking All Tables
===================

Function
--------

This statement is used to check all tables and views in the current database.

Syntax
------

::

   SHOW TABLES [IN | FROM db_name] [LIKE regex_expression];

Keywords
--------

FROM/IN: followed by the name of a database whose tables and views will be displayed.

Parameters
----------

.. table:: **Table 1** Parameters

   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter        | Description                                                                                                                                          |
   +==================+======================================================================================================================================================+
   | db_name          | Database name, which consists of letters, digits, and underscores (_). The value cannot contain only digits or start with a digit or underscore (_). |
   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | regex_expression | Name of a database table.                                                                                                                            |
   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

None

Example
-------

#. Create a table. For details, see :ref:`Creating an OBS Table <dli_08_0223>` or :ref:`Creating a DLI Table <dli_08_0224>`.

#. To show all tables and views in the current database, run the following statement:

   ::

      SHOW TABLES;

#. To show all tables started with **test** in the **testdb** database, run the following statement:

   ::

      SHOW TABLES IN testdb LIKE "test*";
