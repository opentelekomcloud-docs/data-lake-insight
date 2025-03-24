:original_name: dli_spark_covar_pop.html

.. _dli_spark_covar_pop:

covar_pop
=========

This function is used to return the covariance between two columns of numerical values.

Syntax
------

.. code-block::

   covar_pop(col1, col2)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+-------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Description                                                                                     |
   +===========+===========+=================================================================================================+
   | col1      | Yes       | Columns with a data type of numeric. If the values are of any other type, **NULL** is returned. |
   +-----------+-----------+-------------------------------------------------------------------------------------------------+
   | col2      | Yes       | Columns with a data type of numeric. If the values are of any other type, **NULL** is returned. |
   +-----------+-----------+-------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

Example Code
------------

-  Calculates the covariance between the inventory (items) and the price of all offerings. An example command is as follows:

   .. code-block::

      select covar_pop(items, price) from warehouse;

   The command output is as follows:

   .. code-block::

      _c0
      1.242355

-  When used with **group by**, it groups all offerings by warehouse (warehouseId) and returns the covariance between the inventory (items) and the price of offerings within each group. An example command is as follows:

   .. code-block::

      select warehouseId, covar_pop(items, price) from warehouse group by warehouseId;

   The command output is as follows:

   .. code-block::

      warehouseId _c1
      city1    1.13124
      city2    1.13344
      city3    1.53425
