:original_name: dli_01_0419.html

.. _dli_01_0419:

DLI Agency Overview
===================

What Is an Agency?
------------------

Cloud services often interact with each other, with some of which dependent on other services. You can create an agency to delegate DLI to use other cloud services and perform resource O&M on your behalf.

For example, the AK/SK required by DLI Flink jobs is stored in DEW. To allow DLI to access DEW data during job execution, you need to provide an IAM agency to delegate the permissions to perform operations on DEW data to DLI.


.. figure:: /_static/images/en-us_image_0000001742695104.png
   :alt: **Figure 1** DLI service agency

   **Figure 1** DLI service agency

DLI Agencies
------------

Before using DLI, you are advised to set up DLI agency permissions to ensure the proper functioning of DLI.

-  By default, DLI provides the following agencies: **dli_admin_agency**, **dli_management_agency**, and **dli_data_clean_agency**. The names of these agencies are fixed, but the permissions contained in them can be customized. In other scenarios, you need to create custom agencies. For details about the agencies, see :ref:`Table 1 <dli_01_0419__table966993514116>`.

-  DLI upgrades **dli_admin_agency** to **dli_management_agency** to meet the demand for fine-grained agency permissions management. The new agency has the necessary permissions for datasource operations, notifications, and user authorization operations. For details, see :ref:`Configuring DLI Agency Permissions <dli_01_0618>`.

-  To use Flink 1.15, Spark 3.3.1 (Spark general queue scenario), or a later version to execute jobs, perform the following operations:

   Create an agency on the IAM console and add the agency information to the job configuration. For details, see :ref:`Creating a Custom DLI Agency <dli_01_0616>`.

   -  Common scenarios for creating an agency: DLI is allowed to read and write data from and to OBS, dump logs, and read and write Flink checkpoints. DLI is allowed to access DEW to obtain data access credentials and access catalogs to obtain metadata.
   -  You cannot use the default agency names **dli_admin_agency**, **dli_management_agency**, or **dli_data_clean_agency**. It must be unique.

-  If the engine version is earlier than Flink 1.15, **dli_admin_agency** is used by default during job execution. If the engine version is earlier than Spark 3.3.1, user authentication information (AK/SK and security token) is used during job execution.

   This means that jobs whose engine versions are earlier than Flink 1.15 or Spark 3.3.1 are not affected by the update of agency permissions and do not require custom agencies.

-  To maintain compatibility with existing job agency permission requirements, **dli_admin_agency** will still be listed in the IAM agency list even after the update.

.. note::

   -  Only the tenant account or a member account of user group **admin** can authorize the service.
   -  Do not delete the agency created by the system by default.

.. _dli_01_0419__table966993514116:

.. table:: **Table 1** DLI agencies

   +-----------------------+-------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Permission            | Type                                                  | Description                                                                                                                                                                                                                                                                                                                                                                                      |
   +=======================+=======================================================+==================================================================================================================================================================================================================================================================================================================================================================================================+
   | dli_admin_agency      | Default agency                                        | This agency has been discarded and is not recommended. Upgrade the agency to **dli_management_agency** as soon as possible.                                                                                                                                                                                                                                                                      |
   |                       |                                                       |                                                                                                                                                                                                                                                                                                                                                                                                  |
   |                       |                                                       | For details about how to update an agency, see :ref:`Configuring DLI Agency Permissions <dli_01_0618>`.                                                                                                                                                                                                                                                                                          |
   +-----------------------+-------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dli_management_agency | Default agency                                        | DLI system agency, which is used to delegate operation permissions to DLI so that DLI can use other cloud services and perform resource O&M operations on your behalf. This agency grants permissions for datasource operations, message notifications, and user authorization operations. For details about the permissions of an agency, see :ref:`Table 2 <dli_01_0419__table1165153511519>`. |
   +-----------------------+-------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dli_data_clean_agency | Default agency, which needs to be authorized by users | Data cleanup agency, which is used to clean up data according to the lifecycle of a table and clean up lakehouse table data. You need to create a DLI agency named **dli_data_clean_agency** on IAM and grant permissions to it.                                                                                                                                                                 |
   |                       |                                                       |                                                                                                                                                                                                                                                                                                                                                                                                  |
   |                       |                                                       | You need to create an agency and customize permissions for it. However, the agency name is fixed to **dli_data_clean_agency**.                                                                                                                                                                                                                                                                   |
   |                       |                                                       |                                                                                                                                                                                                                                                                                                                                                                                                  |
   |                       |                                                       | For details about the permission policies of an agency, see :ref:`Agency Permission Policies in Common Scenarios <dli_01_0617>`.                                                                                                                                                                                                                                                                 |
   +-----------------------+-------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Other custom agencies | Custom agency                                         | When using Flink 1.15, Spark 3.3, or a later version to execute jobs, create an agency on the IAM console and add new agency information to the job configuration. For details, see :ref:`Creating a Custom DLI Agency <dli_01_0616>`.                                                                                                                                                           |
   |                       |                                                       |                                                                                                                                                                                                                                                                                                                                                                                                  |
   |                       |                                                       | Common scenarios for creating an agency: DLI is allowed to read and write data from and to OBS to transfer logs. DLI is allowed to access DEW to obtain data access credentials and access catalogs to obtain metadata.                                                                                                                                                                          |
   |                       |                                                       |                                                                                                                                                                                                                                                                                                                                                                                                  |
   |                       |                                                       | You cannot use the default agency names **dli_admin_agency**, **dli_management_agency**, or **dli_data_clean_agency**. It must be unique.                                                                                                                                                                                                                                                        |
   |                       |                                                       |                                                                                                                                                                                                                                                                                                                                                                                                  |
   |                       |                                                       | For details about the permission policies of an agency, see :ref:`Agency Permission Policies in Common Scenarios <dli_01_0617>`.                                                                                                                                                                                                                                                                 |
   +-----------------------+-------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0419__table1165153511519:

.. table:: **Table 2** Permissions contained in the dli_management_agency agency

   +------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Policy                                   | Description                                                                                                                                                                  |
   +==========================================+==============================================================================================================================================================================+
   | IAM ReadOnlyAccess                       | To authorize IAM users who have not logged in to DLI, you need to obtain their information. So, the permissions contained in the **IAM ReadOnlyAccess** policy are required. |
   +------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DLI Datasource Connections Agency Access | Permissions to access and use VPCs, subnets, routes, and VPC peering connections                                                                                             |
   +------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DLI Notification Agency Access           | Permissions to send notifications through SMN when a job fails to be executed                                                                                                |
   +------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
