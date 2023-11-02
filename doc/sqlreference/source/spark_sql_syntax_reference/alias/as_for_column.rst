:original_name: dli_08_0181.html

.. _dli_08_0181:

AS for Column
=============

Function
--------

This statement is used to specify an alias for a column.

Syntax
------

::

   SELECT attr_expr [AS] alias, attr_expr [AS] alias, ... FROM table_reference;

Keyword
-------

-  alias: gives an alias for the **attr_expr** field.
-  AS: Whether to add AS does not affect the result.

Precautions
-----------

-  The to-be-queried table must exist. Otherwise, an error is reported.
-  The alias must be specified before execution of the statement. Otherwise, an error is reported. You are advised to specify a unique alias.

Example
-------

Run **SELECT name AS n FROM simple_table WHERE score > 90** to obtain the subquery result. The alias **n** for **name** can be used by external SELECT statement.

::

   SELECT n FROM (SELECT name AS n FROM simple_table WHERE score > 90) m WHERE n = "xiaoming";
