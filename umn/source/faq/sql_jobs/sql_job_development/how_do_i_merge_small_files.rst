:original_name: dli_03_0086.html

.. _dli_03_0086:

How Do I Merge Small Files?
===========================

What Are Small Files?
---------------------

In distributed file systems, data is stored in blocks. A small file refers to a file whose size is significantly smaller than the block size of the storage system. Large numbers of small files can cause significant performance and management issues for big data systems. Merging small files is one of the key strategies to optimize system performance.

This section explains how to use **DISTRIBUTE BY** in DLI to merge small files.

Key Issues Caused by Small Files
--------------------------------

-  **Metadata overhead**: In file systems like HDFS, the master node (NameNode) manages metadata for each file in memory. A massive number of small files drastically increases memory consumption, becoming a bottleneck for cluster scalability.
-  **Poor computational performance**: Compute engines like Spark typically launch a task for each file or file block. Processing numerous small files leads to:

   -  **High task scheduling overhead**: The time spent launching and scheduling thousands of tasks may exceed the actual data processing time.
   -  **Resource wastage**: Significant CPU and memory resources are consumed by task management rather than effective computation.
   -  **Query latency**: Overall job execution time increases, slowing down query responses.

-  **Low storage efficiency**: Small files often result in inefficient storage utilization, failing to leverage the advantages of distributed block sizes.
-  **Risk of limits being reached**: They may trigger limits on the number of files in a directory or the number of tasks per job in compute engines or file systems, causing job failures.

Basic Principles of Small File Merging
--------------------------------------

-  **Basic Principles**

   In MapReduce-based engines like Spark or Hive, the number of output files depends on the number of tasks in the final stage (usually the Reduce phase). Each Reduce task generates one file by default.

   By using **DISTRIBUTE BY**, data is redistributed into a specified number of Reduce tasks, with each task producing one file, thereby merging files.

   -  **Data redistribution**: Data is sent to different Reduce tasks based on the hash value of the expression that follows (e.g., **floor(rand()*N)**).
   -  **Controlling Reduce task count**: By setting the value of **N**, you indirectly and probabilistically control the number of Reduce tasks, which determines the number of output files. Typically, N represents the desired number of merged files per partition.

Non-Partitioned Table Merging Example
-------------------------------------

-  **Using a temporary table for intermediate storage** (recommended)

   ::

      -- 1. Create a temporary table with the same structure as the source table.
      CREATE TABLE temp_tablename LIKE tablename;

      -- 2. Write merged data into the temporary table.
      INSERT OVERWRITE TABLE temp_tablename
      SELECT * FROM tablename
      DISTRIBUTE BY floor(rand()*20); -- Generate approximately 20 files.

      -- 3. Verify the data.
      SELECT count(*) FROM tablename;       -- Source table
      SELECT count(*) FROM temp_tablename;  -- Temporary table

      -- 4. Write the data back to the source table.
      INSERT OVERWRITE TABLE tablename
      SELECT
      *
      FROM temp_tablename;

      -- 5. (Optional) Delete the old table if no longer needed.
      DROP TABLE table_name_old;

-  **Self read-write method**

   .. code-block::

      INSERT OVERWRITE TABLE tablename
      SELECT *
      FROM tablename
      DISTRIBUTE BY floor(rand() * 20)

   -  **INSERT OVERWRITE TABLE tablename**

      Overwrite the target table with query results.

      .. caution::

         This operation deletes the source table data, so consider using a temporary table to avoid data loss.

   -  **SELECT \* FROM tablename**

      Read all data from the source table.

   -  **DISTRIBUTE BY floor(rand() \* 20)**

      -  Random data distribution:

         -  **rand()** generates a random float between [0, 1).
         -  **rand() \* 20** generates a random float between [0, 20).
         -  **floor(...)** rounds down to integers in [0, 19].

      -  Distribution result:

         -  Data is randomly and evenly distributed across up to 20 Reduce tasks.
         -  Up to 20 output files are generated (each Reduce task corresponds to one file).

Partitioned Table Merging
-------------------------

For partitioned tables, we generally prefer to merge files within each partition separately rather than mixing data across partitions.

-  **Parameter description:**

   -  **N**: The desired number of files **per partition** after merging. For example, set it to **5** if you wish for each partition to ultimately contain only five files.
   -  **pt**: The partition field.

-  **Why do I need to add a partition field to DISTRIBUTE BY?**

   -  This ensures **data from the same partition is sent to the same group of Reduce tasks** for processing.

   -  Using **DISTRIBUTE BY pt, floor(rand() \* N)** first distributes data by the partition field (pt) and then further shuffles it within each partition based on randomness.

      Each partition's data is split into *N* files without affecting other partitions.

-  **Using a temporary table** (recommended)

   ::

      -- 1. Create a temporary table with the same structure as the source table.
      CREATE TABLE table_tmp LIKE table_name;

      -- 2. Merge partitions into the temporary table.
      INSERT OVERWRITE TABLE table_tmp PARTITION(pt)
      SELECT col1, col2, ..., pt
      FROM tablename
      WHERE pt = *  -- Partition conditions
      DISTRIBUTE BY pt, floor(rand() * N); -- N indicates the number of target files per partition.

      -- 3. Verify the data.
      SELECT COUNT(*) FROM tablename;   -- Source table
      SELECT COUNT(*) FROM temp_tablename;    -- Temporary table

      -- 4. Write the data back to the source table.
      INSERT OVERWRITE TABLE tablename PARTITION (pt)
      SELECT
      *
      FROM temp_tablename
      WHERE pt = *;

      -- 5. (Optional) Delete the old table if no longer needed.
      DROP TABLE temp_tablename;

