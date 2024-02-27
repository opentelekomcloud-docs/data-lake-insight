:original_name: dli_spark_encode.html

.. _dli_spark_encode:

encode
======

This function is used to encode str in charset format.

Syntax
------

encode(string <str>, string <charset>)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                              |
   +=================+=================+=================+==========================================================================================================================================================================+
   | str             | Yes             | STRING          | At least two strings must be specified.                                                                                                                                  |
   |                 |                 |                 |                                                                                                                                                                          |
   |                 |                 |                 | The value is of the STRING type. If the value is of the BIGINT, DECIMAL, DOUBLE, or DATETIME type, the value is implicitly converted to the STRING type for calculation. |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | charset         | Yes             | STRING          | Encoding format.                                                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                          |
   |                 |                 |                 | The options are **UTF-8**, **UTF-16**, **UTF-16LE**, **UTF-16BE**, **ISO-8859-1**, and **US-ASCII**.                                                                     |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BINARY type.

.. note::

   If the value of **str** or **charset** is **NULL**, **NULL** is returned.

Example Code
------------

-  Encodes string abc in UTF-8 format. An example command is as follows:

   The value **abc** is returned.

   .. code-block::

      select encode("abc", "UTF-8");

-  The value of any input parameter is **NULL**. An example command is as follows:

   The return value is **NULL**.

   .. code-block::

      select encode("abc", null);
