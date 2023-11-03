:original_name: dli_08_0071.html

.. _dli_08_0071:

Creating a Database
===================

Function
--------

This statement is used to create a database.

Syntax
------

::

   CREATE [DATABASE | SCHEMA] [IF NOT EXISTS] db_name
     [COMMENT db_comment]
     [WITH DBPROPERTIES (property_name=property_value, ...)];

Keyword
-------

-  **IF NOT EXISTS**: Prevents system errors if the database to be created exists.
-  **COMMENT**: Describes a database.

-  **DBPROPERTIES**: Specifies database attributes. The attribute name and attribute value appear in pairs.

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter      | Description                                                                                                                                          |
   +================+======================================================================================================================================================+
   | db_name        | Database name, which consists of letters, digits, and underscores (_). The value cannot contain only digits or start with a digit or underscore (_). |
   +----------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | db_comment     | Database description                                                                                                                                 |
   +----------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | property_name  | Database property name                                                                                                                               |
   +----------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | property_value | Database property value                                                                                                                              |
   +----------------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

-  **DATABASE** and **SCHEMA** can be used interchangeably. You are advised to use **DATABASE**.
-  The **default** database is a built-in database. You cannot create a database named **default**.

.. _dli_08_0071__en-us_topic_0114776165_en-us_topic_0093946907_se85f897bfc724638829c13a14150cab6:

Example
-------

#. Create a queue. A queue is the basis for using DLI. Before executing SQL statements, you need to create a queue.

#. On the DLI management console, click **SQL Editor** in the navigation pane on the left. The **SQL Editor** page is displayed.

#. In the editing window on the right of the **SQL Editor** page, enter the following SQL statement for creating a database and click **Execute**. Read and agree to the privacy agreement, and click **OK**.

   If database **testdb** does not exist, run the following statement to create database **testdb**:

   ::

      CREATE DATABASE IF NOT EXISTS testdb;
