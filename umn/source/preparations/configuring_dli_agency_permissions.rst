:original_name: dli_01_0618.html

.. _dli_01_0618:

Configuring DLI Agency Permissions
==================================

To use DLI, you must first configure permissions.

This section applies to the following scenarios:

-  **If you use DLI for the first time, configure DLI agency permissions by referring to this section.**

   DLI needs to work with other cloud services. You must grant DLI basic operation permissions of these services so that DLI can access them and perform resource O&M operations on your behalf.

-  **If you are still using the previous-generation agency dli_admin_agency, update it by referring to this section.**

   To balance practical business needs with the risk of excessive delegation, DLI upgraded its system agency to achieve more granular control over permissions. The previous **dli_admin_agency** was upgraded to **dli_management_agency**, which includes permissions for accessing IAM user information, datasource operations, and message notifications. This effectively prevents uncontrolled permission issues related to the services associated with DLI. After the upgrade, the DLI agency is more flexible and more suitable for scenario-based agency customization for medium- and large-sized enterprises.

After agency permissions are configured, the **dli_management_agency** agency is generated on the **Agencies** page of the IAM console. Do not delete this default system agency. Otherwise, the permissions included in the agency will be automatically revoked. The system cannot obtain IAM user information, access network resources required by datasource connections, or access SMN to send notifications.

Notes and Constraints
---------------------

-  Only the tenant account or a member account of user group **admin** can authorize the service.
-  DLI authorization needs to be conducted by project. The permissions of required agencies must be updated separately in each project. This means you need to switch to the corresponding project and then update the agency by following the instructions provided in this section.

Updating DLI Agency Permissions (dli_management_agency)
-------------------------------------------------------

#. In the navigation pane of the DLI console, choose **Global Configuration** > **Service Authorization**.

#. On the displayed page, select permissions for scenarios.

   Click |image1| on a permission card to view its detailed permission policies.

   :ref:`Table 1 <dli_01_0618__table1165153511519>` describes these agencies.

   .. _dli_01_0618__table1165153511519:

   .. table:: **Table 1** Permissions contained in the dli_management_agency agency

      +-----------------------+------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Use Case              | Agency                                   | Description                                                                                                                                                                                                      |
      +=======================+==========================================+==================================================================================================================================================================================================================+
      | Basic usage           | IAM ReadOnlyAccess                       | To authorize IAM users who have not logged in to DLI, you need to obtain their information. So, the permissions contained in the **IAM ReadOnlyAccess** policy are required.                                     |
      |                       |                                          |                                                                                                                                                                                                                  |
      |                       |                                          | **IAM ReadOnlyAccess** is a global policy. Make sure you select this policy. If you do not select it, all its permissions will become invalid in all regions, and the system cannot obtain IAM user information. |
      +-----------------------+------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Datasource            | DLI Datasource Connections Agency Access | Permissions to access and use VPCs, subnets, routes, and VPC peering connections                                                                                                                                 |
      +-----------------------+------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | O&M                   | DLI Notification Agency Access           | Permissions to send notifications through SMN when a job fails to be executed                                                                                                                                    |
      +-----------------------+------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      Among the permissions contained in **dli_management_agency**:

      -  The authorization scope of the **IAM ReadOnlyAccess** policy covers all global service resources in all regions.

         -  If you select this policy when updating a DLI agency in any region, this policy's permissions apply to the projects in all regions.
         -  If you do not select this policy when updating an agency in any project, this policy's permissions will be revoked from all regions. This means that all projects cannot obtain IAM user information.

      -  The authorization scope of the **DLI Datasource Connections Agency Access** and **DLI Notification Agency Access** policies covers the project resources in specified regions.

         These policies' permissions only apply to projects for which these policies are selected and the DLI agency permissions are updated. Projects for which these policies are not selected do not have the permissions required in datasource scenarios and the permission to send notifications using SMN.

#. Select the policies to be included in **dli_management_agency** and click **Update**.

#. View and understand the notes for updating the agency and click **OK**. The DLI agency permissions are updated.

   -  The system upgrades your **dli_admin_agency** to **dli_management_agency**.
   -  To maintain compatibility with existing job agency permission requirements, **dli_admin_agency** will still be listed in the IAM agency list even after the update.
   -  Do not delete the agency created by the system by default.

Follow-Up Operations
--------------------

In addition to the permissions provided by **dli_management_agency**, you need to create an agency on the IAM console and add information about the new agency to the job configuration for scenarios like allowing DLI to read and write data from and to OBS to transfer logs, or allowing DLI to access DEW to obtain data access credentials. For details, see :ref:`Creating a Custom DLI Agency <dli_01_0616>` and :ref:`Agency Permission Policies in Common Scenarios <dli_01_0617>`.

-  When Flink 1.15, Spark 3.3.1 (Spark general queue scenario), or a later version is used to execute jobs, you need to create an agency on the IAM console.

-  If the engine version is earlier than Flink 1.15, **dli_admin_agency** is used by default during job execution. If the engine version is earlier than Spark 3.3.1, user authentication information (AK/SK and security token) is used during job execution.

   This means that jobs whose engine versions are earlier than Flink 1.15 or Spark 3.3.1 are not affected by the update of agency permissions and do not require custom agencies.

**Common service scenarios where you need to create an agency:**

-  Data cleanup agency required for clearing data according to the lifecycle of a table and clearing lakehouse table data. You need to create a DLI agency named **dli_data_clean_agency** on IAM and grant permissions to it. You need to create an agency and customize permissions for it. However, the agency name is fixed to **dli_data_clean_agency**.
-  **Tenant Administrator** permissions are required to access data from OBS to execute Flink jobs on DLI, for example, obtaining OBS data sources, log dump (including bucket authorization), checkpointing enabling, and job import and export.
-  The AK/SK required by DLI Flink jobs is stored in DEW. To allow DLI to access DEW data during job execution, you need to create an agency to delegate the permissions to operate on DEW data to DLI.
-  To allow DLI to access DLI catalogs to retrieve metadata when executing jobs, you need to create a new agency that grants DLI catalog data operation permissions to DLI. This will enable DLI to access DLI catalogs on your behalf.
-  Cloud data required by DLI Flink jobs is stored in LakeFormation. To allow DLI to access catalogs to retrieve metadata during job execution, you need to create an agency to delegate the permissions to operate on catalog data to DLI.

When creating an agency, you cannot use the default agency names **dli_admin_agency**, **dli_management_agency**, or **dli_data_clean_agency**. It must be unique.

For more information about custom agency operations, see :ref:`Creating a Custom DLI Agency <dli_01_0616>` and :ref:`Agency Permission Policies in Common Scenarios <dli_01_0617>`.

.. |image1| image:: /_static/images/en-us_image_0000001838073572.png
