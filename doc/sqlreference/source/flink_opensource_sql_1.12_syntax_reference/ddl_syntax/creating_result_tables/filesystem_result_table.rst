:original_name: dli_08_0439.html

.. _dli_08_0439:

FileSystem Result Table
=======================

Function
--------

The FileSystem result (sink) table is used to export data to the HDFS or OBS file system. It is applicable to scenarios such as data dumping, big data analysis, data backup, and active, deep, or cold archiving.

Considering that the input stream can be unbounded, you can put the data in each bucket into **part** files of a limited size. Data can be written into a bucket based on time. For example, you can write data into a bucket every hour. This bucket contains the records received within one hour, and

data in the bucket directory is split into multiple **part** files. Each sink bucket that receives data contains at least one **part** file for each subtask. Other **part** files are created based on the configured rolling policy. For Row Formats, the default rolling policy is based on the **part** file size. You need to specify the maximum timeout period for opening a file and the timeout period for the inactive state after closing a file. Bulk Formats are rolled each time a checkpoint is created. You can add other rolling conditions based on size or time.

.. note::

   -  To use FileSink in STREAMING mode, you need to enable the checkpoint function. **Part** files are generated only when the checkpoint is successful. If the checkpoint function is not enabled, the files remain in the in-progress or pending state, and downstream systems cannot securely read the file data.
   -  The number recorded by the sink end operator is the number of checkpoints, not the actual volume of the sent data. For the actual volume, see the number recorded by the streaming-writer or StreamingFileWriter operator.

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
      'auto-compaction' = 'true'
   );

Usage
-----

-  **Rolling Policy**

   The Rolling Policy defines when a given in-progress part file will be closed and moved to the pending and later to finished state. Part files in the "finished" state are the ones that are ready for viewing and are guaranteed to contain valid data that will not be reverted in case of failure.

   In STREAMING mode, the Rolling Policy in combination with the checkpointing interval (pending files become finished on the next checkpoint) control how quickly part files become available for downstream readers and also the size and number of these parts. For details, see :ref:`Parameters <dli_08_0439__en-us_topic_0000001390352625_en-us_topic_0000001201521669_dli_08_0256_section4299113491>`.

-  **Part File Lifecycle**

   To use the output of the FileSink in downstream systems, we need to understand the naming and lifecycle of the output files produced.

   Part files can be in one of three states:

   -  **In-progress**: The part file that is currently being written to is in-progress.
   -  **Pending**: Closed (due to the specified rolling policy) in-progress files that are waiting to be committed.
   -  **Finished**: On successful checkpoints (STREAMING) or at the end of input (BATCH) pending files transition to **Finished**

   Only finished files are safe to read by downstream systems as those are guaranteed to not be modified later.

   By default, the file naming strategy is as follows:

   -  **In-progress / Pending**: part-<uid>-<partFileIndex>.inprogress.uid
   -  **Finished**: part-<uid>-<partFileIndex>

   **uid** is a random ID assigned to a subtask of the sink when the subtask is instantiated. This **uid** is not fault-tolerant so it is regenerated when the subtask recovers from a failure.

-  **Compaction**

   FileSink supports compaction of the pending files, which allows the application to have smaller checkpoint interval without generating a lot of small files.

   Once enabled, the compaction happens between the files become pending and get committed. The pending files will be first committed to temporary files whose path starts with a dot (.). Then these files will be compacted according to the strategy by the compactor specified by the users, and the new compacted pending files will be generated. Then these pending files will be emitted to the committer to be committed to the formal files. After that, the source files will be removed.

-  **Partitions**

   Filesystem sink supports the partitioning function. Partitions are generated based on the selected fields by using the **partitioned by** syntax. The following is an example:

   .. code-block::

      path
      └── datetime=2022-06-25
          └── hour=10
              ├── part-0.parquet
              ├── part-1.parquet
      └── datetime=2022-06-26
          └── hour=16
              ├── part-0.parquet
          └── hour=17
              ├── part-0.parquet

   Similar to files, partitions also need to be submitted to notify downstream applications that files in the partitions can be securely read. Filesystem sink provides multiple configuration submission policies.

.. _dli_08_0439__en-us_topic_0000001390352625_en-us_topic_0000001201521669_dli_08_0256_section4299113491:

Parameters
----------

