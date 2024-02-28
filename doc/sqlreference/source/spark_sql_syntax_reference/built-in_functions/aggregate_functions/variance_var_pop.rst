:original_name: dli_spark_variance_var_pop.html

.. _dli_spark_variance_var_pop:

variance/var_pop
================

This function is used to return the variance of a column.

Syntax
------

.. code-block::

   variance(col),
   var_pop(col)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------------+-----------------------+----------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                              |
   +=======================+=======================+==========================================================+
   | col                   | Yes                   | Columns with a data type of numeric.                     |
   |                       |                       |                                                          |
   |                       |                       | If the value is of any other type, **NULL** is returned. |
   +-----------------------+-----------------------+----------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

Example Code
------------

-  Calculates the variance of all offering inventories (items). An example command is as follows:

   .. code-block::

      select variance(items) from warehouse;
      -- It is equivalent to the following statement:
      select var_pop(items) from warehouse;

   The command output is as follows:

   .. code-block::

      _c0
      203.42352

-  When used with **group by**, it groups all offerings by warehouse (warehourseId) and returns the variance of the offering inventory (items) in the same group. An example command is as follows:

   .. code-block::

      select warehourseId, variance(items) from warehourse group by warehourseId;
      -- It is equivalent to the following statement:
      select warehourseId, var_pop(items) from warehourse group by warehourseId;

   The command output is as follows:

   ::

      warehouseId _c1
      city1    19.23124
      city2    17.23344
      city3    12.43425
