:original_name: dli_08_0296.html

.. _dli_08_0296:

CREATE FUNCTION
===============

Syntax
------

.. code-block::

   CREATE FUNCTION
     [IF NOT EXISTS] function_name
     AS identifier [LANGUAGE JAVA|SCALA]

Function
--------

Create a user-defined function.

Description
-----------

**IF NOT EXISTS**

If the function already exists, nothing happens.

**LANGUAGE JAVA|SCALA**

Language tag is used to instruct Flink runtime how to execute the function. Currently only **JAVA** and **SCALA** are supported, the default language for a function is **JAVA**.

Example
-------

Create a function named **STRINGBACK**.

.. code-block::

   create function STRINGBACK as 'com.dli.StringBack'
