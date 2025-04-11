:original_name: dli_01_0665.html

.. _dli_01_0665:

Connecting DLI to AOM's Prometheus Monitoring
=============================================

AOM's Prometheus Monitoring is a comprehensive solution that integrates with the open source Prometheus ecosystem. It supports monitoring of various types of components, provides pre-configured monitoring dashboards, and offers fully managed Prometheus services. It uses Prometheus monitoring to collect, store, and display data from monitored objects, making it suitable for collecting and processing time series databases, especially for monitoring Flink jobs.

This section describes how to connect DLI to AOM Prometheus for monitoring.

Notes
-----

-  Only Flink 1.15 supports the connection to AOM's Prometheus Monitoring.

-  A general-purpose AOM Prometheus cluster has been created.

-  Only common AOM Prometheus instances are supported.

-  After an elastic resource pool is connected to a Prometheus instance, the metrics of all newly submitted Flink 1.15 jobs in the elastic resource pool are reported to the bound Prometheus instance. Only basic metrics are reported by default. To report all metrics, set the **metrics.reporter.remote.report-all-metrics** parameter in :ref:`Configuration Items for Connecting DLI to AOM's Prometheus Monitoring <dli_01_0666>`.

-  The default reporting interval for DLI Flink metrics is 30 seconds, resulting in a slight delay in metric reporting. To adjust the reporting interval, set the **metrics.reporter.remote.interval** parameter in :ref:`Configuration Items for Connecting DLI to AOM's Prometheus Monitoring <dli_01_0666>`.

   To prevent frequent metric reporting, you are advised not to set this parameter to a small value. The recommended interval is 30 seconds.

-  In Flink 1.15 or later, after an elastic resource pool is unbound from a Prometheus instance, metrics for new jobs are no longer reported to that instance. However, metrics for submitted jobs will continue to be reported until those jobs are completed.

-  In Flink 1.15 or later, if the bound Prometheus instance is changed, metrics for new jobs will be reported to the new instance. However, metrics for submitted jobs will continue to be reported to the original instance until those jobs are completed.

.. _dli_01_0665__section13881657124013:

Step 1: Create an AOM Prometheus Instance
-----------------------------------------

#. Log in to the AOM 2.0 management console.
#. In the navigation pane on the left, choose **Prometheus Monitoring** > **Instances**. On the displayed page, click **Add Prometheus Instance**.
#. Set the instance name, enterprise project, and instance type.

   .. table:: **Table 1** Creating a Prometheus instance

      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                     |
      +===================================+=================================================================================================================================================================================================+
      | Instance Name                     | Prometheus instance name.                                                                                                                                                                       |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Enterprise Project                | Enterprise project name.                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                 |
      |                                   | -  If you have not changed the default value **ALL** for **Enterprise Project** in the left navigation pane of the console, select an enterprise project from the drop-down list on this panel. |
      |                                   | -  If you have manually chosen a value other than **ALL** for **Enterprise Project**, this parameter is automatically populated and cannot be modified.                                         |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Instance Type                     | Type of the Prometheus instance. Select **Common Prometheus Instance**.                                                                                                                         |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Step 2: Bind an Elastic Resource to an AOM Prometheus Cluster
-------------------------------------------------------------

#. Log in to the DLI management console. In the navigation pane on the left, choose **Resources** > **Resource Pool**.

#. Locate the elastic resource pool you want to bind, click **More** in the **Operation** column, and select **Prometheus** > **Bind Prometheus Instance**.

#. In the dialog box that appears, select the Prometheus cluster created in :ref:`Step 1: Create an AOM Prometheus Instance <dli_01_0665__section13881657124013>`.

#. Click **OK**.

   Once bound, monitoring metrics for newly submitted jobs are reported to AOM and billed based on AOM billing rules.

   .. note::

      To bind an elastic resource pool to a Prometheus instance, you must have the permission to access AOM Prometheus. Otherwise, your binding fails.

      The permissions include:

      -  aom:prometheusInstances:list
      -  aom:metric:list
      -  aom:metric:get

Step 3: Create and Submit a Flink Job
-------------------------------------

Create a Flink job by referring to :ref:`Creating a Flink OpenSource SQL Job <dli_01_0498>`.

Set **Flink Version** to **1.15**. Only Flink 1.15 or later supports AOM monitoring.

After approximately 30 seconds of running the job, the system reports the job's monitoring metrics to the AOM Prometheus instance.

Step 4: View Monitoring Metrics on the AOM Dashboard
----------------------------------------------------

For the Prometheus monitoring metrics supported by DLI, see :ref:`Basic Monitoring Metrics Reported by DLI to Prometheus <dli_01_0667>`.

Open an AOM dashboard to view monitoring metrics. You can use either of the following methods to switch to the AOM console:

-  Method 1: Switching from the DLI management console to AOM's **Dashboard** page

   #. Log in to the DLI management console. In the navigation pane on the left, choose **Job Management** > **Flink Jobs**.

   #. On the displayed page, click the name of the job whose monitoring metrics you want to view.

   #. On the **Job Details** tab, click **More** in the upper right corner and select **Prometheus Monitoring**.

      AOM's **Dashboard** page is displayed.

