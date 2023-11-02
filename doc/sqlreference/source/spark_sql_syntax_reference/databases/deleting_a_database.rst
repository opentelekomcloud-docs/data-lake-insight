:original_name: dli_08_0072.html

.. _dli_08_0072:

Deleting a Database
===================

Function
--------

This statement is used to delete a database.

Syntax
------

::

   DROP [DATABASE | SCHEMA] [IF EXISTS] db_name [RESTRICT|CASCADE];

Keyword
-------

**IF EXISTS**: Prevents system errors if the database to be deleted does not exist.

Precautions
-----------

-  **DATABASE** and **SCHEMA** can be used interchangeably. You are advised to use **DATABASE**.
-  **RESTRICT**: If the database is not empty (tables exist), an error is reported and the **DROP** operation fails. **RESTRICT** is the default logic.
-  **CASCADE**: Even if the database is not empty (tables exist), the **DROP** will delete all the tables in the database. Therefore, exercise caution when using this function.

Parameters
----------

.. table:: **Table 1** Parameter description

   +-----------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Description                                                                                                                                          |
   +===========+======================================================================================================================================================+
   | db_name   | Database name, which consists of letters, digits, and underscores (_). The value cannot contain only digits or start with a digit or underscore (_). |
   +-----------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

#. Create a database, for example, **testdb**, by referring to :ref:`Example <dli_08_0071__en-us_topic_0114776165_en-us_topic_0093946907_se85f897bfc724638829c13a14150cab6>`.

#. Run the following statement to delete database **testdb** if it exists:

   ::

      DROP DATABASE IF EXISTS testdb;
