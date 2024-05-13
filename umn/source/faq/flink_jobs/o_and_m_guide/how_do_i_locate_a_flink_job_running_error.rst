:original_name: dli_03_0105.html

.. _dli_03_0105:

How Do I Locate a Flink Job Running Error?
==========================================

#. On the Flink job management, click **Edit** in the **Operation** column of the target job. On the displayed page, check whether **Save Job Log** in the **Running Parameters** tab is enabled.

   -  If the function is enabled, go to :ref:`3 <dli_03_0105__en-us_topic_0000001082261254_li431316504547>`.
   -  If the function is disabled, running logs will not be dumped to an OBS bucket. In this case, perform :ref:`2 <dli_03_0105__en-us_topic_0000001082261254_li1615412509238>` to save job logs.

#. .. _dli_03_0105__en-us_topic_0000001082261254_li1615412509238:

   On the job running page, select **Save Job Log** and specify an OBS bucket for storing the logs. Click **Start** to run the job again. After the executed is complete, perform :ref:`3 <dli_03_0105__en-us_topic_0000001082261254_li431316504547>` and subsequent steps.

#. .. _dli_03_0105__en-us_topic_0000001082261254_li431316504547:

   In the Flink job list, click the job name. On the displayed job details page, click the **Run Log** tab.

#. Click **view OBS Bucket** to obtain the complete run logs of the job.

#. Download the latest **jobmanager.log** file, search for the keyword **RUNNING to FAILED**, and determine the failure cause based on the errors in the context.

#. If the information in the **jobmanager.log** file is insufficient for locating the fault, find the corresponding **taskmanager.log** file in the run logs and search for the keyword **RUNNING to FAILED** to confirm the failure cause.
