:original_name: dli_spark_space.html

.. _dli_spark_space:

space
=====

This function is used to return a specified number of spaces.

Syntax
------

.. code-block::

   space(bigint <n>)

Parameters
----------

.. table:: **Table 1** Parameter

   ========= ========= ====== ================
   Parameter Mandatory Type   Description
   ========= ========= ====== ================
   n         Yes       BIGINT Number of spaces
   ========= ========= ====== ================

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **n** is empty, an error is reported.
   -  If the value of **n** is **NULL**, **NULL** is returned.

Example Code
------------

The value **6** is returned.

.. code-block::

   select length(space(6));
