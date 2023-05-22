:original_name: dli_03_0145.html

.. _dli_03_0145:

Why Is Error "There should be at least one partition pruning predicate on partitioned table XX.YYY" Reported When a Query Statement Is Executed?
================================================================================================================================================

-  Cause Analysis

   When you query the partitioned table **XX.YYY**, the partition column is not specified in the search criteria.

   A partitioned table can be queried only when the query condition contains at least one partition column.

-  Solution

   Query a partitioned table by referring to the following example:

   Assume that **partitionedTable** is a partitioned table and **partitionedColumn** is a partition column. The query statement is as follows:

   .. code-block::

      SELECT * FROM partitionedTable WHERE partitionedColumn  = XXX
