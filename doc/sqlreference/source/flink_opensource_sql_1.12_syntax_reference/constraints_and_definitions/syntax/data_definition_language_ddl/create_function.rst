:original_name: dli_08_0377.html

.. _dli_08_0377:

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

For details about how to create a user-defined function, see :ref:`User-Defined Functions (UDFs) <dli_08_0425>`.

Description
-----------

**IF NOT EXISTS**

If the function already exists, nothing happens.

**LANGUAGE JAVA|SCALA**

The language tag is used to instruct Flink runtime how to execute the function. Currently, only **JAVA** and **SCALA** language tags are supported, the default language for a function is **JAVA**.

Example
-------

Create a function named **STRINGBACK**.

.. code-block::

   create function STRINGBACK as 'com.dli.StringBack'
