:original_name: dli_03_0099.html

.. _dli_03_0099:

How Do I Save Flink Job Logs?
=============================

When you create a Flink SQL job or Flink Jar job, you can select **Save Job Log** on the job editing page to save job running logs to OBS.

To set the OBS bucket for storing the job logs, specify a bucket for **OBS Bucket**. If the selected OBS bucket is not authorized, click **Authorize**.

The logs are saved in the following path: *Bucket name*\ **/jobs/logs/**\ *Directory starting with the job ID*. You can customize the bucket name in the path. **/jobs/logs/**\ *Directory starting with the job ID* is a fixed format.

In the job list, click the job name. In the **Run Log** tab, click the provided OBS link to go to the path.
