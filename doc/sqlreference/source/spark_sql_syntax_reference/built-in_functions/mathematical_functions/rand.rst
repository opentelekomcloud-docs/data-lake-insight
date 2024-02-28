:original_name: dli_spark_rand.html

.. _dli_spark_rand:

rand
====

This function is used to return an evenly distributed random number that is greater than or equal to 0 and less than 1.

Syntax
------

.. code-block::

   rand(INT seed)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                      |
   +=================+=================+=================+==================================================================================================================+
   | seed            | No              | INT             | The value can be a float, integer, or string.                                                                    |
   |                 |                 |                 |                                                                                                                  |
   |                 |                 |                 | If this parameter is specified, a stable random number sequence is obtained within the same running environment. |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

Example Code
------------

The value **0.3668915240363728** is returned.

.. code-block::

   select rand();

The value **0.25738143505962285** is returned.

.. code-block::

   select rand(3);
