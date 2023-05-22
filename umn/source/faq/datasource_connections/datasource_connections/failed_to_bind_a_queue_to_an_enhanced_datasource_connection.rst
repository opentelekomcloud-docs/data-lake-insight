:original_name: dli_03_0237.html

.. _dli_03_0237:

Failed to Bind a Queue to an Enhanced Datasource Connection
===========================================================

Symptom
-------

An enhanced datasource connection failed to pass the network connectivity test. Datasource connection cannot be bound to a queue. The following error information is displayed:

.. code-block::

   Failed to get subnet 86ddcf50-233a-449d-9811-cfef2f603213. Response code : 404, message : {"code":"VPC.0202","message":"Query resource by id 86ddcf50-233a-449d-9811-cfef2f603213 fail.the subnet could not be found."}

Cause Analysis
--------------

VPC Administrator permissions are required to use the VPC, subnet, route, VPC peering connection, and port for DLI datasource connections.

The binding fails because the user does not have the required VPC permissions.

Procedure
---------

On the DLI console, choose **Global Configuration** > **Service Authorization**, select the required VPC permission, and click **Update**.
