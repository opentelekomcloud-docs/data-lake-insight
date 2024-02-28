:original_name: dli_spark_length.html

.. _dli_spark_length:

length
======

This function is used to return the length of a string.

Similar function: :ref:`lengthb <dli_spark_lengthb>`. The **lengthb** function is used to return the length of string **str** in bytes and return a value of the STRING type.

Syntax
------

.. code-block::

   length(string <str>)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                              |
   +=================+=================+=================+==========================================================================================================================================================================================+
   | str             | Yes             | STRING          | Target string to be searched for                                                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                          |
   |                 |                 |                 | If the value is of the BIGINT, DOUBLE, DECIMAL, or DATETIME type, the value is implicitly converted to the STRING type for calculation. For other types of values, an error is reported. |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   -  If the value of **str** is not of the STRING, BIGINT, DOUBLE, DECIMAL, or DATETIME type, an error is reported.
   -  If the value of **str** is **NULL**, **NULL** is returned.

Example Code
------------

-  Calculate the length of string abc. An example command is as follows:

   The value **3** is returned.

   .. code-block::

      select length('abc');

-  The value of the input parameter is **NULL**. An example command is as follows:

   The value **NULL** is returned.

   .. code-block::

      select length(null);
