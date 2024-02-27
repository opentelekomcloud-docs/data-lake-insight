:original_name: dli_08_0203.html

.. _dli_08_0203:

Querying the CSS Table
======================

This statement is used to query data in a CSS table.

Syntax
------

::

   SELECT * FROM table_name LIMIT number;

Keywords
--------

LIMIT is used to limit the query results. Only INT type is supported by the **number** parameter.

Precautions
-----------

The table to be queried must exist. Otherwise, an error is reported.

Example
-------

To query data in the **dli_to_css** table, enter the following statement:

::

   SELECT * FROM dli_to_css limit 100;
