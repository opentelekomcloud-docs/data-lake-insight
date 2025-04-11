:original_name: dli_08_15010.html

.. _dli_08_15010:

CREATE FUNCTION
===============

Function
--------

To create a catalog function with a catalog and a database namespace, you will need to specify an identifier. You can specify a language tag. You cannot register the function if a function with the same name has already been registered in the catalog. If the language tag is **JAVA** or **SCALA**, the identifier is the fully qualified name of the UDF implementation class.

For details about how to create a UDF, see :ref:`UDFs <dli_08_15082>`.

Syntax
------

.. code-block::

   CREATE [TEMPORARY|TEMPORARY SYSTEM] FUNCTION
     [IF NOT EXISTS] [[catalog_name.]db_name.]function_name
     AS identifier [LANGUAGE JAVA|SCALA]

Description
-----------

**TEMPORARY**

Create a temporary catalog function with catalogs and database namespaces and overwrite the original catalog function.

**TEMPORARY SYSTEM**

Create a temporary system catalog function without database namespaces and overwrite the system's built-in functions.

**IF NOT EXISTS**

If the function already exists, nothing happens.

**LANGUAGE JAVA|SCALA**

The language tag is used to instruct Flink runtime how to execute the function. Currently, only **JAVA** and **SCALA** language tags are supported, and the default language for a function is **JAVA**.

Example
-------

Create a function named **STRINGBACK**.

.. code-block::

   create function STRINGBACK as 'com.dli.StringBack'
