:original_name: dli_09_0195.html

.. _dli_09_0195:

Troubleshooting
===============

A Spark Job Fails to Be Executed and "No respond" Is Displayed in the Job Log
-----------------------------------------------------------------------------

-  Symptom

   A Spark job fails to be executed and "No respond" is displayed in the job log.

-  Solution

   Create a Spark job again. When creating the job, add the **spark.sql.mrs.opentsdb.ssl.enabled=true** configuration item to the **Spark parameters (--conf)**.
