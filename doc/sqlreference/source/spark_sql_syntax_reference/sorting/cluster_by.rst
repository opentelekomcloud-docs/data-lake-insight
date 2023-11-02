:original_name: dli_08_0157.html

.. _dli_08_0157:

CLUSTER BY
==========

Function
--------

This statement is used to bucket a table and sort the table within buckets.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     CLUSTER BY col_name [,col_name ,...];

Keyword
-------

CLUSTER BY: Buckets are created based on specified fields. Single fields and multiple fields are supported, and data is sorted in buckets.

Precautions
-----------

The to-be-sorted table must exist. If this statement is used to sort a table that does not exist, an error is reported.

Example
-------

To bucket the **student** table according to the **score** field and sort tables within buckets in descending order, run the following statement:

::

   SELECT * FROM student
     CLUSTER BY score;
