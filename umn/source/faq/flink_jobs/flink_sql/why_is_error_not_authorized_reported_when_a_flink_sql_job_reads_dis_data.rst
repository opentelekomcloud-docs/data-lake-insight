:original_name: dli_03_0176.html

.. _dli_03_0176:

Why Is Error "Not authorized" Reported When a Flink SQL Job Reads DIS Data?
===========================================================================

Symptom
-------

Semantic verification for a Flink SQL job (reading DIS data) fails. The following information is displayed when the job fails:

.. code-block::

   Get dis channel xxx info failed. error info: Not authorized, please click the overview page to do the authorize action

Possible Causes
---------------

Before running a Flink job, the user account was not authorized to access DIS data.

Solution
--------

#. Log in to the DLI management console. Choose **Global Configuration** > **Service Authorization** in the navigation pane on the left.
#. On the **Service Authorization** page, select **DIS Administrator** and click **Update**.
#. Choose **Job Management** > **Flink Jobs**. On the displayed page, locate the desired Flink SQL job and restart the job.
