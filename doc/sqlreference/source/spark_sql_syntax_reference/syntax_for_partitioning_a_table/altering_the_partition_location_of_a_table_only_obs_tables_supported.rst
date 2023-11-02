:original_name: dli_08_0084.html

.. _dli_08_0084:

Altering the Partition Location of a Table (Only OBS Tables Supported)
======================================================================

Function
--------

This statement is used to modify the positions of table partitions.

Syntax
------

::

   ALTER TABLE table_name
     PARTITION partition_specs
     SET LOCATION obs_path;

Keyword
-------

-  PARTITION: a specified partition
-  LOCATION: path of the partition

Parameters
----------

.. table:: **Table 1** Parameter description

   =============== ================
   Parameter       Description
   =============== ================
   table_name      Table name
   partition_specs Partition fields
   obs_path        OBS path
   =============== ================

Precautions
-----------

-  For a table partition whose position is to be modified, the table and partition must exist. Otherwise, an error is reported.
-  By default, the **partition_specs** parameter contains **()**. For example: **PARTITION (dt='2009-09-09',city='xxx')**
-  The specified OBS path must be an absolute path. Otherwise, an error is reported.
-  If the path specified in the new partition contains subdirectories (or nested subdirectories), all file types and content in the subdirectories are considered partition records. Ensure that all file types and file content in the partition directory are the same as those in the table. Otherwise, an error is reported.

Example
-------

To set the OBS path of partition **dt='2008-08-08',city='xxx'** in table **student** to **obs://bucketName/fileName/student/dt=2008-08-08/city=xxx**, run the following statement:

::

   ALTER TABLE student
     PARTITION(dt='2008-08-08',city='xxx')
     SET LOCATION 'obs://bucketName/fileName/student/dt=2008-08-08/city=xxx';
