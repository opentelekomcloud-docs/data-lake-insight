:original_name: dli_03_0089.html

.. _dli_03_0089:

How Do I Create an OBS Partitioned Table for a Flink SQL Job?
=============================================================

Scenario
--------

When using a Flink SQL job, you need to create an OBS partition table for subsequent batch processing.

Procedure
---------

In the following example, the **day** field is used as the partition field with the parquet encoding format (only the parquet format is supported currently) to dump **car_info** data to OBS.

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

#. Create an OBS partitioned table.

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
