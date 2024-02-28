:original_name: dli_spark_split_part.html

.. _dli_spark_split_part:

split_part
==========

This function is used to split a specified string based on a specified separator and return a substring from the start to end position.

Syntax
------

.. code-block::

   split_part(string <str>, string <separator>, bigint <start>[, bigint <end>])

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                                                                                                                                                                                                                       |
   +===========+===========+========+===================================================================================================================================================================================================================================================================================+
   | str       | Yes       | STRING | String to be split                                                                                                                                                                                                                                                                |
   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | separator | Yes       | STRING | Constant of the STRING type. Separator used for splitting. The value can be a character or a string.                                                                                                                                                                              |
   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | start     | Yes       | STRING | Constant of the BIGINT type. The value must be greater than 0. Start number of the returned part, starting from 1.                                                                                                                                                                |
   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | end       | No        | BIGINT | Constant of the BIGINT type. The value must be greater than or equal to the value of **start**. End number of the returned part and can be omitted. The default value indicates that the value is the same as that of **start**, and the part specified by **start** is returned. |
   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **start** is greater than the actual number of segments after splitting, for example, if there are four segments after string splitting and the value of **start** is greater than 4, an empty string is returned.
   -  If **separator** does not exist in **str** and **start** is set to **1**, the entire **str** is returned. If the value of **str** is an empty string, an empty string is output.
   -  If the value of **separator** is an empty string, the original string **str** is returned.
   -  If the value of **end** is greater than the number of segments, the substring starting from **start** is returned.
   -  If the value of **str** is not of the STRING, BIGINT, DOUBLE, DECIMAL, or DATETIME type, an error is reported.
   -  If the value of **start** or **end** is not a constant of the BIGINT type, an error is reported.
   -  If the value of any parameter except **separator** is **NULL**, **NULL** is returned.

Example Code
------------

The value **aa** is returned.

.. code-block::

   select split_part('aa,bb,cc,dd', ',', 1);

The value **aa,bb** is returned.

.. code-block::

   select split_part('aa,bb,cc,dd', ',', 1, 2);

An empty string is returned.

.. code-block::

   select split_part('aa,bb,cc,dd', ',', 10);

The value **aa,bb,cc,dd** is returned.

.. code-block::

   select split_part('aa,bb,cc,dd', ':', 1);

An empty string is returned.

.. code-block::

   select split_part('aa,bb,cc,dd', ':', 2);

The value **aa,bb,cc,dd** is returned.

.. code-block::

   select split_part('aa,bb,cc,dd', '', 1);

The value **bb,cc,dd** is returned.

.. code-block::

   select split_part('aa,bb,cc,dd', ',', 2, 6);

The value **NULL** is returned.

.. code-block::

   select split_part('aa,bb,cc,dd', ',', null);
