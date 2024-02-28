:original_name: dli_spark_concat.html

.. _dli_spark_concat:

concat
======

This function is used to concatenate arrays or strings.

Syntax
------

If multiple arrays are used as the input, all elements in the arrays are connected to generate a new array.

.. code-block::

   concat(array<T> <a>, array<T> <b>[,...])

If multiple strings are used as the input, the strings are connected to generate a new string.

.. code-block::

   concat(string <str1>, string <str2>[,...])

Parameters
----------

-  Using arrays as the input

   .. table:: **Table 1** Parameters

      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                                                                                                        |
      +=================+=================+=================+====================================================================================================================================================================+
      | a, b            | Yes             | STRING          | Array                                                                                                                                                              |
      |                 |                 |                 |                                                                                                                                                                    |
      |                 |                 |                 | In array<T>, **T** indicates the data type of the elements in the array. The elements in the array can be of any type.                                             |
      |                 |                 |                 |                                                                                                                                                                    |
      |                 |                 |                 | The data types of elements in arrays a and b must be the same. If the values of the elements in an array are **NULL**, the elements are involved in the operation. |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  Using strings as the input

   .. table:: **Table 2** Parameters

      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                        |
      +=================+=================+=================+====================================================================================================================================================================================================================+
      | str1, str2      | Yes             | STRING          | String                                                                                                                                                                                                             |
      |                 |                 |                 |                                                                                                                                                                                                                    |
      |                 |                 |                 | If the value of the input parameter is of the BIGINT, DOUBLE, DECIMAL, or DATETIME type, the value is automatically converted to the STRING type for calculation. For other types of values, an error is reported. |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the ARRAY or STRING type.

.. note::

   -  If the return value is of the ARRAY type and any input array is **NULL**, **NULL** is returned.
   -  If the return value is of the STRING type and there is no parameter or any parameter is **NULL**, **NULL** is returned.

Example Code
------------

-  Connect the array (1, 2) and array (2, -2). An example command is as follows:

   The value **[1, 2, 2, -2]** is returned.

   .. code-block::

      select concat(array(1, 2), array(2, -2));

-  Any array is **NULL**. An example command is as follows:

   The value **NULL** is returned.

   .. code-block::

      select concat(array(10, 20), null);

-  Connect strings ABC and DEF. An example command is as follows:

   The value **ABCDEF** is returned.

   .. code-block::

      select concat('ABC','DEF');

-  The input is empty. An example command is as follows:

   The value **NULL** is returned.

   .. code-block::

      select concat();

-  The value of any string is **NULL**. An example command is as follows:

   The value **NULL** is returned.

   .. code-block::

      select concat('abc', 'def', null);
