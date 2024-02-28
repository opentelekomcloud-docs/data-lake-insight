:original_name: dli_spark_count.html

.. _dli_spark_count:

count
=====

This function is used to return the number of records.

Syntax
------

.. code-block::

   count([distinct|all] <colname>)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                     |
   +=======================+=======================+=================================================================================================================================================================+
   | distinct or all       | No                    | Determines whether duplicate records should be excluded during counting. **all** is used by default, indicating that all records will be included in the count. |
   |                       |                       |                                                                                                                                                                 |
   |                       |                       | If **distinct** is specified, only the number of unique values is counted.                                                                                      |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | colname               | Yes                   | The column value can be of any type.                                                                                                                            |
   |                       |                       |                                                                                                                                                                 |
   |                       |                       | The value can be **\***, that is, **count(*)**, indicating that the number of all rows is returned.                                                             |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   If the value of **colname** is **NULL**, the row is not involved in calculation.

Example Code
------------

-  Calculates the total number of records in the warehouse table. An example command is as follows:

   .. code-block::

      select count(*) from warehouse;

   The command output is as follows:

   .. code-block::

      _c0
      10

-  When used with **group by**, it groups all offerings by warehouse (warehouseId) and calculates the number of offerings in each warehouse (warehouseId). An example command is as follows:

   .. code-block::

      select warehouseId, count(*) from warehouse group by warehouseId;

   The command output is as follows:

   .. code-block::

      warehouseId _c1
      city1   6
      city2   5
      city3   6

   Example 3: Calculates the number of warehouses through distinct deduplication. An example command is as follows:

   .. code-block::

      select count(distinct warehouseId) from warehouse;

   The command output is as follows:

   .. code-block::

      _c0
       3
