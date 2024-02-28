:original_name: dli_spark_lengthb.html

.. _dli_spark_lengthb:

lengthb
=======

This function is used to return the length of a specified string in bytes.

Similar function: :ref:`length <dli_spark_length>`. The **length** function is used to return the length of a string and return a value of the BIGINT type.

Syntax
------

.. code-block::

   lengthb(string <str>)

Parameters
----------

.. table:: **Table 1** Parameter

   ========= ========= ====== ============
   Parameter Mandatory Type   Description
   ========= ========= ====== ============
   str       Yes       STRING Input string
   ========= ========= ====== ============

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **str** is not of the STRING, BIGINT, DOUBLE, DECIMAL, or DATETIME type, an error is reported.
   -  If the value of **str** is **NULL**, **NULL** is returned.

Example Code
------------

The value **5** is returned.

.. code-block::

   select lengthb('hello');

The value **NULL** is returned.

.. code-block::

   select lengthb(null);
