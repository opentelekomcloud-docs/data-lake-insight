:original_name: dli_08_15051.html

.. _dli_08_15051:

Hive Dimension Table
====================

Function
--------

You can use Hive tables as temporal tables and associate them through temporal joins. For more information on temporal joins, refer to `temporal join <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/dev/table/sql/queries/joins/#temporal-joins>`__.

Flink supports processing-time temporal joins with Hive tables, which always join the latest version of the temporal table. Flink supports temporary joins with both partitioned and non-partitioned Hive tables. For partitioned tables, Flink automatically tracks the latest partition of the Hive table. For details, see `Apache Flink Hive Read & Write <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/connectors/table/hive/hive_read_write/>`__.

Caveats
-------

-  Currently, Flink does not support event-time temporal joins with Hive tables.
-  The "Temporal Join The Latest Partition" feature is only supported in Flink STREAMING mode.

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  For details about how to use data types, see :ref:`Format <dli_08_15014>`.
-  Flink 1.15 currently only supports creating OBS tables and DLI lakehouse tables using Hive syntax, which is supported by Hive dialect DDL statements.

   -  To create an OBS table using Hive syntax:

      -  For the default dialect, set **hive.is-external** to **true** in the with properties.
      -  For the Hive dialect, use the **EXTERNAL** keyword in the create table statement.

   -  To create a DLI lakehouse table using Hive syntax:

      -  For the Hive dialect, add **'is_lakehouse'='true'** to the table properties.

-  When creating a Flink OpenSource SQL job, enable checkpointing in the job editing interface.

Syntax Format and Parameter Description
---------------------------------------

For details, see the syntax format and parameter description in :ref:`Hive Source Table <dli_08_15049>`.
