:original_name: dli_08_0283.html

.. _dli_08_0283:

Creating a Function
===================

Function
--------

DLI allows you to create and use user-defined functions (UDF) and user-defined table functions (UDTF) in Spark jobs.

Syntax
------

::

   CREATE FUNCTION [db_name.]function_name AS class_name
     [USING resource,...]

   resource:
     : JAR file_uri

Or

::

   CREATE OR REPLACE FUNCTION [db_name.]function_name AS class_name
     [USING resource,...]

   resource:
     : JAR file_uri

Precautions
-----------

-  If a function with the same name exists in the database, the system reports an error.
-  Only the Hive syntax can be used to create functions.
-  If you specify the same class name for two UDFs, the functions conflict though the package names are different. Avoid this problem because it causes failure of job execution.

Keywords
--------

-  USING <resources>: resources to be loaded. It can be a list of JARs, files, or URIs.

Example
-------

Create the mergeBill function.

::

   CREATE FUNCTION mergeBill AS 'com.xxx.hiveudf.MergeBill'
     using jar 'obs://onlyci-7/udf/MergeBill.jar';
