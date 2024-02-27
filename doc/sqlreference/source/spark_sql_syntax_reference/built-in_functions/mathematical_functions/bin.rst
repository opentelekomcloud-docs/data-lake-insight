:original_name: dli_spark_bin.html

.. _dli_spark_bin:

bin
===

This function is used to return the binary format of **a**.

Syntax
------

.. code-block::

   bin(BIGINT a)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                               | Description                                                                                                       |
   +=================+=================+====================================+===================================================================================================================+
   | a               | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | The value is an integer.                                                                                          |
   |                 |                 |                                    |                                                                                                                   |
   |                 |                 |                                    | If the value is not of the BIGINT type, the system will implicitly convert it to the BIGINT type for calculation. |
   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **1** is returned.

.. code-block::

   select bin(1);

The value **NULL** is returned.

.. code-block::

   select bin(null);

The value **1000** is returned.

.. code-block::

   select bin(8);

The value **1000** is returned.

.. code-block::

   select bin(8.123456);
