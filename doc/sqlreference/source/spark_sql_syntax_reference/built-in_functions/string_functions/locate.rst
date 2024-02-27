:original_name: dli_spark_locate.html

.. _dli_spark_locate:

locate
======

This function is used to return the position of substr in str. You can specify the starting position of your search using "start_pos," which starts from 1.

Syntax
------

.. code-block::

   locate(string <substr>, string <str>[, bigint <start_pos>])

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                              |
   +=================+=================+=================+==========================================================================================================================================================================================+
   | str             | Yes             | STRING          | Target string to be searched for                                                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                          |
   |                 |                 |                 | If the value is of the BIGINT, DOUBLE, DECIMAL, or DATETIME type, the value is implicitly converted to the STRING type for calculation. For other types of values, an error is reported. |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | substr          | Yes             | STRING          | Substring to be matched                                                                                                                                                                  |
   |                 |                 |                 |                                                                                                                                                                                          |
   |                 |                 |                 | If the value is of the BIGINT, DOUBLE, DECIMAL, or DATETIME type, the value is implicitly converted to the STRING type for calculation. For other types of values, an error is reported. |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | start_pos       | No              | BIGINT          | Start position for the search                                                                                                                                                            |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   -  If substr cannot be matched in str, **0** is returned.
   -  If the value of **str** or **substr** is **NULL**, **NULL** is returned.
   -  If the value of **start_pos** is **NULL**, **0** is returned.

Example Code
------------

-  Search for the position of string **ab** in string **abhiab**. An example command is as follows:

   The value **1** is returned.

   .. code-block::

      select locate('ab', 'abhiab');

   The value **5** is returned.

   .. code-block::

      select locate('ab', 'abhiab', 2);

   The value **0** is returned.

   .. code-block::

      select locate('ab', 'abhiab', null);

-  Search for the position of string **hi** in string **hanmeimei and lilei**. An example command is as follows:

   The value **0** is returned.

   .. code-block::

      select locate('hi', 'hanmeimei and lilei');