-  **Self read-write method**

   ::

      INSERT OVERWRITE TABLE target_table
      PARTITION (pt)
      SELECT
          col1,
          col2,
          ...,
          pt -- The partition field must appear last in the SELECT statement.
      FROM
          target_table
      WHERE pt = *
      DISTRIBUTE BY pt, floor(rand() * N)

Example: Merging Partitions in a Student Table
----------------------------------------------

-  **Scenario**

   Consider a partitioned table named **student**, with partition fields **facultyNo** (college ID) and **classNo** (class ID). The goal is to merge small files within each partition, limiting each partition to a maximum of 2 files.

   - Table name: **student**

   - Partition fields: **facultyNo** (college ID) and **classNo** (class ID)

   - Goal: Merge each partition into 2 files.

-  **Example code**

   .. code-block::

      -- 1. Create a temporary table.
      CREATE TABLE student_tmp LIKE student;

      -- 2. Merge specific partitions.
      INSERT OVERWRITE TABLE student_tmp PARTITION(facultyNo, classNo)
      SELECT
          id,
          name,
          gender,
          age,
          birth_date,
          phone,
          email,
          address,
          enrollment_date,
          major,
          grade,
          status,
          facultyNo,
          classNo
      FROM student
      WHERE facultyNo = 1 AND classNo = 7  -- Partition conditions
      DISTRIBUTE BY facultyNo, classNo, floor(rand() * 2);  -- Controlling the number of files

   After execution, the data under **facultyNo = 1, classNo = 7** will be consolidated into up to 2 files.

   .. code-block::

      -- 3. Verify the partition (e.g., facultyNo = 1, classNo = 7):
      SELECT count(*) FROM student
      WHERE facultyNo=1 AND classNo=7;   --> Original data volume

      SELECT count(*) FROM student_tmp
      WHERE facultyNo=1 AND classNo=7;   --> Merged data volume

      -- 4. Run INSERT OVERWRITE to write the merged data back to the source table.
      INSERT OVERWRITE TABLE student PARTITION(facultyNo, classNo)
      SELECT
      *
      FROM student_temp
      WHERE facultyNo > 0;

More Operation Recommendations
------------------------------

When directly overwriting the source table, data loss may occur if the task fails during execution.

**To mitigate this risk, you are advised to use a temporary table as an intermediary:**

#. **Create a temporary table** with the same structure as the source table.
#. **Insert merged data into the temporary table.**
#. **After verifying the data in the temporary table**, switch it to the production table using **ALTER TABLE ... RENAME TO** or **INSERT OVERWRITE**.

For the **DISTRIBUTE BY** clause: Instead of relying solely on random numbers, you can use a naturally high-cardinality field like **user_id**. This approach not only merges files but also sorts the data based on that field, potentially improving query performance.

.. code-block::

   -- 1. Create a temporary table with the same structure as the source table table_name.
   CREATE TABLE table_name_tmp LIKE table_name;

   -- 2. Merge the data from the source table and insert it into the temporary table.
   INSERT OVERWRITE TABLE table_name_tmp PARTITION (pt) -- For a partitioned table
   SELECT ... -- Selected fields
   FROM table_name
   WHERE pt = *
   DISTRIBUTE BY pt, floor(rand() * N); -- Partitioned table syntax
   -- For a non-partitioned table: DISTRIBUTE BY floor(rand() * N)

   -- 3. Verify data integrity (e.g., row counts, partitions).
   SELECT count(*) FROM table_name;
   SELECT count(*) FROM table_name_tmp;

   -- 4. Swap tables via renaming (a metadata operation completed instantly).
   ALTER TABLE table_name RENAME TO table_name_old;
   ALTER TABLE table_name_tmp RENAME TO table_name;

   -- 5. (Optional) Delete the old table if no longer needed.
   DROP TABLE table_name_old;

Summary of Small File Merging Methods
-------------------------------------

+---------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
| Scenario                  | Recommended SQL Syntax                                                                                                        | Key Point                                                                                    |
+===========================+===============================================================================================================================+==============================================================================================+
| **Non-partitioned table** | .. code-block::                                                                                                               | Use random numbers to control the number of files (N).                                       |
|                           |                                                                                                                               |                                                                                              |
|                           |    INSERT OVERWRITE TABLE table_tmp SELECT * FROM table DISTRIBUTE BY floor(rand()*N);                                        |                                                                                              |
+---------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
| **Partitioned table**     | .. code-block::                                                                                                               | **Include the partition field in DISTRIBUTE BY** to ensure merging occurs within partitions. |
|                           |                                                                                                                               |                                                                                              |
|                           |    INSERT OVERWRITE TABLE table_tmp PARTITION(pt) SELECT ..., pt FROM table WHERE pt = * DISTRIBUTE BY pt, floor(rand()*N);   |                                                                                              |
+---------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
| **General strategy**      | **Use a temporary table for intermediate storage**. Verify data before swapping tables via renaming to avoid data loss risks. | Minimize the risk of data loss during the process.                                           |
+---------------------------+-------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
