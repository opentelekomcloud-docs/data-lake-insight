:original_name: dli_01_0440.html

.. _dli_01_0440:

Overview of the DLI Permission System
=====================================

DLI features two sets of permission systems. These two permission control mechanisms are used in conjunction, with their permissions overlapping and complementing each other to provide a more comprehensive level of access control.

.. table:: **Table 1** DLI permission system types and descriptions

   +---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------+
   | Type                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Reference                     |
   +===========================+=====================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+===============================+
   | DLI permission management | DLI provides the capability to **control resource permissions** and **resource operation permissions** for IAM users, allowing different resources and operation permissions to be granted to various IAM users. For example, a specific IAM user can be granted permission only to query tables, while an administrator user may have multiple operational permissions such as querying tables, deleting tables, and accessing table metadata.                                                     | :ref:`Overview <dli_01_0690>` |
   |                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |                               |
   |                           | -  **Resource permission control**: Various resources within DLI (such as elastic resource pools, queues, databases, and tables) can be configured with different access permissions. These permissions can be finely tuned down to specific read/write operations. For example, an IAM user can be granted the permission to display a table but not the permission to query it.                                                                                                                   |                               |
   |                           | -  **Resource operation permission control**: DLI also controls the operations that users are permitted to perform. For example, an IAM user might be allowed to query a table but not to add columns to it.                                                                                                                                                                                                                                                                                        |                               |
   +---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------+
   | IAM permission management | IAM offers the capability for fine-grained permission management, allowing you to create and manage detailed permission policies. These policies can precisely define the operation permissions of users or user groups within DLI. For example, you can create a permission that allows the creation of DLI elastic resource pools, then bind this permission to a specific user group. As a result, all users added to that user group will have the permission to create elastic resource pools. | :ref:`Overview <dli_01_0417>` |
   +---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------+

DLI permission management focuses on the fine-grained control over internal resources and operations within DLI, while IAM permission management emphasizes control from the perspective of identity authentication and global permission policies. By combining both, comprehensive permission management can be achieved, spanning from user identities to specific resource operations.

For example, if user A in group A is granted the permission to delete database resources, but other users in group A do not have this permission, then when granting permissions to group A through IAM, the **dli:database:dropDatabase** permission for deleting databases would not be included. However, using the table permission management provided by the DLI management console, you can precisely grant only user A the permission to delete databases.

This example illustrates that the scope of the DLI permission system and the IAM authorization system are clearly distinguished, ensuring that these two permission control mechanisms operate independently without interfering with each other.

Basic Principles for Permission Management
------------------------------------------

-  Priority principle: If there is an explicit denial (Deny) permission, the authorization result should be denied regardless of other permissions.
-  Default deny: If there is no explicit allowance (Allow) permission, the authorization result should still be denied even if there is no denial permission.
-  Explicit allowance: The authorization result is only allowed when there is an explicit allowance permission.

How Does the Access Mechanism Work When There Is a Conflict Between Two Permissions?
------------------------------------------------------------------------------------

-  **When no policy grants the Allow permission, the default condition is to have the Deny permission. The Allow permission can take effect only when there is a policy that grants the Allow permission and no other policies deny this permission.**

   For example, if an Allow permission for creating tables already exists in an IAM authentication policy for a specific database DB1, adding another Allow permission for creating views will stack upon the existing permissions, thereby expanding the user's privileges.

   If a new policy is added with a Deny permission for adding or deleting databases, the user permissions will be adjusted according to the Deny precedence rule. Even if the operation permission to delete the database is granted through the DLI console, it will ultimately be overridden by the Deny precedence rule, denying the user the permission to delete the database.

-  **Based on the principle of least privilege, Deny always takes precedence over Allow.**

   For example, if an IAM user is granted permission to create tables in database DB1 on the DLI console, but the **dli:database:createTable** Deny permission is added in the IAM authentication policy, meaning the user is prohibited from creating data tables, then the user will ultimately be unable to access those data tables.


   .. figure:: /_static/images/en-us_image_0000002348096834.png
      :alt: **Figure 1** Two permission mechanisms of DLI

      **Figure 1** Two permission mechanisms of DLI

Permission Management Methods Supported by Different DLI Resource Types
-----------------------------------------------------------------------

DLI SQL resources are resources that can be created using SQL statements, such as databases and tables. To use SQL resources, you need to have both SQL operation permissions and SQL resource permissions. The SQL operation permissions grant the relevant permissions for using the DLI APIs associated with this type of resource.

-  **SQL resources**: Metadata, databases, tables (including views), and columns.

   Before submitting a DLI job, you must pre-grant IAM users the necessary permissions to access DLI metadata, databases, tables, and columns. This ensures that the job can smoothly access the required data and resources during execution.

-  **Other DLI resources**: Queues, elastic resource pools, jobs, global variables, packages, enhanced datasource connections, and datasource authentication.

   .. table:: **Table 2** Permission management methods supported by different DLI resource types

      +--------------------+---------------------------------------------------------------------+-----------+-------------+
      | Resource Type      | Authorization Type                                                  | Spark 2.x | Spark 3.3.x |
      +====================+=====================================================================+===========+=============+
      | SQL resource       | IAM fine-grained authorization (role or policy-based authorization) | Supported | Supported   |
      +--------------------+---------------------------------------------------------------------+-----------+-------------+
      | SQL resource       | DLI resource authorization                                          | Supported | Supported   |
      +--------------------+---------------------------------------------------------------------+-----------+-------------+
      | Other DLI resource | IAM fine-grained authorization (role or policy-based authorization) | Supported | Supported   |
      +--------------------+---------------------------------------------------------------------+-----------+-------------+
      | Other DLI resource | DLI resource authorization                                          | Supported | Supported   |
      +--------------------+---------------------------------------------------------------------+-----------+-------------+