.. table:: **Table 1** Parameter description

   +---------------------------------------+-------------+-------------------------------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                             | Mandatory   | Default Value                             | Type        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   +=======================================+=============+===========================================+=============+=========================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | connector                             | Yes         | None                                      | String      | The value is fixed at **filesystem**.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
   +---------------------------------------+-------------+-------------------------------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | path                                  | Yes         | None                                      | String      | OBS path                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
   +---------------------------------------+-------------+-------------------------------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format                                | Yes         | None                                      | String      | File format                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   |                                       |             |                                           |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                       |             |                                           |             | Available values are: **csv** and **parquet**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
   +---------------------------------------+-------------+-------------------------------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.rolling-policy.file-size         | No          | 128 MB                                    | MemorySize  | Maximum size of a part file. If the size of a part file exceeds this value, a new file will be generated.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   |                                       |             |                                           |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                       |             |                                           |             | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   |                                       |             |                                           |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                       |             |                                           |             |    The Rolling Policy defines when a given in-progress part file will be closed and moved to the pending and later to finished state. Part files in the "finished" state are the ones that are ready for viewing and are guaranteed to contain valid data that will not be reverted in case of failure. In STREAMING mode, the Rolling Policy in combination with the checkpointing interval (pending files become finished on the next checkpoint) control how quickly part files become available for downstream readers and also the size and number of these parts. |
   +---------------------------------------+-------------+-------------------------------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.rolling-policy.rollover-interval | No          | 30 min                                    | Duration    | Maximum duration that a part file can be opened. If a part file is opened longer than the maximum duration, a new file will be generated in rolling mode. The default value is 30 minutes so that there will not be a large number of small files. The check frequency is specified by **sink.rolling-policy.check-interval**.                                                                                                                                                                                                                                          |
   |                                       |             |                                           |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                       |             |                                           |             | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   |                                       |             |                                           |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                       |             |                                           |             |    There must be a space between the number and the unit.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   |                                       |             |                                           |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                       |             |                                           |             |    The supported time units include **d**, **h**, **min**, **s**, and **ms**.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                       |             |                                           |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                       |             |                                           |             |    For bulk files (parquet, orc, and avro), the checkpoint interval also controls the maximum open duration of a part file.                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   +---------------------------------------+-------------+-------------------------------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.rolling-policy.check-interval    | No          | 1 min                                     | Duration    | Check interval of the time-based rolling policy                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                       |             |                                           |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                       |             |                                           |             | This parameter controls the frequency of checking whether a file should be rolled based on **sink.rolling-policy.rollover-interval**.                                                                                                                                                                                                                                                                                                                                                                                                                                   |
   +---------------------------------------+-------------+-------------------------------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | auto-compaction                       | No          | false                                     | Boolean     | Whether automatic compaction is enabled for the streaming sink. Data is first written to temporary files. After the checkpoint is complete, the temporary files generated by the checkpoint are compacted.                                                                                                                                                                                                                                                                                                                                                              |
   +---------------------------------------+-------------+-------------------------------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | compaction.file-size                  | No          | Size of **sink.rolling-policy.file-size** | MemorySize  | Size of the files that will be compacted. The default value is the size of the files that will be rolled.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   |                                       |             |                                           |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                       |             |                                           |             | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   |                                       |             |                                           |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                       |             |                                           |             |    -  Only files in the same checkpoint are compacted. The final files must be more than or equal to the number of checkpoints.                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                       |             |                                           |             |    -  If the compaction takes a long time, back pressure may occur and the checkpointing may be prolonged.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
   |                                       |             |                                           |             |    -  After this function is enabled, final files are generated during checkpoint and a new file is opened to receive the data generated at the next checkpoint.                                                                                                                                                                                                                                                                                                                                                                                                        |
   +---------------------------------------+-------------+-------------------------------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example 1
---------

Use datagen to randomly generate data and write the data into the **fileName** directory in the OBS bucket **bucketName**. The file generation time is irrelevant to the checkpoint. When the file is opened more than 30 minutes or is bigger than 128 MB, a new file is generated.

.. code-block::

   create table orders(
     name string,
     num INT
   ) with (
     'connector' = 'datagen',
     'rows-per-second' = '100',
     'fields.name.kind' = 'random',
     'fields.name.length' = '5'
   );

   CREATE TABLE sink_table (
      name string,
      num INT
   ) WITH (
      'connector' = 'filesystem',
      'path' = 'obs://bucketName/fileName',
      'format' = 'csv',
      'sink.rolling-policy.file-size'='128m',
      'sink.rolling-policy.rollover-interval'='30 min'
   );
   INSERT into sink_table SELECT * from orders;

Example 2
---------

Use datagen to randomly generate data and write the data into the **fileName** directory in the OBS bucket **bucketName**. The file generation time is relevant to the checkpoint. When the checkpoint interval is reached or the file size reaches 100 MB, a new file is generated.

.. code-block::

   create table orders(
     name string,
     num INT
   ) with (
     'connector' = 'datagen',
     'rows-per-second' = '100',
     'fields.name.kind' = 'random',
     'fields.name.length' = '5'
   );

   CREATE TABLE sink_table (
      name string,
      num INT
   ) WITH (
      'connector' = 'filesystem',
      'path' = 'obs://bucketName/fileName',
      'format' = 'csv',
      'sink.rolling-policy.file-size'='128m',
      'sink.rolling-policy.rollover-interval'='30 min',
      'auto-compaction'='true',
      'compaction.file-size'='100m'

   );
   INSERT into sink_table SELECT * from orders;
