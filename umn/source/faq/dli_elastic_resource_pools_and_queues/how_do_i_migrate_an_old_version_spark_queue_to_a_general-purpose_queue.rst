:original_name: dli_03_0065.html

.. _dli_03_0065:

How Do I Migrate an Old Version Spark Queue to a General-Purpose Queue?
=======================================================================

Currently, DLI provides two types of queues, **For SQL** and **For general use**. SQL queues are used to run SQL jobs. General-use queues are compatible with Spark queues of earlier versions and are used to run Spark and Flink jobs.

You can perform the following steps to convert an old Spark queue to a general purpose queue.

#. Purchase a general purpose queue again.
#. Migrate the jobs in the old Spark queue to the new general queue. That is, specify a new queue when submitting Spark jobs.
#. Release the old Spark queue, that is, delete it or unsubscribe it from the queue.
