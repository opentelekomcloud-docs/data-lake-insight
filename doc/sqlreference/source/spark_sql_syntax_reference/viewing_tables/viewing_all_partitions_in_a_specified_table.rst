:original_name: dli_08_0094.html

.. _dli_08_0094:

Viewing All Partitions in a Specified Table
===========================================

Function
--------

This statement is used to view all partitions in a specified table.

Syntax
------

::

   SHOW PARTITIONS [db_name.]table_name
     [PARTITION partition_specs];

Keyword
-------

-  PARTITIONS: partitions in a specified table
-  PARTITION: a specified partition

Parameters
----------

.. table:: **Table 1** Parameter description

   +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Description                                                                                                                                                                                                                                                                                         |
   +=================+=====================================================================================================================================================================================================================================================================================================+
   | db_name         | Database name that contains letters, digits, and underscores (_). It cannot contain only digits and cannot start with an underscore (_).                                                                                                                                                            |
   +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name      | Table name of a database that contains letters, digits, and underscores (_). It cannot contain only digits and cannot start with an underscore (_). The matching rule is **^(?!_)(?![0-9]+$)[A-Za-z0-9_$]*$**. If special characters are required, use single quotation marks ('') to enclose them. |
   +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | partition_specs | Partition information, in the format of "key=value", where **key** indicates the partition field and **value** indicates the partition value. If a partition field contains multiple fields, the system displays all partition information that matches the partition field.                        |
   +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

The table specified in this statement must exist and must be a partitioned table. Otherwise, an error is reported.

Example
-------

-  To show all partitions in the **student** table, run the following statement:

   ::

      SHOW PARTITIONS student;

-  Check the **dt='2010-10-10'** partition in the **student** table, run the following statement:

   ::

      SHOW PARTITIONS student PARTITION(dt='2010-10-10')
