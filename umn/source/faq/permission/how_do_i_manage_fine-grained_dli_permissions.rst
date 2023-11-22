:original_name: dli_03_0100.html

.. _dli_03_0100:

How Do I Manage Fine-Grained DLI Permissions?
=============================================

DLI has a comprehensive permission control mechanism and supports fine-grained authentication through Identity and Access Management (IAM). You can create policies in IAM to manage DLI permissions.

With IAM, you can use your account to create IAM users for your employees, and assign permissions to the users to control their access to specific resource types. For example, some software developers in your enterprise need to use DLI resources but must not delete them or perform any high-risk operations. To achieve this result, you can create IAM users for the software developers and grant them only the permissions required for using DLI resources.

.. note::

   For a new user, you need to log in for the system to record the metadata before using DLI.

IAM can be used free of charge. You pay only for the resources in your account.

If the account has met your requirements, you do not need to create an independent IAM user for permission management. Then you can skip this section. This will not affect other functions of DLI.

DLI System Permissions
----------------------

:ref:`Table 1 <dli_03_0100__en-us_topic_0000001103929830_table6578220217>` lists all the system-defined roles and policies supported by DLI.

You can grant users permissions by using roles and policies.

-  Roles: A type of coarse-grained authorization that defines permissions related to user responsibilities. This mechanism provides only a limited number of service-level roles for authorization. When using roles to grant permissions, you need to also assign other roles on which the permissions depend to take effect. Roles are not an ideal choice for fine-grained authorization and secure access control.
-  Policies: A type of fine-grained authorization that defines permissions required to perform operations on specific cloud resources under certain conditions. This type of authorization is more flexible and ideal for secure access control. For example, you can grant DLI users only the permissions for managing a certain type of cloud servers.

.. _dli_03_0100__en-us_topic_0000001103929830_table6578220217:

.. table:: **Table 1** DLI system permissions

   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Role/Policy Name      | Description                                                                                                                                                                                                                                                                                                                                     | Category              |
   +=======================+=================================================================================================================================================================================================================================================================================================================================================+=======================+
   | DLI FullAccess        | Full permissions for DLI.                                                                                                                                                                                                                                                                                                                       | System-defined policy |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | DLI ReadOnlyAccess    | Read-only permissions for DLI.                                                                                                                                                                                                                                                                                                                  | System-defined policy |
   |                       |                                                                                                                                                                                                                                                                                                                                                 |                       |
   |                       | With read-only permissions, you can use DLI resources and perform operations that do not require fine-grained permissions. For example, create global variables, create packages and package groups, submit jobs to the default queue, create tables in the default database, create datasource connections, and delete datasource connections. |                       |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Tenant Administrator  | Tenant administrator                                                                                                                                                                                                                                                                                                                            | System-defined role   |
   |                       |                                                                                                                                                                                                                                                                                                                                                 |                       |
   |                       | -  Administer permissions for managing and accessing all cloud services. After a database or a queue is created, the user can use the ACL to assign rights to other users.                                                                                                                                                                      |                       |
   |                       | -  Scope: project-level service                                                                                                                                                                                                                                                                                                                 |                       |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | DLI Service Admin     | DLI administrator                                                                                                                                                                                                                                                                                                                               | System-defined role   |
   |                       |                                                                                                                                                                                                                                                                                                                                                 |                       |
   |                       | -  Administer permissions for managing and accessing the queues and data of DLI. After a database or a queue is created, the user can use the ACL to assign rights to other users.                                                                                                                                                              |                       |
   |                       | -  Scope: project-level service                                                                                                                                                                                                                                                                                                                 |                       |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
