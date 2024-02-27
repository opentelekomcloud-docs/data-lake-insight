:original_name: dli_spark_positive.html

.. _dli_spark_positive:

positive
========

This function is used to return the value of **a**.

Syntax
------

.. code-block::

   positive(INT a)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------+-----------+------------------------------------+-----------------------------------------------+
   | Parameter | Mandatory | Type                               | Description                                   |
   +===========+===========+====================================+===============================================+
   | a         | Yes       | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string. |
   +-----------+-----------+------------------------------------+-----------------------------------------------+

Return Values
-------------

The return value is of the DECIMAL, DOUBLE, or INT type.

.. note::

   If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **3** is returned.

.. code-block::

   SELECT positive(3);

The value **-3** is returned.

.. code-block::

   SELECT positive(-3);

The value **123** is returned.

.. code-block::

   SELECT positive('123');
