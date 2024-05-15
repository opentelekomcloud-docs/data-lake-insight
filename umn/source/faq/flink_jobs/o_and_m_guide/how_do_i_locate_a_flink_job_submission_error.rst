:original_name: dli_03_0103.html

.. _dli_03_0103:

How Do I Locate a Flink Job Submission Error?
=============================================

#. On the Flink job management page, hover the cursor on the status of the job that fails to be submitted to view the brief information about the failure.

   The possible causes are as follows:

   -  Insufficient CUs: Increase the number of CUs of the queue.
   -  Failed to generate the JAR file: Check the SQL syntax and UDFs.

#. If you cannot locate the fault or the call stack is incorrect, click the job name to go to the job details page and click the **Commit Logs** tab to view the job submission logs.
