:original_name: dli_08_0074.html

.. _dli_08_0074:

Viewing All Databases
=====================

Function
--------

This syntax is used to query all current databases.

Syntax
------

::

   SHOW [DATABASES | SCHEMAS] [LIKE regex_expression];

Keywords
--------

None

Parameters
----------

.. table:: **Table 1** Parameter

   ================ =============
   Parameter        Description
   ================ =============
   regex_expression Database name
   ================ =============

Precautions
-----------

Keyword DATABASES is equivalent to SCHEMAS. You can use either of them in this statement.

Example
-------

View all the current databases.

::

   SHOW DATABASES;

View all databases whose names start with **test**.

::

   SHOW DATABASES LIKE "test.*";
