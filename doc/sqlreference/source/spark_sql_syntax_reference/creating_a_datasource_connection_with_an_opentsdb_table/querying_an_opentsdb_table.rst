:original_name: dli_08_0124.html

.. _dli_08_0124:

Querying an OpenTSDB Table
==========================

This **SELECT** command is used to query data in an OpenTSDB table.

.. note::

   -  If no metric exists in OpenTSDB, an error will be reported when the corresponding DLI table is queried.
   -  If the security mode is enabled, you need to set **conf:dli.sql.mrs.opentsdb.ssl.enabled** to **true** when connecting to OpenTSDB.

Syntax
------

::

   SELECT * FROM table_name LIMIT number;

Keyword
-------

LIMIT is used to limit the query results. Only INT type is supported by the **number** parameter.

Precautions
-----------

The table to be queried must exist. Otherwise, an error is reported.

Example
-------

Query data in the **opentsdb_table** table.

::

   SELECT * FROM opentsdb_table limit 100;
