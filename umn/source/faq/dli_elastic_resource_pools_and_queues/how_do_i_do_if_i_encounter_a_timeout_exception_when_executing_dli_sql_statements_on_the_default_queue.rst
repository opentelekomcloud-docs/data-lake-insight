:original_name: dli_03_0171.html

.. _dli_03_0171:

How Do I Do If I Encounter a Timeout Exception When Executing DLI SQL Statements on the default Queue?
======================================================================================================

Symptom
-------

After a SQL job was submitted to the default queue, the job runs abnormally. The job log reported that the execution timed out. The exception logs are as follows:

.. code-block::

   [ERROR] Execute DLI SQL failed. Please contact DLI service.
   [ERROR] Error message:Execution Timeout

Possible Causes
---------------

The default queue is a public preset queue in the system for function trials. When multiple users submit jobs to this queue, traffic control might be triggered. As a result, the jobs fail to be submitted.

Solution
--------

Buy a custom queue for your jobs.
