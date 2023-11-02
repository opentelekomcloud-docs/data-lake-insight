:original_name: dli_08_0180.html

.. _dli_08_0180:

AS for Table
============

Function
--------

This statement is used to specify an alias for a table or the subquery result.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference [AS] alias;

Keyword
-------

-  table_reference: Can be a table, view, or subquery.
-  As: Is used to connect to table_reference and alias. Whether this keyword is added or not does not affect the command execution result.

Precautions
-----------

-  The to-be-queried table must exist. Otherwise, an error is reported.
-  The alias must be specified before execution of the statement. Otherwise, an error is reported. You are advised to specify a unique alias.

Example
-------

-  To specify alias **n** for table **simple_table** and visit the name field in table **simple_table** by using n.name, run the following statement:

   ::

      SELECT n.score FROM simple_table n WHERE n.name = "leilei";

-  To specify alias **m** for the subquery result and return all the query results using **SELECT \* FROM m**, run the following statement:

   ::

      SELECT * FROM (SELECT * FROM simple_table WHERE score > 90) AS m;
