:original_name: dli_08_0092.html

.. _dli_08_0092:

Viewing Table Properties
========================

Function
--------

Check the properties of a table.

Syntax
------

::

   SHOW TBLPROPERTIES table_name [('property_name')];

Keywords
--------

TBLPROPERTIES: This statement allows you to add a **key/value** property to a table.

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------------------------+---------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                 |
   +===================================+=============================================================================================+
   | table_name                        | Table name                                                                                  |
   +-----------------------------------+---------------------------------------------------------------------------------------------+
   | property_name                     | -  If this parameter is not specified, all properties and their values are returned.        |
   |                                   | -  If a property name is specified, only the specified property and its value are returned. |
   +-----------------------------------+---------------------------------------------------------------------------------------------+

Precautions
-----------

**property_name** is case sensitive. You cannot specify multiple **property_name** attributes at the same time. Otherwise, an error occurs.

Example
-------

To return the value of **property_key1** in the test table, run the following statement:

::

   SHOW TBLPROPERTIES test ('property_key1');
