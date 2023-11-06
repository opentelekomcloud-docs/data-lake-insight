:original_name: dli_03_0220.html

.. _dli_03_0220:

Why Is a Spark Jar Job Stuck in the Submitting State?
=====================================================

The remaining CUs in the queue may be insufficient. As a result, the job cannot be submitted.

To view the remaining CUs of a queue, perform the following steps:

#. Check the CU usage of the queue.

   Log in to the Cloud Eye console. In the navigation pane on the left, choose **Cloud Service Monitoring** > **Data Lake Insight**. On the displayed page, locate the desired queue and click **View Metric** in the **Operation** column, and check **CU Usage (queue)** on the displayed page.

2. Calculate the number of remaining CUs.

   Remaining CUs of a queue = Total CUs of the queue - CU usage.

If the number of remaining CUs is less than the number of CUs required by the job, the job submission fails. The submission can be successful only after resources are available.
