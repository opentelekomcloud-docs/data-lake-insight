:original_name: dli_03_0235.html

.. _dli_03_0235:

Why Is Information Displayed on the FlinkUI/Spark UI Page Incomplete?
=====================================================================

Symptom
-------

The Flink/Spark UI was displayed with incomplete information.

Possible Causes
---------------

When the queue is used to run a job, the system releases the cluster and takes about 10 minutes to create a new one. Accessing the Flink UI before completion of the creation will empty the project ID in the cache. As a result, the UI cannot be displayed. The possible cause is that the cluster was not created.

Solution
--------

Change the queue to dedicated, so that the cluster will not be released when the queue is idle. Alternatively, submit a job, wait for a while, and then access FlinkUI.
