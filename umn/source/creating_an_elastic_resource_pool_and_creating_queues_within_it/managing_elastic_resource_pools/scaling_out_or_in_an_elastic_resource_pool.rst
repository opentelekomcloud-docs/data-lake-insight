:original_name: dli_01_0686.html

.. _dli_01_0686:

Scaling Out or In an Elastic Resource Pool
==========================================

Scaling out or in an elastic resource pool essentially means adjusting the actual CUs of the resource pool.

In an elastic resource pool:

-  Actual CUs: actual size of resources currently allocated to the elastic resource pool (in CUs). That is, the number of compute resources actually owned.
-  Elastic resource pool scaling out: It involves increasing the actual CUs, meaning increasing the number of compute resources within the resource pool. The upper limit for this expansion corresponds to the maximum value specified in the CU range of the elastic resource pool.
-  Elastic resource pool scaling in: It involves decreasing the actual CUs, meaning decreasing the number of compute resources within the resource pool. The lower limit for this reduction corresponds to the minimum value specified in the CU range of the elastic resource pool.

That is, the actual CUs of the elastic resource pool dynamically change between the minimum value and the maximum value of the CU range.

For more information about basic concepts of elastic resource pools, see :ref:`Basic Concepts <dli_07_0003>`.

Notes and Constraints
---------------------

-  Creating or deleting queues within an elastic resource pool triggers elastic resource scaling.
-  Scaling in an elastic resource pool may affect nodes containing shuffle data, leading to the recomputation of Spark tasks. This causes automatic retries for Spark and SQL jobs, and if the retries exceed the limit, the job execution fails, requiring you to rerun the job.
-  Spark 2.3 jobs need to be upgraded to a later Spark version to support dynamic scale-in of the jobs while they are running.
-  Spark Streaming and Flink jobs cannot be scaled in while they are running. To perform a scale-in, suspend the jobs or migrate them to another elastic resource pool.
-  Scale-out and scale-in triggered by the following operations take effect at the next full hour after the operations are performed:

   -  Adjusting the CU range of a queue
   -  Modifying the specifications of an elastic resource pool
   -  Setting CUs for an elastic resource pool

-  You can add queues to adjust the actual CUs of an elastic resource pool. The adjustment takes effect immediately.

Triggering Methods for Scaling Out or In an Elastic Resource Pool
-----------------------------------------------------------------

.. table:: **Table 1** Triggering methods for scaling out or in an elastic resource pool

   +-------------+----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operation   | Actual CU Change     | Trigger Method                                                                                                                                                                        |
   +=============+======================+=======================================================================================================================================================================================+
   | Scaling out | Actual CUs increase. | max{(min[sum(maximum CUs of queues), maximum CUs of the elastic resource pool]), minimum CUs of the elastic resource pool} > current actual CUs, the system automatically scales out. |
   +-------------+----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Scaling in  | Actual CUs decrease. | max{(min[sum(maximum CUs of queues), maximum CUs of the elastic resource pool]), minimum CUs of the elastic resource pool} < current actual CUs, the system automatically scales in.  |
   +-------------+----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

For details, see :ref:`Figure 1 <dli_01_0686__fig194081836134720>`.

Formula for Calculating Actual CUs of an Elastic Resource Pool
--------------------------------------------------------------

The actual CUs of an elastic resource pool must meet the following requirements:

-  Meet the demands of all queues.
-  Ensure that it does not exceed the upper limit of the CUs of the elastic resource pool.
-  Ensure that it is not below the lower limit of the CUs of the elastic resource pool.

.. _dli_01_0686__fig194081836134720:

.. figure:: /_static/images/en-us_image_0000002363450016.png
   :alt: **Figure 1** Formula for calculating actual cus of an elastic resource pool

   **Figure 1** Formula for calculating actual cus of an elastic resource pool
