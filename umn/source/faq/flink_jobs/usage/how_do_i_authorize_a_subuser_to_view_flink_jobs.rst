:original_name: dli_03_0139.html

.. _dli_03_0139:

How Do I Authorize a Subuser to View Flink Jobs?
================================================

A sub-user can view queues but cannot view Flink jobs. You can authorize the sub-user using DLI or IAM.

-  Authorization on DLI

   #. Log in to the DLI console using a tenant account, a job owner account, or an account with the **DLI Service Administrator** permission.
   #. Choose **Job Management** > **Flink Jobs**. On the displayed page, locate the target job.
   #. In the **Operation** column of the target job, choose **More** > **Permissions**.
   #. On the displayed page, click **Grant Permission**. Enter the name of the user to be authorized and select the required permissions. Click **OK**. The authorized user can view the job and perform related operations.

-  Authorization on IAM

   #. Log in to the IAM console. In the navigation pane, choose **Permissions** > **Policies/Roles**. On the displayed page, click **Create Custom Policy**.

   #. .. _dli_03_0139__en-us_topic_0000001117353058_li598911295292:

      Create a permission policy for the subuser to view DLI Flink jobs.

      -  **Policy Name**: Use the default name or customize a name.
      -  **Scope**: Select **Project-level services**.
      -  **Policy View**: Select **Visual editor**.
      -  **Policy Content**: Select **Allow**, **Data Lake Insight**, and **dli:jobs:list_all** in sequence.

      Click **OK** to create the policy.

   #. Go to the **User Group** page, locate the user group to which the subuser to be authorized belongs and click the user group name. On the displayed page, click **Assign**.

   #. Grant permissions to the user group.

      -  Select **Region-specific projects** for **Scope**.

      -  Select the permission policy created in :ref:`2 <dli_03_0139__en-us_topic_0000001117353058_li598911295292>` for **Permissions**.

         You can also select **DLI Service Admin** (with all DLI permissions) for the subuser to view Flink jobs.
