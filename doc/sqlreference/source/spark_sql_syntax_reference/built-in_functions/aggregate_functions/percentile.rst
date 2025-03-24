:original_name: dli_spark_percentile.html

.. _dli_spark_percentile:

percentile
==========

This function is used to return the numerical value at a certain percentage point within a range of values.

Syntax
------

.. code-block::

   percentile(BIGINT col, p)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+-------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Description                                                                                     |
   +===========+===========+=================================================================================================+
   | col       | Yes       | Columns with a data type of numeric. If the values are of any other type, **NULL** is returned. |
   +-----------+-----------+-------------------------------------------------------------------------------------------------+
   | p         | Yes       | The value should be between 0 and 1. Otherwise, **NULL** is returned.                           |
   +-----------+-----------+-------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   The value should be between 0 and 1. Otherwise, **NULL** is returned.

Example Code
------------

-  Calculates the 0.5 percentile of all offering inventories (items). An example command is as follows:

   .. code-block::

      select percentile(items,0.5) from warehouse;

   The command output is as follows:

   .. code-block::

      +------------+
      | _c0        |
      +------------+
      | 500.6      |
      +------------+

-  When used with **group by**, it groups all offerings by warehouse (warehouseId) and returns the 0.5 percentile of the offering inventory (items) in the same group. An example command is as follows:

   .. code-block::

      select warehouseId, percentile(items, 0.5) from warehouse group by warehouseId;

   The command output is as follows:

   .. code-block::

      +------------+------------+
      | warehouseId| _c1        |
      +------------+------------+
      | city1    | 499.6      |
      | city2    | 354.8      |
      | city3    | 565.7      |
      +------------+------------+
