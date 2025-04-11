:original_name: dli_spark_corr.html

.. _dli_spark_corr:

corr
====

This function is used to return the correlation coefficient between two columns of numerical values.

Syntax
------

.. code-block::

   corr(col1, col2)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type                                                      | Description                                                                                     |
   +===========+===========+===========================================================+=================================================================================================+
   | col1      | Yes       | DOUBLE, BIGINT, INT, SMALLINT, TINYINT, FLOAT, or DECIMAL | Columns with a data type of numeric. If the value is of any other type, **NULL** is returned.   |
   +-----------+-----------+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------+
   | col2      | Yes       | DOUBLE, BIGINT, INT, SMALLINT, TINYINT, FLOAT, or DECIMAL | Columns with a data type of numeric. If the values are of any other type, **NULL** is returned. |
   +-----------+-----------+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

Example Code
------------

-  Calculates the correlation coefficient between the inventory (items) and the price of all products. An example command is as follows:

   .. code-block::

      select corr(items,price) from warehouse;

   The command output is as follows:

   .. code-block::

      _c0
      1.242355

-  When used with **group by**, it groups all offerings by warehouse (warehouseId) and returns the correlation coefficient between the inventory (items) and the price of offerings within each group. An example command is as follows:

   .. code-block::

      select warehouseId, corr(items,price) from warehouse group by warehouseId;

   The command output is as follows:

   .. code-block::

      warehouseId _c1
      city1   0.43124
      city2   0.53344
      city3   0.73425
