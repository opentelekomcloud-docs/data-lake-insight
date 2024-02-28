:original_name: dli_08_0082.html

.. _dli_08_0082:

Renaming a Partition (Only OBS Tables Supported)
================================================

Function
--------

This statement is used to rename partitions.

Syntax
------

::

   ALTER TABLE table_name
     PARTITION partition_specs
     RENAME TO PARTITION partition_specs;

Keywords
--------

-  PARTITION: a specified partition
-  RENAME: new name of the partition

Parameters
----------

.. table:: **Table 1** Parameters

   =============== ================
   Parameter       Description
   =============== ================
   table_name      Table name
   partition_specs Partition fields
   =============== ================

Precautions
-----------

-  **This statement is used for OBS table operations.**
-  The table and partition to be renamed must exist. Otherwise, an error occurs. The name of the new partition must be unique. Otherwise, an error occurs.
-  If a table is partitioned using multiple fields, you are required to specify all the fields of a partition (at random order) when renaming the partition.
-  By default, the **partition_specs** parameter contains **()**. For example: **PARTITION (dt='2009-09-09',city='xxx')**

Example
-------

To modify the name of the **city='xxx',dt='2008-08-08'** partition in the **student** table to **city='xxx',dt='2009-09-09'**, run the following statement:

::

   ALTER TABLE student
     PARTITION (city='xxx',dt='2008-08-08')
     RENAME TO PARTITION (city='xxx',dt='2009-09-09');
