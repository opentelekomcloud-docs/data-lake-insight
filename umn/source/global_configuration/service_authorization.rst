:original_name: dli_01_0486.html

.. _dli_01_0486:

Service Authorization
=====================

Prerequisites
-------------

Only the tenant account or a subaccount of user group **admin** can authorize access.

Procedure
---------

After entering the DLI management console, you are advised to set agency permissions to ensure that DLI can be used properly.

If you need to adjust the agency permissions, modify them on the **Service Authorization** page. For details about the required agency permissions, see :ref:`Table 1 <dli_01_0486__table57135458133>`.

#. Select required agency permissions and click **Update Authorization**. Only the tenant account or a subaccount of user group **admin** can authorize access. If the message "Agency permissions updated" is displayed, the update is successful.
#. Once service authorization has succeeded, an agency named **dli_admin_agency** on IAM will be created. Go to the agency list to view the details. Do not delete **dli_admin_agency**.

.. _dli_01_0486__table57135458133:

.. table:: **Table 1** DLI agency permissions

   +---------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------+
   | Permission                            | Details                                                                                                                                                                                                                                            | Remarks                                                                                                        |
   +=======================================+====================================================================================================================================================================================================================================================+================================================================================================================+
   | Tenant Administrator (global service) | **Tenant Administrator** permissions are required to access data from OBS to execute Flink jobs on DLI, for example, obtaining OBS/DWS data sources, log dump (including bucket authorization), checkpointing enabling, and job import and export. | Due to cloud service cache differences, permission setting operations require about 60 minutes to take effect. |
   +---------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------+
   | DIS Administrator                     | **DIS Administrator** permissions are required to use DIS data as the data source of DLI Flink jobs.                                                                                                                                               | Due to cloud service cache differences, permission setting operations require about 30 minutes to take effect. |
   +---------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------+
   | VPC Administrator                     | **VPC Administrator** permissions are required to use the VPC, subnet, route, VPC peering connection, and port for DLI datasource connections.                                                                                                     | Due to cloud service cache differences, permission setting operations require about 3 minutes to take effect.  |
   +---------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------+
   | SMN Administrator                     | To receive notifications when a DLI job fails, **SMN Administrator** permissions are required.                                                                                                                                                     | Due to cloud service cache differences, permission setting operations require about 3 minutes to take effect.  |
   +---------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------+
