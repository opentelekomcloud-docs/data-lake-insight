:original_name: dli_03_0195.html

.. _dli_03_0195:

Why Does the System Display a Message Indicating Insufficient Permissions When I Update a Program Package?
==========================================================================================================

Symptom
-------

When the user update an existing program package, the following error information is displayed:

.. code-block::

   "error_code"*DLI.0003","error_msg":"Permission denied for resource 'resources. xxx', User = 'xxx', Action = "UPDATE_RESOURCE'."

Solution
--------

You need to assign the package permission to the user who executes the job. The procedure is as follows:

#. In the left navigation pane of the DLI management console, choose **Data Management** > **Package Management**.
#. On the **Package Management** page, click **Manage Permission** in the **Operation** column of the package. The **User Permissions** page is displayed.
#. Click **Grant Permission** in the upper right corner of the page to authorize a user to access a package group or package. Select the **Update Group** permission.
#. Click **OK**.
