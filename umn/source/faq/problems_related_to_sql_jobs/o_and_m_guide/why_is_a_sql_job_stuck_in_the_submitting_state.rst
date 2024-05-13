:original_name: dli_03_0213.html

.. _dli_03_0213:

Why Is a SQL Job Stuck in the Submitting State?
===============================================

The possible causes and solutions are as follows:

-  After you purchase a DLI queue and submit a SQL job for the first time, wait for 5 to 10 minutes. After the cluster is started in the background, the submission will be successful.
-  If the network segment of the queue is changed, wait for 5 to 10 minutes and then submit the SQL job immediately. After the cluster is re-created in the background, the submission is successful.
-  The queue is idle for more than one hour, and background resources have been released. You must wait for 5 to 10 minutes and then submit the SQL job. After the cluster is restarted in the background, the submission will be successful.
