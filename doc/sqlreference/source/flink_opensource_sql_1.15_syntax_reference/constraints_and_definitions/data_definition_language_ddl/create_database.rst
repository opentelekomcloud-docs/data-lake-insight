:original_name: dli_08_15008.html

.. _dli_08_15008:

CREATE DATABASE
===============

Function
--------

This statement creates a database using specified table attributes. If a table with the same name already exists in the database, an exception is thrown.

Syntax
------

.. code-block::

   CREATE DATABASE [IF NOT EXISTS] [catalog_name.]db_name
     [COMMENT database_comment]
     WITH (key1=val1, key2=val2, ...)

Description
-----------

**IF NOT EXISTS**

If the database already exists, no operation is performed.

**WITH OPTIONS**

Database attributes typically store additional information about the database.

The key and value of the **key1=val1** expression are string literals.
