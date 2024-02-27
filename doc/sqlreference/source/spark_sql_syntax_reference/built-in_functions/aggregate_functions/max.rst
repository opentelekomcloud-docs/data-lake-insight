:original_name: dli_spark_max.html

.. _dli_spark_max:

max
===

This function is used to return the maximum value.

Syntax
------

.. code-block::

   max(col)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------+-----------+-------------------------+----------------------------------------------+
   | Parameter | Mandatory | Type                    | Description                                  |
   +===========+===========+=========================+==============================================+
   | col       | Yes       | Any type except BOOLEAN | The value can be of any type except BOOLEAN. |
   +-----------+-----------+-------------------------+----------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   The return type is the same as the type of **col**. The return rules are as follows:

   -  If the value of **col** is **NULL**, the row is not involved in calculation.
   -  If the value of **col** is of the BOOLEAN type, it cannot be used for calculation.

Example Code
------------

-  Calculates the maximum inventory (items) of all offerings. An example command is as follows:

   .. code-block::

      select max(items) from warehouse;

   The command output is as follows:

   .. code-block::

      _c0
      900

-  When used with **group by**, it returns the maximum inventory of each warehouse. An example command is as follows:

   .. code-block::

      select warehourseId, max(items) from warehouse group by warehourseId;

   The command output is as follows:

   .. code-block::

      warehouseId _c1
      city1    200
      city2    300
      city3    400
