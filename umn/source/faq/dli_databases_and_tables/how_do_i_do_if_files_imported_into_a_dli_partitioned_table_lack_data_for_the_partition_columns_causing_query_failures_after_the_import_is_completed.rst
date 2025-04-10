:original_name: dli_03_0177.html

.. _dli_03_0177:

How Do I Do If Files Imported Into a DLI Partitioned Table Lack Data for the Partition Columns, Causing Query Failures After the Import Is Completed?
=====================================================================================================================================================

Symptom
-------

A CSV file is imported to a DLI partitioned table, but the imported file data does not contain the data in the partitioning column. The partitioning column needs to be specified for a partitioned table query. As a result, table data cannot be queried.

Possible Causes
---------------

When data is imported to a DLI partitionedtable, if the file data does not contain the partitioning column, the system specifies **\__HIVE_DEFAULT_PARTITION_\_** as the column by default. If a Spark job finds that the partition is empty, **null** is returned.

Solution
--------

#. Log in to the DLI management console. In the SQL editor, click **Settings**.
#. Add **spark.sql.forcePartitionPredicatesOnPartitionedTable.enabled** and set it to **false**.
#. Query the entire table or the partitioned table.
