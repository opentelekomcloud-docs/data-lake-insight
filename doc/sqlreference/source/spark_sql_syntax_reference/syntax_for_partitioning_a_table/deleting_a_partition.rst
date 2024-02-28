:original_name: dli_08_0083.html

.. _dli_08_0083:

Deleting a Partition
====================

Function
--------

This statement is used to delete one or more partitions from a partitioned table.

Partitioned tables are classified into OBS tables and DLI tables. You can delete one or more partitions from a DLI or OBS partitioned table based on specified conditions. OBS tables also support deleting partitions by specifying filter criteria. For details, see :ref:`Deleting Partitions by Specifying Filter Criteria (Only Supported on OBS Tables) <dli_08_0343>`.

Precautions
-----------

-  The table in which partitions are to be deleted must exist. Otherwise, an error is reported.
-  The partition to be deleted must exist. Otherwise, an error is reported. To avoid this error, add **IF EXISTS** to this statement.

Syntax
------

::

   ALTER TABLE [db_name.]table_name
     DROP [IF EXISTS]
     PARTITION partition_spec1[,PARTITION partition_spec2,...];

Keywords
--------

-  DROP: deletes a partition.
-  IF EXISTS: The partition to be deleted must exist. Otherwise, an error is reported.
-  PARTITION: specifies the partition to be deleted

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
   +=================+============================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | db_name         | Database name that contains letters, digits, and underscores (_). It cannot contain only digits and cannot start with an underscore (_).                                                                                                                                                                                                                                                                                                                                                                   |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name      | Table name of a database that contains letters, digits, and underscores (_). It cannot contain only digits and cannot start with an underscore (_). The matching rule is **^(?!_)(?![0-9]+$)[A-Za-z0-9_$]*$**. If special characters are required, use single quotation marks ('') to enclose them.                                                                                                                                                                                                        |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | partition_specs | Partition information, in the format of "key=value", where **key** indicates the partition field and **value** indicates the partition value. In a table partitioned using multiple fields, if you specify all the fields of a partition name, only the partition is deleted; if you specify only some fields of a partition name, all matching partitions will be deleted. By default, parameters in **partition_specs** contain parentheses (), for example, **PARTITION (facultyNo=20, classNo=103);**. |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

To help you understand how to use this statement, this section provides an example of deleting a partition from the source data.

#. Use the DataSource syntax to create an OBS partitioned table.

   An OBS partitioned table named **student** is created, which contains the student ID (**id**), student name (**name**), student faculty number (**facultyNo**), and student class number (**classNo**) and uses **facultyNo** and **classNo** for partitioning.

   ::

      create table if not exists student (
      id int,
      name STRING,
      facultyNo int,
      classNo INT)
      using csv
      options (path 'obs://bucketName/filePath')
      partitioned by (faculytNo, classNo);

#. Insert partition data into the table.

   You can insert the following data:

   ::

      INSERT into student
      partition (facultyNo = 10, classNo = 101)
      values (1010101, "student01"), (1010102, "student02");

      INSERT into student
      partition (facultyNo = 10, classNo = 102)
      values (1010203, "student03"), (1010204, "student04");

      INSERT into student
      partition (facultyNo = 20, classNo = 101)
      values (2010105, "student05"), (2010106, "student06");

      INSERT into student
      partition (facultyNo = 20, classNo = 102)
      values (2010207, "student07"), (2010208, "student08");

      INSERT into student
      partition (facultyNo = 20, classNo = 103)
      values (2010309, "student09"), (2010310, "student10");

      INSERT into student
      partition (facultyNo = 30, classNo = 101)
      values (3010111, "student11"), (3010112, "student12");

      INSERT into student
      partition (facultyNo = 30, classNo = 102)
      values (3010213, "student13"), (3010214, "student14");

#. View the partitions.

   You can view all partitions in the table.

   The example code is as follows:

   **SHOW partitions student;**

   .. table:: **Table 2** Example table data

      ============ ===========
      facultyNo    classNo
      ============ ===========
      facultyNo=10 classNo=101
      facultyNo=10 classNo=102
      facultyNo=20 classNo=101
      facultyNo=20 classNo=102
      facultyNo=20 classNo=103
      facultyNo=30 classNo=101
      facultyNo=30 classNo=102
      ============ ===========

#. Delete a partition.

   -  **Example 1: deleting a partition by specifying multiple filter criteria**

      In this example, the partition whose **facultyNo** is **20** and **classNo** is **103** is deleted.

      .. note::

         For details about how to delete a partition by specifying filter criteria, see :ref:`Deleting Partitions by Specifying Filter Criteria (Only Supported on OBS Tables) <dli_08_0343>`.

      The example code is as follows:

      .. code-block::

         ALTER TABLE student
         DROP IF EXISTS
         PARTITION (facultyNo=20, classNo=103);

      Use the method described in step 3 to check the partitions in the table. You can see that the partition has been deleted.

      .. code-block::

         SHOW partitions student;

   -  **Example 2: deleting a partition by specifying a single filter criterion**

      In this example, the partitions whose **facultyNo** is **30** is deleted. During data insertion, there are two partitions whose **facultyNo** is **30**.

      .. note::

         For details about how to delete a partition by specifying filter criteria, see :ref:`Deleting Partitions by Specifying Filter Criteria (Only Supported on OBS Tables) <dli_08_0343>`.

      The example code is as follows:

      .. code-block::

         ALTER TABLE student
         DROP IF EXISTS
         PARTITION (facultyNo = 30);

      Execution result:

      .. table:: **Table 3** Example table data

         ============ ===========
         facultyNo    classNo
         ============ ===========
         facultyNo=10 classNo=101
         facultyNo=10 classNo=102
         facultyNo=20 classNo=101
         facultyNo=20 classNo=102
         facultyNo=20 classNo=103
         ============ ===========
