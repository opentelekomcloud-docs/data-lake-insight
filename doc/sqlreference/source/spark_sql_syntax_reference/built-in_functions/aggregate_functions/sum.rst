:original_name: dli_spark_sum.html

.. _dli_spark_sum:

sum
===

This function is used to calculate the total sum.

Syntax
------

.. code-block::

   sum(col),
   sum(DISTINCT col)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                      |
   +=======================+=======================+==================================================================================================+
   | col                   | Yes                   | The value can be of any data type and can be converted to the DOUBLE type for calculation.       |
   |                       |                       |                                                                                                  |
   |                       |                       | The value can be of the DOUBLE, DECIMAL, or BIGINT type.                                         |
   |                       |                       |                                                                                                  |
   |                       |                       | If the value is of the STRING type, the system implicitly converts it to DOUBLE for calculation. |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   If the value of **col** is **NULL**, the row is not involved in calculation.

Example Code
------------

-  Calculates the total number of offerings (items) in all warehouses. An example command is as follows:

   .. code-block::

      select sum(items) from warehouse;

   The command output is as follows:

   .. code-block::

      _c0
      55357

-  When used with **group by**, it groups all offerings by warehouse (warehouseId) and calculates the total number of offerings in all warehouses. An example command is as follows:

   .. code-block::

      select warehouseId, sum(items) from warehouse group by warehouseId;

   The command output is as follows:

   .. code-block::

      warehouseId| _c1
      city1    15500
      city2    10175
      city3    19400
