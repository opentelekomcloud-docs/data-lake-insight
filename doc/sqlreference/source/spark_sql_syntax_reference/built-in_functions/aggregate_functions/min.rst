:original_name: dli_spark_min.html

.. _dli_spark_min:

min
===

This function is used to return the minimum value.

Syntax
------

.. code-block::

   min(col)

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

-  Calculates the minimum inventory (items) of all offerings. An example command is as follows:

   .. code-block::

      select min(items) from warehouse;

   The command output is as follows:

   .. code-block::

      _c0
      600

-  When used with **group by**, it returns the minimum inventory of each warehouse. An example command is as follows:

   .. code-block::

      select warehourseId, min(items) from warehouse group by warehourseId;

   The command output is as follows:

   .. code-block::

      warehouseId _c1
      city1      15
      city2      10
      city3      19
