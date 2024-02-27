:original_name: dli_spark_upper_ucase.html

.. _dli_spark_upper_ucase:

upper/ucase
===========

This function is used to convert all characters of a string to the upper case.

Syntax
------

.. code-block::

   upper(string A)

or

.. code-block::

   ucase(string A)

Parameters
----------

.. table:: **Table 1** Parameter

   ========= ========= ====== ===========================
   Parameter Mandatory Type   Description
   ========= ========= ====== ===========================
   A         Yes       STRING Text string to be converted
   ========= ========= ====== ===========================

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of the input parameter is not of the STRING, BIGINT, DOUBLE, DECIMAL, or DATETIME type, an error is reported.
   -  If the value of the input parameter is **NULL**, **NULL** is returned.

Example Code
------------

Converts lowercase characters in a string to the upper case. An example command is as follows:

The value **ABC** is returned.

.. code-block::

   select upper('abc');

The value of the input parameter is **NULL**. An example command is as follows:

The value **NULL** is returned.

.. code-block::

   select upper(null);
