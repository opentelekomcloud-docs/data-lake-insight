:original_name: dli_03_0138.html

.. _dli_03_0138:

Why Is a Message Displayed Indicating That the OBS Bucket Is Not Authorized When I Select an OBS Bucket for a Flink SQL Job?
============================================================================================================================

Symptom
-------

When you create a Flink SQL job and configure the parameters, you select an OBS bucket you have created. The system displays a message indicating that the OBS bucket is not authorized. After you click **Authorize**, the system displays a message indicating that an internal error occurred on the server and you need to contact customer service or try again later.

Solution
--------

On the settings page, press F12 to view the error details. The following is an example:

.. code-block::

   {"error_msg":"An internal error occurred. {0} Contact customer services or try again later ","error_json_opt":{"error": "Unexpected exception[NoSuchElementException: None.get]"},"error_code":"DLI.10001"}

Check whether a DLI agency has been created. If you do not have the permission to create an agency. On the DLI console, choose **Global Configuration** > **Service Authorization**, select **Tenant Administrator (Global service)**, and click **Update**.
