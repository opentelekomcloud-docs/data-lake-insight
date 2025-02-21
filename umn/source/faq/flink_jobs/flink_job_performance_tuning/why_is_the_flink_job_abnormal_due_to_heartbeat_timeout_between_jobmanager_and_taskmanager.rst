:original_name: dli_03_0236.html

.. _dli_03_0236:

Why Is the Flink Job Abnormal Due to Heartbeat Timeout Between JobManager and TaskManager?
==========================================================================================

Symptom
-------

JobManager and TaskManager heartbeats timed out. As a result, the Flink job is abnormal.


.. figure:: /_static/images/en-us_image_0000001391536818.png
   :alt: **Figure 1** Error information

   **Figure 1** Error information

Possible Causes
---------------

#. Check whether the network is intermittently disconnected and whether the cluster load is high.

#. If Full GC occurs frequently, check the code to determine whether memory leakage occurs.


   .. figure:: /_static/images/en-us_image_0000001441656525.png
      :alt: **Figure 2** Full GC

      **Figure 2** Full GC

Handling Procedure
------------------

-  If Full GC occurs frequently, check the code to determine whether memory leakage occurs.
-  Allocate more resources for a single TaskManager.

-  Contact technical support to modify the cluster heartbeat configuration.
