:original_name: dli_08_0281.html

.. _dli_08_0281:

Displaying Function Details
===========================

Function
--------

Displays information about a specified function.

Syntax
------

::

   DESCRIBE FUNCTION [EXTENDED] [db_name.] function_name;

Keywords
--------

EXTENDED: displays extended usage information.

Precautions
-----------

The metadata (implementation class and usage) of an existing function is returned. If the function does not exist, the system reports an error.

Example
-------

Displays information about the mergeBill function.

::

   DESCRIBE FUNCTION mergeBill;
