:original_name: dli_08_0083.html

.. _dli_08_0083:

Deleting a Partition
====================

Function
--------

Deletes one or more partitions from a partitioned table.

Precautions
-----------

-  The table in which partitions are to be deleted must exist. Otherwise, an error is reported.
-  The to-be-deleted partition must exist. Otherwise, an error is reported. To avoid this error, add **IF EXISTS** in this statement.

Syntax
------

::

   ALTER TABLE [db_name.]table_name
     DROP [IF EXISTS]
     PARTITION partition_spec1[,PARTITION partition_spec2,...];

Keyword
-------

-  DROP: deletes a partition.
-  IF EXISTS: The partition to be deleted must exist. Otherwise, an error is reported.
-  PARTITION: specifies the partition to be deleted

Parameters
----------

.. table:: **Table 1** Parameter description

   +-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   +=================+====================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | db_name         | Database name that contains letters, digits, and underscores (_). It cannot contain only digits and cannot start with an underscore (_).                                                                                                                                                                                                                                                                                                                                                           |
   +-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name      | Table name of a database that contains letters, digits, and underscores (_). It cannot contain only digits and cannot start with an underscore (_). The matching rule is **^(?!_)(?![0-9]+$)[A-Za-z0-9_$]*$**. If special characters are required, use single quotation marks ('') to enclose them.                                                                                                                                                                                                |
   +-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | partition_specs | Partition information, in the format of "key=value", where **key** indicates the partition field and **value** indicates the partition value. In a table partitioned using multiple fields, if you specify all the fields of a partition name, only the partition is deleted; if you specify only some fields of a partition name, all matching partitions will be deleted. By default, the **partition_specs** parameter contains **()**. For example: **PARTITION (dt='2009-09-09',city='xxx')** |
   +-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

To delete the **dt = '2008-08-08', city = 'xxx'** partition in the **student** table, run the following statement:

::

   ALTER TABLE student
     DROP
     PARTITION (dt = '2008-08-08', city = 'xxx');
