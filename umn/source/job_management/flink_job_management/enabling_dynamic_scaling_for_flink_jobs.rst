:original_name: dli_01_0534.html

.. _dli_01_0534:

Enabling Dynamic Scaling for Flink Jobs
=======================================

Scenario
--------

In actual job operations, the compute resources required by a job vary depending on the data volume. As a result, compute resources are wasted when the volume is small and are insufficient when the volume is large.

DLI provides dynamic scaling to dynamically adjust the compute resources used by a job based on the job load, such as the data input and output volume, data input and output rate, and backpressure, to improve resource utilization.

.. note::

   Currently, dynamic scaling can be enabled only for jobs of Flink 1.12 or later.

Notes
-----

-  During dynamic scaling of a Flink job, if queue resources are occupied and the remaining resources are insufficient for starting the job, the job may fail to be restored.
-  When the resources that can be used by a Flink job are dynamically scaled in or out, the background job needs to be stopped and then restored from the savepoint. So, the job cannot process data before the restoration is successful.
-  Savepoints need to be triggered during scaling. So, you must configure an OBS bucket, save logs, and enable checkpointing.
-  Do not set the scaling detection period to a small value to avoid frequent job start and stop.
-  The restoration duration of a scaling job is affected by the savepoint size. If the savepoint size is large, the restoration may take a long time.
-  To adjust the configuration items of dynamic scaling, you need to stop the job, edit the job, and submit the job for the modification to take effect.

Procedure
---------

Dynamic scaling applies to Flink OpenSource SQL and Flink Jar jobs.

#. Log in to the DLI management console.
#. In the navigation pane on the left, choose **Job Management** > **Flink Jobs**.
#. Select the job for which you want to enable dynamic scaling and click **Edit** in the **Operation** column.

   -  For a Flink OpenSource SQL job, click **Runtime Configuration** on the right to configure dynamic scaling parameters.
   -  For a Flink Jar job, click the **Runtime Configuration** box to configure dynamic scaling parameters.

   .. table:: **Table 1** Dynamic scaling parameters

      +------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                    | Default Value         | Description                                                                                                                                                                                                                                                      |
      +==============================+=======================+==================================================================================================================================================================================================================================================================+
      | flink.dli.job.scale.enable   | false                 | Whether to enable dynamic scaling, that is, whether to allow DLI to adjust the resources used by jobs based on job loads and job priorities.                                                                                                                     |
      |                              |                       |                                                                                                                                                                                                                                                                  |
      |                              |                       | If this parameter is set to **false**, the function is disabled.                                                                                                                                                                                                 |
      |                              |                       |                                                                                                                                                                                                                                                                  |
      |                              |                       | If this parameter is set to **true**, the function is enabled.                                                                                                                                                                                                   |
      |                              |                       |                                                                                                                                                                                                                                                                  |
      |                              |                       | The default value is **false**.                                                                                                                                                                                                                                  |
      +------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | flink.dli.job.scale.interval | 30                    | Interval for checking whether to scale the resources for the current job, in minutes. The default value is **30**. For example, **30** indicates that the job is checked every 30 minutes to determine whether to scale in or out the resources used by the job. |
      |                              |                       |                                                                                                                                                                                                                                                                  |
      |                              |                       | Note: This configuration is effective only when dynamic scaling is enabled.                                                                                                                                                                                      |
      +------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | flink.dli.job.cu.max         | Initial CU value      | Maximum number of CUs that can be used by the current job during dynamic scaling. If this parameter is not set, the default value is the initial total number of CUs of the job.                                                                                 |
      |                              |                       |                                                                                                                                                                                                                                                                  |
      |                              |                       | Note: The value of this parameter cannot be smaller than the total number of CUs configured by the user. In addition, this parameter is effective only when dynamic scaling is enabled.                                                                          |
      +------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | flink.dli.job.cu.min         | 2                     | Minimum number of CUs that can be used by the current job during dynamic scaling. The default value is **2**.                                                                                                                                                    |
      |                              |                       |                                                                                                                                                                                                                                                                  |
      |                              |                       | Note: The value of this parameter cannot be greater than the total number of CUs configured by the user. In addition, this parameter is effective only when dynamic scaling is enabled.                                                                          |
      +------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
