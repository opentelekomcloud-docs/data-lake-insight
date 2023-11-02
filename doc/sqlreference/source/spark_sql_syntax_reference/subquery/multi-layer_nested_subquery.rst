:original_name: dli_08_0178.html

.. _dli_08_0178:

Multi-Layer Nested Subquery
===========================

Function
--------

This statement is used to nest queries in the subquery.

Syntax
------

::

   SELECT attr_expr FROM ( SELECT attr_expr FROM ( SELECT attr_expr FROM... ... ) [alias] ) [alias];

Keyword
-------

-  All is used to return repeated rows. By default, all repeated rows are returned. It is followed by asterisks (*) only. Otherwise, an error will occur.
-  DISTINCT is used to remove the repeated line from the result.

Precautions
-----------

-  The to-be-queried table must exist. If this statement is used to query a table that does not exist, an error is reported.
-  The alias of the subquery must be specified in the nested query. Otherwise, an error is reported.
-  The alias must be specified before the running of the statement. Otherwise, an error is reported. It is advised to specify a unique alias.

Example
-------

To return the name field from the **user_info** table after three queries, run the following statement:

::

   SELECT name FROM ( SELECT name, acc_num FROM ( SELECT name, acc_num, password FROM ( SELECT name, acc_num, password, bank_acc FROM user_info) a ) b ) c;
