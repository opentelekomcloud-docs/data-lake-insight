:original_name: dli_03_0036.html

.. _dli_03_0036:

Why Is a Message Displayed Indicating That the SMN Topic Does Not Exist When I Use the SMN Topic in DLI?
========================================================================================================

When you set running parameters of a DLI Flink job, you can enable **Alarm Generation upon Job Exception** to receive alarms when the job runs abnormally or is in arrears.

If a message is displayed indicating that the SMN topic does not exist, perform the following steps:

#. Check whether the SMN topic has been created.

   If not, create a topic on the SMN console.

#. Check IAM permissions.

   If the SMN topic already exists but the system still prompts that it does not exist, go to the IAM service, select the user group where the corresponding IAM user is, and ensure that the SMN policy for the corresponding region has been added to the user group.

#. Confirm the topic name and region.

   Make sure that the SMN topic name and region configured in DLI match the actual SMN topic created. If the SMN topic name is not consistent, the system will prompt that the SMN topic does not exist.
