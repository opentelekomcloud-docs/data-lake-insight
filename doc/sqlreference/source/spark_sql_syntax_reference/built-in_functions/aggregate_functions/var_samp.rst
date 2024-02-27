:original_name: dli_spark_war_samp.html

.. _dli_spark_war_samp:

var_samp
========

This function is used to return the sample variance of a specified column.

Syntax
------

.. code-block::

   var_samp(col)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------------+-----------------------+------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                |
   +=======================+=======================+============================================================+
   | col                   | Yes                   | Columns with a data type of numeric.                       |
   |                       |                       |                                                            |
   |                       |                       | If the values are of any other type, **NULL** is returned. |
   +-----------------------+-----------------------+------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

Example Code
------------

-  Calculates the sample variance of all offering inventories (items). An example command is as follows:

   .. code-block::

      select var_samp(items) from warehouse;

   The command output is as follows:

   .. code-block::

      _c0
      294.342355

-  When used with **group by**, it groups all offerings by warehouse (warehourseId) and returns the sample variance of the offering inventory (items) in the same group. An example command is as follows:

   .. code-block::

      select warehourseId, var_samp(items) from warehourse group by warehourseId;

   The command output is as follows:

   .. code-block::

      warehouseId _c1
      city1    18.23124
      city2    16.23344
      city3    11.43425
