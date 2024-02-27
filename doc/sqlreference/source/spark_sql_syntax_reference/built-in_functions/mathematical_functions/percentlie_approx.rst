:original_name: dli_spark_percentlie_approx.html

.. _dli_spark_percentlie_approx:

percentlie_approx
=================

This function is used to return the approximate percentile, which is applicable to a large amount of data. It sorts a specified column in ascending order, and then obtains the value closest to the pth percentile.

Syntax
------

.. code-block::

   percentile_approx (colname,DOUBLE p)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                     |
   +=================+=================+=================+=================================================================================================================================================================================+
   | colname         | Yes             | STRING          | Name of the column to be sorted                                                                                                                                                 |
   |                 |                 |                 |                                                                                                                                                                                 |
   |                 |                 |                 | The elements in a column are of the DOUBLE type. If an element in a column is not of the DOUBLE type, the system will implicitly convert it to the DOUBLE type for calculation. |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | p               | Yes             | DOUBLE          | The value can be a float, integer, or string.                                                                                                                                   |
   |                 |                 |                 |                                                                                                                                                                                 |
   |                 |                 |                 | The value ranges from 0 to 1. The value is a float.                                                                                                                             |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE or ARRAY type.

.. note::

   -  If the column name does not exist, an error is reported.
   -  If the value of **p** is **NULL** or outside the range of [0,1], an error is reported.

Example Code
------------

Assume that the elements in the **int_test** column are 1, 2, 3, and 4 and they are of the INT type.

The value **3** is returned.

.. code-block::

   select percentile_approx(int_test,0.7) FROM int_test;

The value **3** is returned.

.. code-block::

   select percentile_approx(int_test,0.75) FROM int_test;

The value **2** is returned.

.. code-block::

   select percentile_approx(int_test,0.5) FROM int_test;

The value **[1,2,2,3,4]** is returned.

.. code-block::

   select percentile_approx (int_test,ARRAY(0.1,0.3,0.5,0.6,0.9)) FROM int_test;
