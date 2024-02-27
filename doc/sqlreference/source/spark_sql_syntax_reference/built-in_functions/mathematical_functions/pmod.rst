:original_name: dli_spark_pmod.html

.. _dli_spark_pmod:

pmod
====

This function is used to return the positive value of the remainder after division of **x** by **y**.

Syntax
------

pmod(INT a, INT b)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+------------------------------------+-----------------------------------------------+
   | Parameter | Mandatory | Type                               | Description                                   |
   +===========+===========+====================================+===============================================+
   | a         | Yes       | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string. |
   +-----------+-----------+------------------------------------+-----------------------------------------------+
   | b         | Yes       | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string. |
   +-----------+-----------+------------------------------------+-----------------------------------------------+

Return Values
-------------

The return value is of the DECIMAL or INT type.

.. note::

   -  If the value of **a** or **b** is **NULL**, **NULL** is returned.
   -  If the value of **b** is **0**, **NULL** is returned.

Example Code
------------

The value **2** is returned.

.. code-block::

   select pmod(2,5);

The value **3** is returned.

.. code-block::

   select pmod (-2,5) (parse: -2=5* (-1)...3);

The value **NULL** is returned.

.. code-block::

   select pmod(5,0);

The value **1** is returned.

.. code-block::

   select pmod(5,2);

The value **0.877** is returned.

.. code-block::

   select pmod(5.123,2.123);
