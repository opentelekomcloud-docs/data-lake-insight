:original_name: dli_spark_log2.html

.. _dli_spark_log2:

log2
====

This function is used to return the natural logarithm of a given value with a base of 2.

Syntax
------

.. code-block::

   log2(DOUBLE a)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                               | Description                                                                                                       |
   +=================+=================+====================================+===================================================================================================================+
   | a               | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string.                                                                     |
   |                 |                 |                                    |                                                                                                                   |
   |                 |                 |                                    | If the value is not of the DOUBLE type, the system will implicitly convert it to the DOUBLE type for calculation. |
   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   If the value of **a** is negative, **0**, or **NULL**, **NULL** is returned.

Example Code
------------

The value **NULL** is returned.

.. code-block::

   select log2(null);

The value **NULL** is returned.

.. code-block::

   select log2(0);

The value **3.1699250014423126** is returned.

.. code-block::

   select log2(9);

The value **4** is returned.

.. code-block::

   select log2(16);
