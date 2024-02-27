:original_name: dli_08_0343.html

.. _dli_08_0343:

Deleting Partitions by Specifying Filter Criteria (Only Supported on OBS Tables)
================================================================================

Function
--------

This statement is used to delete one or more partitions based on specified conditions.

Precautions
-----------

-  This statement is only used for OBS tables.
-  The table in which partitions are to be deleted must exist. Otherwise, an error is reported.
-  The partition to be deleted must exist. Otherwise, an error is reported. To avoid this error, add **IF EXISTS** to this statement.

Syntax
------

::

   ALTER TABLE [db_name.]table_name
     DROP [IF EXISTS]
     PARTITIONS partition_filtercondition;

Keywords
--------

-  DROP: deletes specified partitions.
-  IF EXISTS: Partitions to be deleted must exist. Otherwise, an error is reported.
-  PARTITIONS: specifies partitions meeting the conditions

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                                                                 |
   +===================================+=============================================================================================================================================================================================================================================================================================+
   | db_name                           | Database name that contains letters, digits, and underscores (_). It cannot contain only digits or start with an underscore (_).                                                                                                                                                            |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name                        | Table name of a database that contains letters, digits, and underscores (_). It cannot contain only digits or start with an underscore (_). The matching rule is **^(?!_)(?![0-9]+$)[A-Za-z0-9_$]*$**. If special characters are required, use single quotation marks ('') to enclose them. |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   | **This statement is used for OBS table operations.**                                                                                                                                                                                                                                        |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | partition_filtercondition         | Condition used to search partitions to be deleted. The format is as follows:                                                                                                                                                                                                                |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   | *Partition column name* :ref:`Operator <dli_08_0061__en-us_topic_0093946932_t34b3b699258a401085f3c3b3ad1a3717>` *Value to compare*                                                                                                                                                          |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   | Example: start_date < '201911'                                                                                                                                                                                                                                                              |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   | -  Example 1: <partition_filtercondition1> AND|OR <partition_filtercondition2>                                                                                                                                                                                                              |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   |    Example: start_date < '201911' OR start_date >= '202006'                                                                                                                                                                                                                                 |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   | -  Example 2: (<partition_filtercondition1>)[,partitions (<partition_filtercondition2>), ...]                                                                                                                                                                                               |
   |                                   |                                                                                                                                                                                                                                                                                             |
   |                                   |    Example: (start_date <> '202007'), partitions(start_date < '201912')                                                                                                                                                                                                                     |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

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
      options (path 'path 'obs://bucketName/filePath'')
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

   .. code-block::

      SHOW partitions student;

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

   .. note::

      This step describes how to delete a partition by specifying filter criteria. If you want to delete a partition without specifying filter criteria, see :ref:`Deleting a Partition <dli_08_0083>`.

      This example cannot be used together with that in :ref:`Deleting a Partition <dli_08_0083>`. Distinguish the keyword **partitions** in this example from the keyword **partition** in the example in :ref:`Deleting a Partition <dli_08_0083>`.

   -  **Example 1: deleting partitions by specifying filter criteria (only supported on OBS tables), and using the AND statement to delete partitions**

      .. table:: **Table 3** Data before execution

         ============ ===========
         facultyNo    classNo
         ============ ===========
         facultyNo=10 classNo=101
         facultyNo=10 classNo=102
         facultyNo=20 classNo=101
         facultyNo=20 classNo=102
         ============ ===========

      Run the following statements to delete the partitions whose **facultyNo** is **20** and **classNo** is **102**:

      .. code-block::

         ALTER TABLE student
         DROP IF EXISTS
         PARTITIONS (facultyNo = 20 AND classNo = 102);

      You can see that the statement deletes the partitions that meet both the criteria.

      .. table:: **Table 4** Data after execution

         ============ ===========
         facultyNo    classNo
         ============ ===========
         facultyNo=10 classNo=101
         facultyNo=10 classNo=102
         facultyNo=20 classNo=101
         ============ ===========

   -  **Example 2: deleting partitions by specifying filter criteria (only supported on OBS tables), and using the OR statement to delete partitions**

      .. table:: **Table 5** Data before execution

         ============ ===========
         facultyNo    classNo
         ============ ===========
         facultyNo=10 classNo=101
         facultyNo=10 classNo=102
         facultyNo=20 classNo=101
         facultyNo=20 classNo=102
         ============ ===========

      Run the following statements to delete the partitions whose **facultyNo** is **10** or **classNo** is **101**:

      .. code-block::

         ALTER TABLE student
         DROP IF EXISTS
         PARTITIONS (facultyNo = 10),
         PARTITIONS (classNo = 101);

      Execution result:

      .. table:: **Table 6** Data after execution

         ============ ===========
         facultyNo    classNo
         ============ ===========
         facultyNo=20 classNo=102
         ============ ===========

      Under the selected deletion criteria, the first record in the partition meets both **facultyNo** and **classNo**, the second record meets **facultyNo**, and the third record meets **classNo**.

      As a result, only one partition row remains after executing the partition deletion statement.

      According to method 1, the foregoing execution statement may also be written as:

      .. code-block::

         ALTER TABLE student
         DROP IF EXISTS
         PARTITIONS (facultyNo = 10 OR classNo = 101);

   -  **Example 3: deleting partitions by specifying filter criteria (only supported on OBS tables), and using relational operator statements to delete specified partitions**

      .. table:: **Table 7** Data before execution

         ============ ===========
         facultyNo    classNo
         ============ ===========
         facultyNo=10 classNo=101
         facultyNo=10 classNo=102
         facultyNo=20 classNo=101
         facultyNo=20 classNo=102
         facultyNo=20 classNo=103
         ============ ===========

      Run the following statements to delete partitions whose **classNo** is greater than 100 and less than 102:

      .. code-block::

         ALTER TABLE student
         DROP IF EXISTS
         PARTITIONS (classNo BETWEEN 100 AND 102);

      Execution result:

      .. table:: **Table 8** Data before execution

         ============ ===========
         facultyNo    classNo
         ============ ===========
         facultyNo=20 classNo=103
         ============ ===========
