:original_name: dli_spark_exp.html

.. _dli_spark_exp:

exp
===

This function is used to return the value of **e** raised to the power of **a**.

Syntax
------

.. code-block::

   exp(DOUBLE a)

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

   If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **7.38905609893065** is returned.

.. code-block::

   select exp(2);

The value **20.085536923187668** is returned.

.. code-block::

   select exp(3);

The value **NULL** is returned.

.. code-block::

   select exp(null);
