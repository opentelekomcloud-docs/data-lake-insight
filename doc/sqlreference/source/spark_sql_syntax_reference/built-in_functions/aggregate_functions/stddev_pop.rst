:original_name: dli_spark_stddev_pop.html

.. _dli_spark_stddev_pop:

stddev_pop
==========

This function is used to return the deviation of a specified column.

Syntax
------

.. code-block::

   stddev_pop(col)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------+-----------+-----------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Description                                                                                   |
   +===========+===========+===============================================================================================+
   | col       | Yes       | Columns with a data type of numeric. If the value is of any other type, **NULL** is returned. |
   +-----------+-----------+-----------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

Example Code
------------

-  Calculates the deviation of all offering inventories (items). An example command is as follows:

   .. code-block::

      select stddev_pop(items) from warehouse;

   The command output is as follows:

   .. code-block::

      _c0
      1.342355

-  When used with **group by**, it groups all offerings by warehouse (warehouseId) and returns the deviation of the offering inventory (items) in the same group. An example command is as follows:

   .. code-block::

      select warehouseId, stddev_pop(items) from warehouse group by warehouseId;

   The command output is as follows:

   .. code-block::

      warehouseId _c1
      city1    1.23124
      city2    1.23344
      city3    1.43425
