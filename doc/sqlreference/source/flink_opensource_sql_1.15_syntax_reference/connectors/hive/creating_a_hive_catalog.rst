:original_name: dli_08_15047.html

.. _dli_08_15047:

Creating a Hive Catalog
=======================

Introduction
------------

Catalogs provide metadata, such as databases, tables, partitions, views, and functions and information needed to access data stored in a database or other external systems.

One of the most crucial aspects of data processing is managing metadata. It may be transient metadata like temporary tables, or UDFs registered against the table environment. Or permanent metadata, like that in a Hive Metastore. Catalogs provide a unified API for managing metadata and making it accessible from the Table API and SQL Queries. For details, see `Apache Flink Catalogs <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/dev/table/catalogs/>`__.

Function
--------

The HiveCatalog serves two purposes; as persistent storage for pure Flink metadata, and as an interface for reading and writing existing Hive metadata.

Flink's `Hive documentation <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/connectors/table/hive/overview/>`__ provides full details on setting up the catalog and interfacing with an existing Hive installation. For details, see `Apache Flink Hive Catalog <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/hive/hive_catalog/>`__.

HiveCatalog can be used to handle two kinds of tables: Hive-compatible tables and generic tables.

-  Hive-compatible tables are those stored in a Hive-compatible way, in terms of both metadata and data in the storage layer. Therefore, Hive-compatible tables created via Flink can be queried from Hive side.
-  Generic tables, on the other hand, are specific to Flink. When creating generic tables with HiveCatalog, we're just using HMS to persist the metadata. While these tables are visible to Hive, it is unlikely Hive is able to understand the metadata. And therefore using such tables in Hive leads to undefined behavior.

You are advised to switch to Hive dialect to create Hive-compatible tables. If you want to create Hive-compatible tables with default dialect, make sure to set **'connector'='hive'** in your table properties, otherwise a table is considered generic by default in HiveCatalog. Note that the connector property is not required if you use Hive dialect. Refer to :ref:`Hive Dialect <dli_08_15048>`.

Caveats
-------

-  Warning: The Hive Metastore stores all meta-object names in lower case.
-  If a directory with the same name already exists, an exception is thrown.
-  To use Hudi tables, you need to use the Hudi catalog, which is not compatible with the Hive catalog.
-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.

Syntax
------

.. code-block::

   CREATE CATALOG myhive
   WITH (
       'type' = 'hive',
       'default-database' = 'default',
       'hive-conf-dir' = '/opt/flink/conf'
   );

   USE CATALOG myhive;

Parameter Description
---------------------

.. table:: **Table 1** Parameters

   +------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------+
   | Parameter        | Mandatory   | Default Value | Data Type   | Description                                                                                |
   +==================+=============+===============+=============+============================================================================================+
   | type             | Yes         | None          | String      | Catalog type. Set this parameter to **hive** when creating a HiveCatalog.                  |
   +------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------+
   | hive-conf-dir    | Yes         | None          | String      | This refers to the URI that points to the directory containing the **hive-site.xml** file. |
   |                  |             |               |             |                                                                                            |
   |                  |             |               |             | The value is fixed at **'hive-conf-dir' = '/opt/flink/conf'**.                             |
   +------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------+
   | default-database | No          | default       | String      | When a catalog is set as the current catalog, the default current database is used.        |
   +------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------+

Supported Types
---------------

The HiveCatalog supports universal tables for all Flink types.

For Hive-compatible tables, the HiveCatalog needs to map Flink data types to their corresponding Hive types.

.. table:: **Table 2** Data type mapping

   ============== ==============
   Flink SQL Type Hive Data Type
   ============== ==============
   CHAR(p)        CHAR(p)
   VARCHAR(p)     VARCHAR(p)
   STRING         STRING
   BOOLEAN        BOOLEAN
   TINYINT        TINYINT
   SMALLINT       SMALLINT
   INT            INT
   BIGINT         LONG
   FLOAT          FLOAT
   DOUBLE         DOUBLE
   DECIMAL(p, s)  DECIMAL(p, s)
   DATE           DATE
   TIMESTAMP(9)   TIMESTAMP
   BYTES          BINARY
   ARRAY<T>       LIST<T>
   MAP            MAP
   ROW            STRUCT
   ============== ==============

.. note::

   -  The maximum length for Hive's CHAR(p) is 255.
   -  The maximum length for Hive's VARCHAR(p) is 65535.
   -  Hive's MAP only supports primitive types as keys, while Flink's MAP can be any data type.
   -  Hive does not support UNION types.
   -  Hive's TIMESTAMP always has a precision of 9 and does not support other precisions. However, Hive UDFs can handle TIMESTAMP values with precision <= 9.
   -  Hive does not support Flink's **TIMESTAMP_WITH_TIME_ZONE**, **TIMESTAMP_WITH_LOCAL_TIME_ZONE**, and **MULTISET**.
   -  Flink's INTERVAL type cannot yet be mapped to Hive's INTERVAL type.

Example
-------

#. Create a catalog named **myhive** in the Flink OpenSource SQL job and use it to manage metadata.

   .. code-block::

      CREATE CATALOG myhive WITH (
          'type' = 'hive'
          ,'hive-conf-dir' = '/opt/flink/conf'
      );

      USE CATALOG myhive;

      create table dataGenSource(
        user_id string,
        amount int
      ) with (
        'connector' = 'datagen',
        'rows-per-second' = '1', --Generates a piece of data per second.
        'fields.user_id.kind' = 'random', --Specifies a random generator for the user_id field.
        'fields.user_id.length' = '3' --Limits the length of user_id to 3.
      );

      create table printSink(
        user_id string,
        amount int
      ) with (
        'connector' = 'print'
      );

      insert into printSink select * from dataGenSource;

#. Check if the **dataGenSource** and **printSink** tables exist in the **default** database.

   .. note::

      The Hive Metastore stores all meta-object names in lower case.

#. Create a new Flink OpenSource SQL job using the metadata in the catalog named **myhive**.

   .. code-block::

      CREATE CATALOG myhive WITH (
          'type' = 'hive'
          ,'hive-conf-dir' = '/opt/flink/conf'
      );

      USE CATALOG myhive;

      insert into printSink select * from dataGenSource;
