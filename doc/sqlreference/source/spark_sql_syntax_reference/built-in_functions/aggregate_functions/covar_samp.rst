:original_name: dli_spark_covar_samp.html

.. _dli_spark_covar_samp:

covar_samp
==========

This function is used to return the sample covariance between two columns of numerical values.

Syntax
------

.. code-block::

   covar_samp(col1, col2)

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

-  Calculates the sample covariance between the inventory (items) and the price of all offerings. An example command is as follows:

   .. code-block::

      select covar_samp(items,price) from warehouse;

   The command output is as follows:

   .. code-block::

      _c0
      1.242355

-  When used with **group by**, it groups all offerings by warehouse (warehouseId) and returns the sample covariance between the inventory (items) and the price of offerings within each group. An example command is as follows:

   .. code-block::

      select warehouseId, covar_samp(items,price) from warehouse group by warehouseId;

   The command output is as follows:

   .. code-block::

      warehouseId _c1
      city1    1.03124
      city2    1.03344
      city3    1.33425
