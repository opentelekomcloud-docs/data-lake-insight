:original_name: dli_08_15040.html

.. _dli_08_15040:

OBS Source Table
================

Function
--------

The file system connector can be used to read single files or entire directories into a single table.

When using a directory as the source path, there is no defined order of ingestion for the files inside the directory. For more information, see `FileSystem SQL Connector <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/connectors/table/filesystem/>`__.

Syntax
------

::

   CREATE TABLE sink_table (
      name string,
      num INT,
      p_day string,
      p_hour string
   ) partitioned by (p_day, p_hour) WITH (
      'connector' = 'filesystem',
      'path' = 'obs://*** ',
      'format' = 'parquet',
      'source.monitor-interval'=''
   );

Parameter Description
---------------------

-  **Directory watching**

   By default, the file system connector is bounded, that is it will scan the configured path once and then close itself.

   You can enable continuous directory watching by configuring the **source.monitor-interval** parameter:

   +-------------------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Key                     | Default Value   | Data Type       | Description                                                                                                                                                                   |
   +=========================+=================+=================+===============================================================================================================================================================================+
   | source.monitor-interval | None            | Duration        | The interval in which the source checks for new files. The interval must be greater than 0.                                                                                   |
   |                         |                 |                 |                                                                                                                                                                               |
   |                         |                 |                 | Each file is uniquely identified by its path, and will be processed once, as soon as it is discovered.                                                                        |
   |                         |                 |                 |                                                                                                                                                                               |
   |                         |                 |                 | The set of files already processed is kept in state during the whole lifecycle of the source, so it's persisted in checkpoints and savepoints together with the source state. |
   |                         |                 |                 |                                                                                                                                                                               |
   |                         |                 |                 | Shorter intervals mean that files are discovered more quickly, but also imply more frequent listing or directory traversal of the file system/object store.                   |
   |                         |                 |                 |                                                                                                                                                                               |
   |                         |                 |                 | If this config option is not set, the provided path will be scanned once, hence the source will be bounded.                                                                   |
   +-------------------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  **Available Metadata**

   The following connector metadata can be accessed as metadata columns in a table definition. All the metadata are read only.

   +------------------------+---------------------------+------------------------------------------------------------------------------+
   | Key                    | Data Type                 | Description                                                                  |
   +========================+===========================+==============================================================================+
   | file.path              | STRING NOT NULL           | Full path of the input file                                                  |
   +------------------------+---------------------------+------------------------------------------------------------------------------+
   | file.name              | STRING NOT NULL           | Name of the file, that is the farthest element from the root of the filepath |
   +------------------------+---------------------------+------------------------------------------------------------------------------+
   | file.size              | STRING NOT NULL           | Byte count of the file                                                       |
   +------------------------+---------------------------+------------------------------------------------------------------------------+
   | file.modification-time | TIMESTAMP_LTZ(3) NOT NULL | Modification time of the file                                                |
   +------------------------+---------------------------+------------------------------------------------------------------------------+

Example
-------

Read data from the OBS table as the data source and output it to the Print connector.

.. code-block::

   CREATE TABLE obs_source(
      name string,
      num INT,
      `file.path` STRING NOT NULL METADATA
   ) WITH (
      'connector' = 'filesystem',
      'path' = 'obs://demo/sink_parquent_obs',
      'format' = 'parquet',
      'source.monitor-interval'='1 h'
   );


   CREATE TABLE print (
      name string,
      num INT,
      path  STRING
   ) WITH (
      'connector' = 'print'
   );

   insert into print
   select * from obs_source;

Print result:

.. code-block::

   +I[0e72e, 841255524, /spark.db/sink_parquent_obs/compacted-part-fd4d4cc8-8b18-42d5-b522-9b524500fa23-0-0]
   +I[53524, -2032270969, /spark.db/sink_parquent_obs/compacted-part-fd4d4cc8-8b18-42d5-b522-9b524500fa23-0-0]
   +I[77225, 245599258, /spark.db/sink_parquent_obs/compacted-part-fd4d4cc8-8b18-42d5-b522-9b524500fa23-0-0]
   +I[fc202, -545621464, /spark.db/sink_parquent_obs/compacted-part-fd4d4cc8-8b18-42d5-b522-9b524500fa23-0-0]
   +I[07e9d, 1511139764, /spark.db/sink_parquent_obs/compacted-part-fd4d4cc8-8b18-42d5-b522-9b524500fa23-0-0]
   +I[4e48b, 278014413, /spark.db/sink_parquent_obs/compacted-part-fd4d4cc8-8b18-42d5-b522-9b524500fa23-0-0]
