:original_name: dli_03_0183.html

.. _dli_03_0183:

How Do I Check for a Backlog of Jobs in the Current DLI Queue?
==============================================================

Symptom
-------

You need to check the large number of jobs in the **Submitting** and **Running** states on the queue.

Solution
--------

Use Cloud Eye to view jobs in different states on the queue. The procedure is as follows:

#. Log in the management console and search for Cloud Eye.
#. In the navigation pane on the left, choose **Cloud Service Monitoring** > **Data Lake Insight**.
#. On the **Cloud Service Monitoring** page, click the queue name.
#. On the monitoring page, view the following metrics to check the job status:

   a. Number of jobs being submitted: Statistics of jobs in the **Submitting** state on the current queue
   b. Number of running jobs: Statistics of jobs in the **Running** state on the current queue
   c. Number of finished jobs: Statistics of jobs in the **Finished** state on the current queue
