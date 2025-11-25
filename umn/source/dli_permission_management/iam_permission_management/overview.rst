:original_name: dli_01_0417.html

.. _dli_01_0417:

Overview
========

IAM is a foundational service for permission management, offering functions like user identity authentication, permission allocation, and access control, enabling you to securely manage access to cloud resources and perform fine-grained permission management.

IAM Authorization Types and Use Cases
-------------------------------------

IAM can authorize different enterprise users to access cloud service resources. For example, if there are employees responsible for data analysis within your organization whom you wish to have usage permissions for DLI compute resources but not the permission to perform high-risk operations such as deleting DLI resources, you can use IAM for permission allocation. By granting users the ability to only use DLI resources without allowing them to delete these resources, you can control their access to DLI resources.

For newly created users, they must first log in to DLI once to record metadata before being able to use DLI.

.. table:: **Table 1** IAM authorization types

   +---------------------------------+-------------------------------------+--------------------------+-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Type                            | Core Relationship                   | Permission               | Authorization Method                      | Use Case                                                                                                                                                                                                                                                                                         |
   +=================================+=====================================+==========================+===========================================+==================================================================================================================================================================================================================================================================================================+
   | Role/Policy-based authorization | User-permission-authorization scope | -  System-defined role   | Assigning roles or policies to principals | To authorize a user, you need to add it to a user group first and then specify the scope of authorization. It provides a limited number of condition keys and cannot meet the requirements of fine-grained permissions control. This method is suitable for small- and medium-sized enterprises. |
   |                                 |                                     | -  System-defined policy |                                           |                                                                                                                                                                                                                                                                                                  |
   |                                 |                                     | -  Custom policy         |                                           |                                                                                                                                                                                                                                                                                                  |
   +---------------------------------+-------------------------------------+--------------------------+-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

IAM is free to use, and you only need to pay for the resources in your account.

If your account does not need individual IAM users for permission management, skip over this section.

DLI System Permissions
----------------------

:ref:`Table 2 <dli_01_0417__table14662032053>` lists all system-defined permissions for DLI.

.. _dli_01_0417__table14662032053:

.. table:: **Table 2** DLI system permissions

   +----------------------------------------------------------------+
   | Type                                                           |
   +================================================================+
   | System-defined permissions for role/policy-based authorization |
   +----------------------------------------------------------------+

Permission types: Based on the granularity of authorization, they are divided into roles and policies.

-  Roles: A coarse-grained authorization strategy that defines permissions by job responsibility. This strategy offers limited service-level roles for authorization. Cloud services are interdependent. When you assign permissions using roles, you also need to attach any existing role dependencies. Roles are not suitable for fine-grained authorization and least privilege access.
-  Policies: A fine-grained authorization strategy that defines permissions required to perform operations on specific cloud resources under certain conditions. This strategy is more flexible and ideal for least privilege access. For example, you can grant IAM users only permissions to manage DLI resources of a certain type.
