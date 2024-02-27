:original_name: dli_spark_percentlie.html

.. _dli_spark_percentlie:

percentlie
==========

This function is used to return the exact percentile, which is applicable to a small amount of data. It sorts a specified column in ascending order, and then obtains the exact value of the pth percentile.

Syntax
------

.. code-block::

   percentile(colname,DOUBLE p)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                         |
   +=================+=================+=================+=====================================================+
   | colname         | Yes             | STRING          | Name of the column to be sorted                     |
   |                 |                 |                 |                                                     |
   |                 |                 |                 | The elements in the column must be integers.        |
   +-----------------+-----------------+-----------------+-----------------------------------------------------+
   | p               | Yes             | DOUBLE          | The value ranges from 0 to 1. The value is a float. |
   +-----------------+-----------------+-----------------+-----------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE or ARRAY type.

.. note::

   -  If the column name does not exist, an error is reported.
   -  If the value of **p** is **NULL** or outside the range of [0,1], an error is reported.

Example Code
------------

Assume that the elements in the **int_test** column are 1, 2, 3, and 4 and they are of the INT type.

The value **3.0999999999999996** is returned.

.. code-block::

   select percentile(int_test,0.7) FROM int_test;

The value **3.997** is returned.

.. code-block::

   select percentile(int_test,0.999) FROM int_test;

The value **2.5** is returned.

.. code-block::

   select percentile(int_test,0.5) FROM int_test;

The value **[1.3, 1.9, 2.5, 2.8, 3.7]** is returned.

.. code-block::

   select percentile (int_test,ARRAY(0.1,0.3,0.5,0.6,0.9)) FROM int_test;
