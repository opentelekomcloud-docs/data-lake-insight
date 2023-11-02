:original_name: dli_08_0206.html

.. _dli_08_0206:

Table-Valued Functions
======================

Table-valued functions can convert one row of records into multiple rows or convert one column of records into multiple columns. Table-valued functions can only be used in JOIN LATERAL TABLE.

.. table:: **Table 1** Table-valued functions

   +--------------------------------+------------------+------------------------------------------------------------------------------------+
   | Function                       | Return Data Type | Description                                                                        |
   +================================+==================+====================================================================================+
   | split_cursor(value, delimiter) | cursor           | Separates the "value" string into multiple rows of strings by using the delimiter. |
   +--------------------------------+------------------+------------------------------------------------------------------------------------+

Example
-------

Input one record ("student1", "student2, student3") and output two records ("student1", "student2") and ("student1", "student3").

.. code-block::

   create source stream s1(attr1 string, attr2 string) with (......);
   insert into s2 select  attr1, b1 from s1 left join lateral table(split_cursor(attr2, ',')) as T(b1) on true;
