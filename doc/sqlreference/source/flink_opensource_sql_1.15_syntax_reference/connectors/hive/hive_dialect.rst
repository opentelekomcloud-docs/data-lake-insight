:original_name: dli_08_15048.html

.. _dli_08_15048:

Hive Dialect
============

Introduction
------------

Starting from 1.11.0, Flink allows users to write SQL statements in Hive syntax when Hive dialect is used. By providing compatibility with Hive syntax, we aim to improve the interoperability with Hive and reduce the scenarios when users need to switch between Flink and Hive in order to execute different statements. For details, see `Apache Flink Hive Dialect <https://nightlies.apache.org/flink/flink-docs-release-1.11/dev/table/hive/hive_dialect.html>`__.

Function
--------

Flink currently supports two SQL dialects: default and hive. You need to switch to Hive dialect before you can write in Hive syntax. The following describes how to set dialect with SQL Client.

Also notice that you can dynamically switch dialect for each statement you execute. There's no need to restart a session to use a different dialect.

Syntax
------

SQL dialect can be specified via the **table.sql-dialect** property.

::

   set table.sql-dialect=hive;

Caveats
-------

-  Hive dialect should only be used to process Hive meta objects, and requires the current catalog to be a `HiveCatalog <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/connectors/table/hive/hive_catalog/>`__.

-  Hive dialect only supports 2-part identifiers, so you can not specify catalog for an identifier. Refer to `Apache Flink Hive Read & Write <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/hive/hive_read_write/>`__ for more details.

   While all Hive versions support the same syntax, whether a specific feature is available still depends on the `Hive version <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/hive/overview/#supported-hive-versions>`__ you use.

   For example, updating database location is only supported in Hive-2.4.0 or later.

-  Use `HiveModule <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/connectors/table/hive/hive_functions/#use-hive-built-in-functions-via-hivemodule>`__ to run DML and DQL.

-  Since Flink 1.15 you need to swap flink-table-planner-loader located in **/lib** with **flink-table-planner_2.12** located in **/opt** to avoid the following exception. Please see `FLINK-25128 <https://issues.apache.org/jira/browse/FLINK-25128>`__ for more details.
