:original_name: dli_03_0168.html

.. _dli_03_0168:

Why Is No Data Queried in the DLI Table Created Using the OBS File Path When Data Is Written to OBS by a Flink Job Output Stream?
=================================================================================================================================

Symptom
-------

After data is written to OBS through the Flink job output stream, data cannot be queried from the DLI table created in the OBS file path.

For example, use the following Flink result table to write data to the **obs://obs-sink/car_infos** path in OBS.

.. code-block::

   create sink stream car_infos_sink (
     carId string,
     carOwner string,
     average_speed double,
     buyday string
     ) partitioned by (buyday)
     with (
       type = "filesystem",
       file.path = "obs://obs-sink/car_infos",
       encode = "parquet",
       ak = "{{myAk}}",
       sk = "{{mySk}}"
   );

Use the following statements to create a DLI partition table with data retrieved from the OBS file path. No data is found when you query the **car_infos** table on DLI.

.. code-block::

   create table car_infos (
     carId string,
     carOwner string,
     average_speed double
   )
     partitioned by (buyday string)
     stored as parquet
     location 'obs://obs-sink/car_infos';

Solution
--------

#. Check whether checkpointing is enabled for the Flink result table (**car_infos_sink** in the preceding example) when you create the job on DLI. If checkpointing is disabled, enable it and run the job again to generate OBS data files.

   To enable checkpointing, perform the following steps:

   a. Log in to the DLI management console. Choose **Job Management** > **Flink Jobs** in the left navigation pane. Locate the row that contains the target Flink job and click **Edit** in the **Operation** column.
   b. In the **Running Parameters** area, check whether **Enable Checkpointing** is enabled.

#. Check whether the structure of the Flink result table is the same as that of the DLI partitioned table. For the preceding example, check whether the fields of the **car_infos_sink** table are consistent with those of the **car_infos** table.

#. Check whether the partitioning information of the the partitioned table is restored after it is created using the OBS file. The following statement restore partitions of the **car_infos** table:

   .. code-block::

      alter table car_infos recover partitions;
