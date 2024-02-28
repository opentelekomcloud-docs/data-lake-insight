:original_name: dli_spark_aocs.html

.. _dli_spark_aocs:

acos
====

This function is used to return the arc cosine value of a given angle **a**.

Syntax
------

.. code-block::

   acos(DOUBLE a)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                               | Description                                                                                                       |
   +=================+=================+====================================+===================================================================================================================+
   | a               | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | The value range is [-1,1]. The value can be a float, integer, or string.                                          |
   |                 |                 |                                    |                                                                                                                   |
   |                 |                 |                                    | If the value is not of the DOUBLE type, the system will implicitly convert it to the DOUBLE type for calculation. |
   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type. The value ranges from 0 to Pi.

.. note::

   -  If the value of **a** is not within the range [-1,1], **NaN** is returned.
   -  If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **3.141592653589793** is returned.

.. code-block::

   select acos(-1);

The value **0** is returned.

.. code-block::

   select acos(1);

The value **NULL** is returned.

.. code-block::

   select acos(null);

The value **NAN** is returned.

.. code-block::

   select acos(10);
