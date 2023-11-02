:original_name: dli_08_0158.html

.. _dli_08_0158:

DISTRIBUTE BY
=============

Function
--------

This statement is used to bucket a table according to the field.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     DISTRIBUTE BY col_name [,col_name ,...];

Keyword
-------

DISTRIBUTE BY: Buckets are created based on specified fields. A single field or multiple fields are supported, and the fields are not sorted in the bucket. This parameter is used together with SORT BY to sort data after bucket division.

Precautions
-----------

The to-be-sorted table must exist. If this statement is used to sort a table that does not exist, an error is reported.

Example Value
-------------

To bucket the **student** table according to the **score** field, run the following statement:

::

   SELECT * FROM student
     DISTRIBUTE BY score;
