:original_name: dli_spark_reverse.html

.. _dli_spark_reverse:

reverse
=======

This function is used to return a string in reverse order.

Syntax
------

.. code-block::

   reverse(string <str>)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                                                                             |
   +===========+===========+========+=========================================================================================================================================+
   | str       | Yes       | STRING | If the value is of the BIGINT, DOUBLE, DECIMAL, or DATETIME type, the value is implicitly converted to the STRING type for calculation. |
   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **str** is not of the STRING, BIGINT, DOUBLE, DECIMAL, or DATETIME type, an error is reported.
   -  If the value of **str** is **NULL**, **NULL** is returned.

Example Code
------------

The value **LQS krapS** is returned.

.. code-block::

   SELECT reverse('Spark SQL');

The value **[3,4,1,2]** is returned.

.. code-block::

   SELECT reverse(array(2, 1, 4, 3));
