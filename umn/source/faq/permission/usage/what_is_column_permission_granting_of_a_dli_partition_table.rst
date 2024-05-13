:original_name: dli_03_0008.html

.. _dli_03_0008:

What Is Column Permission Granting of a DLI Partition Table?
============================================================

You cannot perform permission-related operations on the partition column of a partitioned table.

However, when you grant the permission of any non-partition column in a partitioned table to another user, the user gets the permission of the partition column by default.

When the user views the permission of the partition table, the permission of the partition column will not be displayed.
