:original_name: dli_03_0233.html

.. _dli_03_0233:

Why Is Error Invalid OBS Bucket Name Reported After a Flink Job Submission Failed?
==================================================================================

Symptom
-------

The storage path of the Flink Jar job checkpoints was set to an OBS bucket. The job failed to be submitted, and an error message indicating an invalid OBS bucket name was displayed.

Possible Causes
---------------

#. Check that the OBS bucket name is correct.
#. Check that the AK/SK has the required permission.
#. Set the dependency to **provided** to prevent JAR file conflicts.
#. Check that the **esdk-obs-java-3.1.3.jar** version is used.
#. Confirm that the cluster configuration is faulty.

Procedure
---------

#. Set the dependency to **provided**.
#. Restart the **clusteragent** cluster after an upgrade to make the configuration take effect.
#. Remove the OBS dependency. Otherwise, the checkpoints cannot be written to OBS.
