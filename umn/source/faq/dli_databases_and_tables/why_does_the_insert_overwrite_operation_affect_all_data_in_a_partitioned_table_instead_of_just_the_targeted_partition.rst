:original_name: dli_03_0212.html

.. _dli_03_0212:

Why Does the "insert overwrite" Operation Affect All Data in a Partitioned Table Instead of Just the Targeted Partition?
========================================================================================================================

When using the **INSERT OVERWRITE** statement to overwrite data in a partitioned table, if you find that it overwrites all data instead of the expected partitioned data, it may be because the dynamic partition overwrite feature is not enabled.

To dynamically overwrite partitioned data specified in a datasource table, you need to first set **dli.sql.dynamicPartitionOverwrite.enabled** to **true**, and then run the **INSERT OVERWRITE** statement.

The default value of **dli.sql.dynamicPartitionOverwrite.enabled** is **false**. If unset, data in the entire table is overwritten.
