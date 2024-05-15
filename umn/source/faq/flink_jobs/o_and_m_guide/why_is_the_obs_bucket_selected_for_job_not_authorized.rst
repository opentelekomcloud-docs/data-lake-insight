:original_name: dli_03_0045.html

.. _dli_03_0045:

Why Is the OBS Bucket Selected for Job Not Authorized?
======================================================

If the OBS bucket selected for a job is not authorized, perform the following steps:

#. On the DLI management console, choose **Global Configuration** > **Service Authorization** and select the **Tenant Administrator (Global service)** permission.
#. In the navigation pane, choose **Job Management** > **Flink Jobs**.
#. In the row where the target job resides, click **Edit** under **Operation** to switch to the **Edit** page.
#. Configure parameters under **Running Parameters** on the **Edit** page.

   a. Select **Enable Checkpointing** or **Save Job Log**.
   b. Specify **OBS Bucket**.
   c. Select **Authorize OBS**.
