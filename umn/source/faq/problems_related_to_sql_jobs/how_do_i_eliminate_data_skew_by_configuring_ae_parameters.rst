:original_name: dli_03_0093.html

.. _dli_03_0093:

How Do I Eliminate Data Skew by Configuring AE Parameters?
==========================================================

Scenario
--------

If the execution of an SQL statement takes a long time, you need to access the Spark UI to check the execution status.

If data skew occurs, the running time of a stage exceeds 20 minutes and only one task is running.


.. figure:: /_static/images/en-us_image_0000001200929158.png
   :alt: **Figure 1** Data skew example

   **Figure 1** Data skew example

Procedure
---------

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