-  Method 2: Viewing monitoring metrics on AOM's preset dashboard

   #. Log in to the AOM 2.0 management console.

   #. In the navigation pane on the left, choose **Dashboard** > **Dashboard**.

      In the navigation pane of the **Dashboard** page, choose **System** > **Applications**. On the displayed page, find the dashboard with type of **DLI_FLINK**.

   #. Click the name of the dashboard to access its details.

   #. Configure filter criteria to view detailed monitoring metrics.

      By default, all metrics of the current Prometheus instance is displayed. To view metrics for a specific elastic resource pool, job, or even a specific job submission, you need to apply filters based on your specific needs.

      .. table:: **Table 2** Monitoring metrics

         +-----------------------------------+----------------------------------------------------------------------------------------------------------+
         | Filter                            | Description                                                                                              |
         +===================================+==========================================================================================================+
         | Prometheus Instance               | If you filter metrics by Prometheus instance, all metrics of the instance are displayed.                 |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------+
         | Resource Pool                     | If you filter metrics by elastic resource pool, all metrics of the elastic resource pool are displayed.  |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------+
         | DLI-flink-jobId                   | If you filter metrics by DLI Flink job ID, all metrics of the current DLI Flink job are displayed.       |
         |                                   |                                                                                                          |
         |                                   | You can query the DLI Flink job ID on the **Job Management** > **Flink Jobs** page of the DLI console.   |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------+
         | DLI-flink-job name                | If you filter metrics by DLI Flink job name, all metrics of the current DLI Flink job are displayed.     |
         |                                   |                                                                                                          |
         |                                   | You can query the DLI Flink job name on the **Job Management** > **Flink Jobs** page of the DLI console. |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------+
         | jobId                             | If you filter metrics by DLI Flink job ID, you can only view the metrics of the current Flink job.       |
         |                                   |                                                                                                          |
         |                                   | -  View the job ID through the FlinkUI.                                                                  |
         |                                   | -  View the job ID by searching for keywords in the Flink JobManager log.                                |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------+

(Optional) Step 5: Configure Alarm Notifications for Prometheus Monitoring
--------------------------------------------------------------------------

To stay updated on the monitoring status of Prometheus and take prompt action, you need to configure alarm notifications. SMN offers flexible message push capabilities, allowing you to send Prometheus alarm events to various endpoints, enabling multi-channel alarm notifications. This step walks you through how to configure alarm notifications for Prometheus monitoring.

#. **Create an SMN topic and add a subscription to the topic.**

   a. Create an SMN topic.

      #. Log in to the SMN management console.

      #. In the navigation pane on the left, choose **Topic Management** > **Topics**.

      #. On the displayed **Topics** page, click **Create Topic** in the upper right corner.

      #. Set topic parameters.

         Set **Topic Name** and **Display Name**.

      #. In **Topic Name**, enter a topic name. In **Display Name**, enter a display name.

   b. Add a subscription to the topic.

      To receive messages published to a topic, you need to add subscription endpoints to that topic.

      #. Log in to the SMN management console.

      #. In the navigation pane on the left, choose **Topic Management** > **Topics**.

      #. On the displayed **Topics** page, locate the topic to which you want to add a subscription and click **Add Subscription** in the **Operation** column.

      #. In the **Add Subscription** slide-out panel, select a protocol from the **Protocol** drop-down list.

      #. In **Endpoint**, enter a subscription endpoint.

         Once you have added a subscription, SMN will send a confirmation message to the subscription endpoint, which includes a link for confirming the subscription. The subscription confirmation link is valid for 48 hours. Make sure to confirm your subscription on your mobile phone, mailbox, or other endpoints within this time frame.

#. .. _dli_01_0665__li9307448197:

   **Create an alarm action rule on the AOM management console.**

   You can create an alarm action rule and associate it with an SMN topic and a message template. If the log, resource, or metric data meets the alarm condition, the system sends an alarm notification based on the associated SMN topic and message template.

   Ensure that you have created an SMN topic and added subscriptions to the topic.

   a. Log in to the AOM 2.0 console.

   b. In the navigation pane on the left, choose **Alarm Management** > **Alarm Action Rules**.

   c. On the displayed **Action Rules** tab, click **Create**.

   d. In the slide-out panel, set the rule name, rule type, and action.

      When the alarm conditions corresponding to the resources are triggered, the system sends alarm notifications based on the associated SMN topics and message templates.

#. **Create a metric alarm rule.**

   You can set threshold conditions in metric alarm rules for resource metrics. When a metric value meets a threshold condition, a threshold alarm will be triggered. If there is no metric data available, an insufficient data event will be reported.

   The following uses creating a metric alarm rule from all metrics as an example.

   a. Log in to the AOM 2.0 console.
   b. In the navigation pane on the left, choose **Alarm Management** > **Alarm Rules**.
   c. Click **Create**.
   d. Set basic information and details about the alarm rule.

      -  When configuring alarm rules, the Prometheus instance selected should be the one associated with the elastic resource pool where the job requiring alarm notifications is located.
      -  Configure advanced settings. The **Action Taken for Insufficient Data** parameter in the **Advanced Settings** area is available only when **Select from all metrics** is selected for **Configuration Mode**. You are advised to enable **Action Taken for Insufficient Data**. This determines how the system will handle the absence or insufficient availability of metric data during the configured monitoring period.
      -  Action rule for alarm notifications: You are advised to enable **Action Rule** for alarm notification to ensure that alarms can be notified by email or SMS. Select the alarm action rule configured in :ref:`2 <dli_01_0665__li9307448197>`.
