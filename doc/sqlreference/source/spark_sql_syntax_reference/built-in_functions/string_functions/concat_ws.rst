:original_name: dli_spark_concat_ws.html

.. _dli_spark_concat_ws:

concat_ws
=========

This function is used to return a string concatenated from multiple input strings that are separated by specified separators.

Syntax
------

.. code-block::

   concat_ws(string <separator>, string <str1>, string <str2>[,...])

or

.. code-block::

   concat_ws(string <separator>, array<string> <a>)

Returns the result of joining all the strings in the parameters or the elements in an array using a specified separator.

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                              |
   +=================+=================+=================+==========================================================================================================================================================================+
   | separator       | Yes             | STRING          | Separator of the STRING type                                                                                                                                             |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | str1, str2      | Yes             | STRING          | At least two strings must be specified.                                                                                                                                  |
   |                 |                 |                 |                                                                                                                                                                          |
   |                 |                 |                 | The value is of the STRING type. If the value is of the BIGINT, DECIMAL, DOUBLE, or DATETIME type, the value is implicitly converted to the STRING type for calculation. |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | a               | Yes             | ARRAY           | The elements in an array are of the STRING type.                                                                                                                         |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING or STRUCT type.

.. note::

   -  If the value of **str1** or **str2** is not of the STRING, BIGINT, DECIMAL, DOUBLE, or DATETIME type, an error is reported.
   -  If the value of the parameter (string to be concatenated) is **NULL**, the parameter is ignored.
   -  If there is no input parameter (string to be concatenated), **NULL** is returned.

Example Code
------------

-  Use a colon (:) to connect strings ABC and DEF. An example command is as follows:

   The value **ABC:DEF** is returned.

   .. code-block::

      select concat_ws(':','ABC','DEF');

-  The value of any input parameter is **NULL**. An example command is as follows:

   The value **avg:18** is returned.

   .. code-block::

      select concat_ws(':','avg',null,'18');

-  Use colons (:) to connect elements in the array ('name', 'lilei'). An example command is as follows:

   The value **name:lilei** is returned.

   .. code-block::

      select concat_ws(':',array('name', 'lilei'));
