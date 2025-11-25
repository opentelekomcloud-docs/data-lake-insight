:original_name: dli_01_0693.html

.. _dli_01_0693:

IAM Authorization Entity
========================

In IAM, authorization entities are primarily categorized into **users** and **user groups**. Integration with enterprise projects enables resource isolation by group and facilitates refined permission management.


IAM Authorization Entity
------------------------

.. table:: **Table 1** IAM authorization entities

   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Type                              | Description                                                                                                                                                                                                                     |
   +===================================+=================================================================================================================================================================================================================================+
   | User                              | The user you create using a master account in IAM. Each IAM user has their own identity credentials (password and access keys) and can use cloud resources after being granted permissions. IAM users do not own any resources. |
   |                                   |                                                                                                                                                                                                                                 |
   |                                   | -  You can grant specific permission policies to IAM users to define their scope of operations on resources.                                                                                                                    |
   |                                   | -  You can add IAM users to user groups, and they can inherit the permissions of the user groups.                                                                                                                               |
   |                                   | -  You can associate IAM users with enterprise projects for more fine-grained permission management.                                                                                                                            |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | User group                        | A user group is a collection of IAM users. You can create user groups and add IAM users to them to quickly grant permissions to the users.                                                                                      |
   |                                   |                                                                                                                                                                                                                                 |
   |                                   | -  You can grant permission policies to a user group, and all users in the group automatically inherit these permissions.                                                                                                       |
   |                                   | -  You can associate user groups with enterprise projects for resource isolation and permission control.                                                                                                                        |
   |                                   | -  You can create user groups with nested structures to further refine permission management.                                                                                                                                   |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Enterprise Project
------------------

An enterprise project is a resource grouping management function, designed to partition resources into distinct logical units for grouped isolation and access control.

When using IAM authorization, associating IAM authorization entities (users and user groups) with enterprise projects effectively achieves grouped resource isolation and refined permission control.

**Example**: Use enterprise projects to group and isolate DLI resource pools and grant different user groups the permissions to access the corresponding enterprise projects.

An enterprise has two project teams A and B. Project team A and project team B use distinct elastic resource pools and databases. To ensure effective isolation of resources and data, the enterprise plans to use IAM to control access permissions to different resources, ensuring that users in project team A can only access resources corresponding to project team A, and users in project team B can only access resources corresponding to project team B.

#. .. _dli_01_0693__li181955571515:

   **Create enterprise projects and associate elastic resource pools and databases with the enterprise projects.**

   Create enterprise project A and associate the resources used by project team A with enterprise project A.

   Create enterprise project B and associate the resources used by project team B with enterprise project B.

#. **Create user groups.**

   Create user group A and add users in project team A to user group A.

   Create user group B and add users in project team B to user group B.

#. **Grant permissions to user groups.**

   Grant permissions to user group A. On the **Select Scope** page, select **Enterprise projects**, and select enterprise project A created in :ref:`1 <dli_01_0693__li181955571515>`.

   Grant permissions to user group B. On the **Select Scope** page, select **Enterprise projects**, and select enterprise project B created in :ref:`1 <dli_01_0693__li181955571515>`.

In this way, enterprises can group and isolate resources for refined permission control. This ensures both secure and efficient resource utilization.
