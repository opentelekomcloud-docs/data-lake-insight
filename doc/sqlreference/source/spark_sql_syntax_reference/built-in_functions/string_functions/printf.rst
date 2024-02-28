:original_name: dli_spark_printf.html

.. _dli_spark_printf:

printf
======

This function is used to print the input in a specific format.

Syntax
------

.. code-block::

   printf(String format, Obj... args)

Parameters
----------

.. table:: **Table 1** Parameters

   ========= ========= ====== ======================
   Parameter Mandatory Type   Description
   ========= ========= ====== ======================
   format    Yes       STRING Output format
   Obj       No        STRING Other input parameters
   ========= ========= ====== ======================

Return Values
-------------

The return value is of the STRING type.

The value is returned after the parameters that filled in **Obj** are specified for **format**.

Example Code
------------

The string **name: Zhang San, age: 20, gender: female, place of origin: city 1** is returned.

.. code-block::

   SELECT printf('Name: %s, Age: %d, Gender: %s, Place of origin: %s', "Zhang San", 20, "Female", "City 1");
