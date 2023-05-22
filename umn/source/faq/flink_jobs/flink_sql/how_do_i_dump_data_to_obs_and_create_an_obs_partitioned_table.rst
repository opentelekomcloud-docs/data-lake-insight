:original_name: dli_03_0075.html

.. _dli_03_0075:

How Do I Dump Data to OBS and Create an OBS Partitioned Table?
==============================================================

In this example, the **day** field is used as the partition field with the parquet encoding format (only the parquet format is supported currently) to dump **car_info** data to OBS. For more information, see "File System Sink Stream (Recommended)" in *Data Lake Insight SQL Syntax Reference*.

::

   create sink stream car_infos (
     carId string,
     carOwner string,
     average_speed double,
     day string
     ) partitioned by (day)
     with (
       type = "filesystem",
       file.path = "obs://obs-sink/car_infos",
       encode = "parquet",
       ak = "{{myAk}}",
       sk = "{{mySk}}"
   );

Structure of the data storage directory in OBS: **obs://obs-sink/car_infos/day=xx/part-x-x**.

After the data is generated, the OBS partition table can be established for subsequent batch processing through the following SQL statements:

#. Create an OBS partition table.

   ::

      create table car_infos (
        carId string,
        carOwner string,
        average_speed double
      )
        partitioned by (day string)
        stored as parquet
        location 'obs://obs-sink/car-infos';

#. Restore partition information from the associated OBS path.

   ::

      alter table car_infos recover partitions;
