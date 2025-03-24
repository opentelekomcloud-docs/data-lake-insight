:original_name: dli_spark_percentile_approx.html

.. _dli_spark_percentile_approx:

percentile_approx
=================

This function is used to approximate the pth percentile (including floating-point numbers) of a numeric column within a group.

Syntax
------

.. code-block::

   percentile_approx(DOUBLE col, p [, B])

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Description                                                                                                                                                                                                                                                             |
   +===========+===========+=========================================================================================================================================================================================================================================================================+
   | col       | Yes       | Columns with a data type of numeric. If the values are of any other type, **NULL** is returned.                                                                                                                                                                         |
   +-----------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | p         | Yes       | The value should be between 0 and 1. Otherwise, **NULL** is returned.                                                                                                                                                                                                   |
   +-----------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | B         | Yes       | The parameter B controls the accuracy of the approximation, with a higher value of B resulting in a higher level of approximation. The default value is **10000**. If the number of non-repeating values in the column is less than B, an exact percentile is returned. |
   +-----------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

Example Code
------------

-  Calculates the 0.5 percentile of all offering inventories (items), with an accuracy of 100. An example command is as follows:

   .. code-block::

      select PERCENTILE_APPROX(items,0.5,100) from warehouse;

   The command output is as follows:

   .. code-block::

      +------------+
      | _c0        |
      +------------+
      | 521        |
      +------------+

-  When used with **group by**, it groups all offerings by warehouse (warehouseId) and returns the 0.5 percentile of the offering inventory (items) in the same group, with an accuracy of 100. An example command is as follows:

   .. code-block::

      select warehouseId, PERCENTILE_APPROX(items, 0.5, 100) from warehouse group by warehouseId;

   The command output is as follows:

   .. code-block::

      +------------+------------+
      | warehouseId| _c1        |
      +------------+------------+
      | city1    | 499     |
      | city2    | 354     |
      | city3    | 565     |
      +------------+------------+
