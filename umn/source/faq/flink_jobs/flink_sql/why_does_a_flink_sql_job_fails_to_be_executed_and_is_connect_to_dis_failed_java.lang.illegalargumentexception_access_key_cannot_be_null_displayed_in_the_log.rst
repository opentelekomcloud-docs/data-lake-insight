:original_name: dli_03_0174.html

.. _dli_03_0174:

Why Does a Flink SQL Job Fails to Be Executed, and Is "connect to DIS failed java.lang.IllegalArgumentException: Access key cannot be null" Displayed in the Log?
=================================================================================================================================================================

Symptom
-------

After a Flink SQL job is submitted on DLI, the job fails to be executed. The following error information is displayed in the job log:

.. code-block::

   connect to DIS failed java.lang.IllegalArgumentException: Access key cannot be null

Possible Causes
---------------

When configuring job running parameters for the Flink SQL job, Save Job Log or Checkpointing is enabled, and an OBS bucket for saving job logs and Checkpoints is configured. However, the IAM user who runs the Flink SQL job does not have the OBS write permission.

Solution
--------

#. Log in to the IAM console, search for the IAM user who runs the job in the upper left corner of the **Users** page.
#. Click the desired username to view the user group where the user belongs.
#. In the navigation pane on the left, choose **User Groups**, and search for the user group of the target user. Click the user group name, and view the permissions of the current user in **Permissions**.
#. Check whether the user group has the permission to write data to OBS, for example, **OBS OperateAccess**. If the user group does not have the OBS write permission, grant the permission to the user group.
#. Wait for 5 to 10 minutes for the permission to take effect. Run the Flink SQL job again and check the job running status.
