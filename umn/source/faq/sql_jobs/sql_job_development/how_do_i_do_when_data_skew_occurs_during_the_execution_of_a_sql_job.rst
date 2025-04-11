:original_name: dli_03_0093.html

.. _dli_03_0093:

How Do I Do When Data Skew Occurs During the Execution of a SQL Job?
====================================================================

What Is Data Skew?
------------------

Data skew is a common issue during the execution of SQL jobs. When data is unevenly distributed, some compute nodes process significantly more data than others, which can impact the efficiency of the entire computation process.

For example, if you notice that a SQL query is taking a long time to execute, you can check its status in SparkUI. See :ref:`Figure 1 <dli_03_0093__fig1563419919123>`. If you see a stage that has been running for over 20 minutes with only one task remaining, it is likely due to data skew.

.. _dli_03_0093__fig1563419919123:

.. figure:: /_static/images/en-us_image_0000001200929158.png
   :alt: **Figure 1** Data skew example

   **Figure 1** Data skew example

Common Data Skew Scenarios
--------------------------

-  Group By aggregation skew

   During the execution of Group By aggregation, if some grouping keys have significantly more data than others, the larger groups will consume more compute resources and time during the aggregation process, resulting in slower processing speeds and data skew.

-  JOIN operation skew

   During table JOIN operations, if the keys involved in the JOIN are unevenly distributed in one of the tables, a large amount of data will be concentrated in a few tasks while others have already completed, causing data skew.

Solution for Group By Data Skew
-------------------------------

Select a subset of data and run **select count(*) as sum,Key from tbl group by Key order by sum desc** to identify which keys are causing data skew.

Then, for the skewed keys, you can handle them separately by adding a salt to split them into multiple tasks for individual statistics, and finally combine the results of the separate statistics.

For example, consider the following SQL query where **Key01** is identified as the skewed key causing a single task to process a large amount of data. The following steps can be taken to handle it:

.. code-block::

   SELECT
     a.Key,
     SUM(a.sum) AS Cnt
   FROM
     (
       SELECT
         Key,
         count(*) AS sum
       FROM
         tbl
       GROUP BY
         Key,
         CASE
           WHEN KEY = 'Key01' THEN floor(random () * 200)
           ELSE 0
         END
     ) a
   GROUP BY
     a.Key;

Solution for JOIN Data Skew
---------------------------

#. Log in to the DLI management console. Choose **Job Management** > **SQL Jobs** in the navigation pane. On the displayed page, locate the job you want to modify and click **Edit** in the **Operation** column to switch to the **SQL Editor** page.

#. On the **SQL editor** page, click **Set Property** and add the following Spark parameters through the **Settings** pane:

   The string followed by the colons (:) are the configuration parameters, and the strings following the colons are the values.

   .. code-block::

      spark.sql.enableToString:false
      spark.sql.adaptive.join.enabled:true
      spark.sql.adaptive.enabled:true
      spark.sql.adaptive.skewedJoin.enabled:true
      spark.sql.adaptive.enableToString:false
      spark.sql.adaptive.skewedPartitionMaxSplits:10

   .. note::

      **spark.sql.adaptive.skewedPartitionMaxSplits** indicates the maximum number of tasks for processing a skewed partition. The default value is **5**, and the maximum value is **10**. This parameter is optional.

#. Click **Execute** to run the job again.
