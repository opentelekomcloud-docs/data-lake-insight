:original_name: dli_spark_factorial.html

.. _dli_spark_factorial:

factorial
=========

This function is used to return the factorial of **a**.

Syntax
------

.. code-block::

   factorial(INT a)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+-----------------------------------+-------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                              | Description                                                                                                 |
   +=================+=================+===================================+=============================================================================================================+
   | a               | Yes             | BIGINT, INT, SMALLINT, or TINYINT | The value is an integer.                                                                                    |
   |                 |                 |                                   |                                                                                                             |
   |                 |                 |                                   | If the value is not of the INT type, the system will implicitly convert it to the INT type for calculation. |
   |                 |                 |                                   |                                                                                                             |
   |                 |                 |                                   | The string is converted to its corresponding ASCII code.                                                    |
   +-----------------+-----------------+-----------------------------------+-------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   -  If the value of **a** is **0**, **1** is returned.
   -  If the value of **a** is **NULL** or outside the range of [0,20], **NULL** is returned.

Example Code
------------

The value **720** is returned.

.. code-block::

   select factorial(6);

The value **1** is returned.

.. code-block::

   select factorial(1);

The value **120** is returned.

.. code-block::

   select factorial(5.123456);

The value **NULL** is returned.

.. code-block::

   select factorial(null);

The value **NULL** is returned.

.. code-block::

   select factorial(21);
