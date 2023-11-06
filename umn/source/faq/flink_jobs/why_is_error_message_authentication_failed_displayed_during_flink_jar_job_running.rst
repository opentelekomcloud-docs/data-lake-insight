:original_name: dli_03_0165.html

.. _dli_03_0165:

Why Is Error Message "Authentication failed" Displayed During Flink Jar Job Running?
====================================================================================

Symptom
-------

An exception occurred when a Flink Jar job is running. The following error information is displayed in the job log:

.. code-block::

   org.apache.flink.shaded.curator.org.apache.curator.ConnectionState - Authentication failed

Possible Causes
---------------

Service authorization is not configured for the account on the **Global Configuration** page. When the account is used to create a datasource connection to access external data, the access fails.

Solution
--------

#. Log in to the DLI management console. Choose **Global Configuration** > **Service Authorization** in the navigation pane.
#. On the **Service Authorization** page, select all agency permissions.
#. Click **Update**. If the message "Agency permissions updated successfully" is displayed, the modification is successful.
#. After the authorization is complete, create a datasource connection and run the job again.
