:original_name: dli_08_0284.html

.. _dli_08_0284:

Deleting a Function
===================

Function
--------

This statement is used to delete functions.

Syntax
------

::

   DROP [TEMPORARY] FUNCTION [IF EXISTS] [db_name.] function_name;

Keywords
--------

-  TEMPORARY: Indicates whether the function to be deleted is a temporary function.
-  IF EXISTS: Used when the function to be deleted does not exist to avoid system errors.

Precautions
-----------

-  An existing function is deleted. If the function to be deleted does not exist, the system reports an error.
-  Only the HIVE syntax is supported.

Example
-------

The mergeBill function is deleted.

::

   DROP FUNCTION mergeBill;
