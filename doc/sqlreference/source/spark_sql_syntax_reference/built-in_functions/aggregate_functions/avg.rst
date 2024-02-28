:original_name: dli_spark_avg.html

.. _dli_spark_avg:

avg
===

This function is used to return the average value.

Syntax
------

.. code-block::

   avg(col), avg(DISTINCT col)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------+-----------+----------------+--------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type           | Description                                                                                |
   +===========+===========+================+============================================================================================+
   | col       | Yes       | All data types | The value can be of any data type and can be converted to the DOUBLE type for calculation. |
   +-----------+-----------+----------------+--------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   If the value of **col** is **NULL**, the column is not involved in calculation.

Example Code
------------

-  Calculates the average number of items across all warehouses. An example command is as follows:

   .. code-block::

      select avg(items) from warehouse;

   The command output is as follows:

   .. code-block::

      _c0
      100.0

-  Calculates the average inventory of all items in each warehouse when used with **group by**. An example command is as follows:

   .. code-block::

      select warehourseId, avg(items) from warehourse group by warehourseId;

   The command output is as follows:

   .. code-block::

      warehouseId _c1
      city1    155
      city2    101
      city3    194
