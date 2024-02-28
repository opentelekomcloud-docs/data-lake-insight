:original_name: dli_spark_ascii.html

.. _dli_spark_ascii:

ascii
=====

This function is used to return the ASCII code of the first character in str.

Syntax
------

.. code-block::

   ascii(string <str>)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                |
   +=================+=================+=================+============================================================================================================================================+
   | str             | Yes             | STRING          | If the value is of the BIGINT, DOUBLE, DECIMAL, or DATETIME type, the value is automatically converted to the STRING type for calculation. |
   |                 |                 |                 |                                                                                                                                            |
   |                 |                 |                 | Example: **ABC**                                                                                                                           |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   -  If the value of **str** is not of the STRING, BIGINT, DOUBLE, DECIMAL, or DATETIME type, an error is reported.
   -  If the value of **str** is **NULL**, **NULL** is returned.

Example Code
------------

-  Returns the ASCII code of the first character in string ABC. An example command is as follows:

   The value **97** is returned.

   .. code-block::

      select ascii('ABC');

-  The value of the input parameter is **NULL**. An example command is as follows:

   The value **NULL** is returned.

   .. code-block::

      select ascii(null);
