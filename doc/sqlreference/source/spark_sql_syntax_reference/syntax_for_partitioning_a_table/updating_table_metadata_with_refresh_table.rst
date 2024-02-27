:original_name: dli_08_0359.html

.. _dli_08_0359:

Updating Table Metadata with REFRESH TABLE
==========================================

Function
--------

Spark caches Parquet metadata to improve performance. If you update a Parquet table, the cached metadata is not updated. Spark SQL cannot find the newly inserted data and an error similar with the following is reported:

.. code-block::

   DLI.0002: FileNotFoundException: getFileStatus on  error message

You can use REFRESH TABLE to solve this problem. REFRESH TABLE reorganizes files of a partition and reuses the original table metadata information to detect the increase or decrease of table fields. This statement is mainly used when the metadata in a table is not modified but the table data is modified.

Syntax
------

::

   REFRESH TABLE [db_name.]table_name;

Keywords
--------

None

Parameters
----------

.. table:: **Table 1** Parameters

   +------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Description                                                                                                                                                                                                                                                                                 |
   +============+=============================================================================================================================================================================================================================================================================================+
   | db_name    | Database name that contains letters, digits, and underscores (_). It cannot contain only digits or start with an underscore (_).                                                                                                                                                            |
   +------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name | Table name of a database that contains letters, digits, and underscores (_). It cannot contain only digits or start with an underscore (_). The matching rule is **^(?!_)(?![0-9]+$)[A-Za-z0-9_$]*$**. If special characters are required, use single quotation marks ('') to enclose them. |
   +------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

None

Example
-------

Update metadata of the **test** table.

::

   REFRESH TABLE test;
