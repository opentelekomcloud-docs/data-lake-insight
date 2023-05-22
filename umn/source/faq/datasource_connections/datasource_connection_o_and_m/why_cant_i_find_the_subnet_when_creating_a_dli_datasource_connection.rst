:original_name: dli_03_0111.html

.. _dli_03_0111:

Why Can't I Find the Subnet When Creating a DLI Datasource Connection?
======================================================================

When you create a VPC peering connection for the datasource connection, the following error information is displayed:

.. code-block::

   Failed to get subnet 2c2bd2ed-7296-4c64-9b60-ca25b5eee8fe. Response code : 404, message : {"code":"VPC.0202","message":"Query resource by id 2c2bd2ed-7296-4c64-9b60-ca25b5eee8fe fail.the subnet could not be found."}

Before you create a datasource connection, check whether **VPC Administrator** is selected. If only the global **Tenant Administrator** is selected, the system cannot find the subnet.
