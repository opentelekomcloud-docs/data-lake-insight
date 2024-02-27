:original_name: dli_08_0463.html

.. _dli_08_0463:

Querying an Oracle Table
========================

Function
--------

This statement is used to query data in an Oracle table.

Syntax
------

::

   SELECT * FROM table_name LIMIT number;

Keywords
--------

LIMIT is used to limit the query results. Only INT type is supported by the **number** parameter.

Precautions
-----------

If schema information is not specified during table creation, the query result contains the **\_id** field for storing **\_id** in the DOC file.

Example
-------

Querying data in the **test_oracle** table

::

   SELECT * FROM test_oracle limit 100;
