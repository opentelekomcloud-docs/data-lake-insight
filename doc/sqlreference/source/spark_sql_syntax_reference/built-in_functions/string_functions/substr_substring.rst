:original_name: dli_spark_substr_substring.html

.. _dli_spark_substr_substring:

substr/substring
================

This function is used to return the substring of **str**, starting from **start_position** and with a length of **length**.

Syntax
------

.. code-block::

   substr(string <str>, bigint <start_position>[, bigint <length>])

or

.. code-block::

   substring(string <str>, bigint <start_position>[, bigint <length>])

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                             |
   +=================+=================+=================+=========================================================================================================================================+
   | str             | Yes             | STRING          | If the value is of the BIGINT, DECIMAL, DOUBLE, or DATETIME type, the value is implicitly converted to the STRING type for calculation. |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | start_position  | Yes             | BIGINT          | Start position. The default value is **1**.                                                                                             |
   |                 |                 |                 |                                                                                                                                         |
   |                 |                 |                 | If the value is positive, the substring is returned from the left. If the value is negative, the substring is returned from the right.  |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | length          | No              | BIGINT          | Length of the substring. The value must be greater than 0.                                                                              |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **str** is not of the STRING, BIGINT, DECIMAL, DOUBLE, or DATETIME type, an error is reported.
   -  If the value of **length** is not of the BIGINT type or is less than or equal to 0, an error is reported.
   -  When the **length** parameter is omitted, a substring that ends with **str** is returned.
   -  If the value of **str**, **start_position**, or **length** is **NULL**, **NULL** is returned.

Example Code
------------

The value **k SQL** is returned.

.. code-block::

   SELECT substr('Spark SQL', 5);

The value **SQL** is returned.

.. code-block::

   SELECT substr('Spark SQL', -3);

The value **k** is returned.

.. code-block::

   SELECT substr('Spark SQL', 5, 1);
