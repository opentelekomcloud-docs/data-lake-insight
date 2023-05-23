:original_name: dli_03_0073.html

.. _dli_03_0073:

How Do I Set the Timeout Duration for Querying SQL Job Results Using SDK?
=========================================================================

When you query the SQL job results using SDK, the system checks the job status when the job is submitted. The timeout interval set in the system is 300s. If the job is not in the **FINISHED** state, a timeout error is reported after 300s.

You are advised to use **getJobId()** to obtain the job ID and then call **queryJobResultInfo(String jobId)** or **cancelJob(String jobId)** to obtain the result or cancel the job.
