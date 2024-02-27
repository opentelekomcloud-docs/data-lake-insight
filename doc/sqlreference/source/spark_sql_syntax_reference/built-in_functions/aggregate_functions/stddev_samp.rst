:original_name: dli_spark_stddev_samp.html

.. _dli_spark_stddev_samp:

stddev_samp
===========

This function is used to return the sample deviation of a specified column.

Syntax
------

.. code-block::

   stddev_samp(col)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------+-----------+-------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Description                                                                                     |
   +===========+===========+=================================================================================================+
   | col       | Yes       | Columns with a data type of numeric. If the values are of any other type, **NULL** is returned. |
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

-  When used with **group by**, it groups all offerings by warehouse (warehourseId) and returns the sample covariance between the inventory (items) and the price of offerings within each group. An example command is as follows:

   .. code-block::

      select warehourseId, covar_samp(items,price) from warehourse group by warehourseId;

   The command output is as follows:

   .. code-block::

      warehouseId _c1
      city1    1.03124
      city2    1.03344
      city3    1.33425
