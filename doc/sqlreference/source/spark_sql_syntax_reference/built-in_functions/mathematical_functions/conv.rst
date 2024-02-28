:original_name: dli_spark_conv.html

.. _dli_spark_conv:

conv
====

This function is used to convert a number from **from_base** to **to_base**.

Syntax
------

.. code-block::

   conv(BIGINT num, INT from_base, INT to_base)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+------------------------------------+------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                               | Description                                                |
   +=================+=================+====================================+============================================================+
   | num             | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | Number whose base needs to be converted                    |
   |                 |                 |                                    |                                                            |
   |                 |                 |                                    | The value can be a float, integer, or string.              |
   +-----------------+-----------------+------------------------------------+------------------------------------------------------------+
   | from_base       | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | It represents the base from which the number is converted. |
   |                 |                 |                                    |                                                            |
   |                 |                 |                                    | The value can be a float, integer, or string.              |
   +-----------------+-----------------+------------------------------------+------------------------------------------------------------+
   | to_base         | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | It represents the base to which the number is converted.   |
   |                 |                 |                                    |                                                            |
   |                 |                 |                                    | The value can be a float, integer, or string.              |
   +-----------------+-----------------+------------------------------------+------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **num**, **from_base**, or **to_base** is **NULL**, **NULL** is returned.
   -  The conversion process works with 64-bit precision and returns **NULL** when there is overflow.
   -  If the value of **num** is a decimal, it will be converted to an integer before the base conversion, and the decimal part will be discarded.

Example Code
------------

The value **8** is returned.

.. code-block::

   select conv('1000', 2, 10);

The value **B** is returned.

.. code-block::

   select conv('1011', 2, 16);

The value **703710** is returned.

.. code-block::

   select conv('ABCDE', 16, 10);

The value **27** is returned.

.. code-block::

   select conv(1000.123456, 3.123456, 10.123456);

The value **18446744073709551589** is returned.

.. code-block::

   select conv(-1000.123456, 3.123456, 10.123456);

The value **NULL** is returned.

.. code-block::

   select conv('1100', null, 10);
