:original_name: dli_09_0207.html

.. _dli_09_0207:

Configuring High-Reliability Flink Jobs (Automatic Restart upon Exceptions)
===========================================================================

Scenario
--------

If you need to configure high reliability for a Flink application, you can set the parameters when creating your Flink jobs.

Procedure
---------

#. .. _dli_09_0207__li28667133227:

   Create an SMN topic and add an email address or mobile number to subscribe to the topic. You will receive a subscription notification by an email or message. Click the confirmation link to complete the subscription.

#. Log in to the DLI console, create a Flink SQL job, write SQL statements for the job, and configure running parameters. In this example, key parameters are described. Set other parameters based on your requirements. For details about how to create a Flink SQL job, see .

   .. note::

      The reliability configuration of a Flink Jar job is the same as that of a SQL job, which will not be described in this section.

   a. Set **CUs**, Job **Manager CUs**, and **Max Concurrent Jobs** based on the following formulas:

      Total number of CUs = Number of manager CUs + (Total number of concurrent operators / Number of slots of a TaskManager) x Number of TaskManager CUs

      For example, with a total of 9 CUs (1 manager CU) and a maximum of 16 concurrent jobs, the number of compute-specific CUs is 8.

      If you do not configure TaskManager specifications, a TaskManager occupies 1 CU by default and has no slot. To ensure a high reliability, set the number of slots of the TaskManager to 2, according to the preceding formula.

      Set the maximum number of concurrent jobs be twice the number of CUs.

   b. Select **Save Job Log** and select an OBS bucket. If you are not authorized to access the bucket, click **Authorize**. This allows job logs be saved to your OBS bucket. If a job fails, the logs can be used for fault locating.

   c. Select **Alarm Generation upon Job Exception** and select the SMN topic created in :ref:`1 <dli_09_0207__li28667133227>`. This allows DLI to send notifications to your email box or phone when a job exception occurs, so you can be notified of any exceptions in time.

   d. Select **Enable Checkpointing** and set the checkpoint interval and mode as needed. This function ensures that a failed Flink task can be restored from the latest checkpoint.

      .. note::

         -  Checkpoint interval indicates the interval between two triggers. Checkpointing hurts real-time computing performance. To minimize the performance loss, you need to allow for the recovery duration when configuring the interval. It is recommended that the checkpoint interval **be greater than the checkpointing duration**. The recommended value is 5 minutes.
         -  The **Exactly once** mode ensures that each piece of data is consumed only once, and the **At least once** mode ensures that each piece of data is consumed at least once. Select a mode as you need.

   e. Select **Auto Restart upon Exception** and **Restore Job from Checkpoint**, and set the number of retry attempts as needed.

   f. Configure **Dirty Data Policy**. You can select **Ignore**, **Trigger a job exception**, or **Save** based on your service requirements.

   g. Select a queue, and then submit and run the job.

#. Log in to the Cloud Eye console. In the navigation pane on the left, choose **Cloud Service Monitoring** > **Data Lake Insight**. Locate the target Flink job and click **Create Alarm Rule**.

   DLI provides various monitoring metrics for Flink jobs. You can define alarm rules as required using different monitoring metrics for fine-grained job monitoring.
