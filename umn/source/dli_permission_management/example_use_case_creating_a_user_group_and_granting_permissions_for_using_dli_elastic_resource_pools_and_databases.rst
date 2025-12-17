:original_name: dli_01_0695.html

.. _dli_01_0695:

Example Use Case: Creating a User Group and Granting Permissions for Using DLI Elastic Resource Pools and Databases
===================================================================================================================

Description
-----------

An e-commerce company conducts data analysis operations in region A. The marketing department's employees, user 1 and user 2, along with the technical department's employees, user 3 and user 4, need to use DLI elastic resource pools and databases to accomplish various tasks.

-  Add the marketing department employees to user group G1 and the technical department employees to user group G2.


   .. figure:: /_static/images/en-us_image_0000002381617561.png
      :alt: **Figure 1** Adding users to user groups

      **Figure 1** Adding users to user groups

-  Associate compute resource E1 and database DB1 with enterprise project A to store sales data and user behavior data.

-  Associate compute resource E2 and database DB2 with enterprise project B to store system logs and server performance data.


   .. figure:: /_static/images/en-us_image_0000002381737281.png
      :alt: **Figure 2** Associating resources with enterprise projects

      **Figure 2** Associating resources with enterprise projects

User 1 in the marketing department needs to perform in-depth analysis on sales data. To delete invalid data in a timely manner, user 1 must have the permission to delete resources in enterprise project A.

User 2 in the marketing department is responsible for evaluating the effectiveness of marketing activities and needs to have the permission to delete related data resources to ensure data timeliness.

User 3 in the technical department focuses on system log analysis and only needs to read and use resources in enterprise project B.

User 4 in the technical department is responsible for monitoring server performance and performing read-only operations on related resources.

Authorization Scheme Analysis
-----------------------------

Based on the preceding service requirements, combined with the DLI service permissions, IAM permission management, and enterprise project management methods, there are two authorization schemes to choose from:

Scheme 1: Grant access permissions to corresponding enterprise project resources to user groups G1 and G2 through IAM, and separately grant resource deletion permissions to user 1 and user 2.

Scheme 2: Through the permission management method provided by DLI, individually set different resource usage permissions according to the specific job requirements of each user.

This section details the operational steps for both scheme 1 and scheme 2.

When submitting jobs using elastic resource pools, you need to have the permission to use queues. In this example, we take the submission and cancellation of jobs for queues in the elastic resource pool as an example.

Scheme 1: Use IAM to Grant Enterprise User Groups A and B Different Resource Permissions
----------------------------------------------------------------------------------------

When submitting jobs using elastic resource pools, you need to have the permission to use queues. In this example, we take the submission and cancellation of jobs for queues in the elastic resource pool as an example.

**Prerequisites: You have added users to user groups and associated resources with enterprise projects.**

#. **Create a custom IAM policy Policy_A to grant the permission to access resources E1 and DB1 under enterprise project A.**

   Example policy content:

   .. code-block::

      {
          "Version": "1.1",
          "Statement": [
              {
                  "Effect": "Allow",
                  "Action": [
                      "dli:elasticresourcepool:resourceManagement",
                      "dli:queue:submitJob",
                      "dli:queue:cancelJob",
                      "dli:database:list"
                  ],
                  "Resource": [
                      "dli:*:*:elasticresourcepool:elasticresourcepools.E1",
                      "dli:*:*:database:databases.DB1"
                  ]
              }
          ]
      }

#. **Create a custom IAM policy Policy_B to grant the permission to access resources E2 and DB2 under enterprise project B.**

   Example policy content:

   .. code-block::

      {
          "Version": "1.1",
          "Statement": [
              {
                  "Effect": "Allow",
                  "Action": [
                      "dli:elasticresourcepool:resourceManagement",
                      "dli:queue:submitJob",
                      "dli:queue:cancelJob",
                      "dli:database:list"
                  ],
                  "Resource": [
                      "dli:*:*:elasticresourcepool:elasticresourcepools.E2",
                      "dli:*:*:database:databases.DB2"
                  ]
              }
          ]
      }

#. **Create Policy_C to grant the resource deletion permission to user 1 and user 2.**

   Example policy content:

   .. code-block::

      {
          "Version": "1.1",
          "Statement": [
              {
                  "Effect": "Allow",
                  "Action": [
                      "dli:elasticresourcepool:drop",
                      "dli:database:dropDatabase"
                  ],
                  "Resource": [
                      "dli:*:*:elasticresourcepool:elasticresourcepool.E1",
                      "dli:*:*:database:databases.DB1"
                  ]
              }
          ]
      }

#. **Grant permissions to user groups.**

   In the user group list, click **Authorize** in the **Operation** column of the user groups.

   -  Selecting a policy:

      Apply **Policy_A** to user group G1.

      Apply **Policy_B** to user group G2.

      Apply the deletion permission policy **Policy_C** to user 1 and user 2, respectively.

   -  Selecting the minimum authorization scope

      When authorizing user group G1, associate the authorization scope with enterprise project A.

      When authorizing user group G2, associate the authorization scope with enterprise project B.

      When granting the deletion permission to user 1 and user 2, associate the authorization scope with enterprise project A.

Solution 2: Authorizing Different Permissions for Users User 1, User 2, User 3, and User 4 Using DLI Resources
--------------------------------------------------------------------------------------------------------------

**Prerequisites: You have added users to user groups and associated resources with enterprise projects.**

The permissions here are only examples. Select the permissions as required.

Log in to the DLI management console, find the corresponding resources, and use the user authorization function of DLI to grant appropriate permissions to each user.

-  **Elastic resource pool E1**

   -  User 1: Submit jobs, terminate jobs, and delete queues.
   -  User 2: Submit jobs, terminate jobs, and delete queues.

-  **Elastic resource pool E2**

   -  User 3: Submit jobs and terminate jobs.
   -  User 4: Submit jobs and terminate jobs.

-  **Database DB1**

   -  User 1: Display all tables, show databases, delete databases, query tables, insert data, rewrite data, fill in columns, add partitions, delete partitions, update table data, update data, and delete data.
   -  User 2: Display all tables, show databases, delete databases, query tables, insert data, rewrite data, fill in columns, add partitions, delete partitions, update table data, update data, and delete data.

-  **Database DB2**

   -  User 3: Display all tables, show databases, query tables, insert data, rewrite data, fill in columns, add partitions, delete partitions, update table data, update data, and delete data.
   -  User 4: Display all tables, show databases, query tables, insert data, rewrite data, fill in columns, add partitions, delete partitions, update table data, update data, and delete data.

Scheme Comparison
-----------------

-  Scheme 1: The IAM user group authorization method is more suitable for scenarios that require batch management of user permissions. By combining user groups and policies, it efficiently allocates permissions while facilitating subsequent management.
-  Scheme 2: The DLI user-level authorization method is better suited for scenarios where fine-grained permission management for each user is needed. It can meet complex service requirements but comes with relatively higher management costs.

.. table:: **Table 1** Scheme comparison

   +-------------------------------------+---------------------------------------------------------------------+-----------------------------------------------------------------------------+
   | Item                                | Scheme 1: IAM User Group Authorization                              | Scheme 2: DLI User-Level Authorization                                      |
   +=====================================+=====================================================================+=============================================================================+
   | User authorization complexity       | Based on user groups, suitable for batch management.                | Based on individual users, suitable for fine-grained permission control.    |
   +-------------------------------------+---------------------------------------------------------------------+-----------------------------------------------------------------------------+
   | Permission configuration complexity | Centralized policy management, lower complexity.                    | Independent configuration per user, higher complexity.                      |
   +-------------------------------------+---------------------------------------------------------------------+-----------------------------------------------------------------------------+
   | Resource isolation                  | Isolated by enterprise projects, ideal for multi-team environments. | Flexible cross-project authorization, suitable for collaborative scenarios. |
   +-------------------------------------+---------------------------------------------------------------------+-----------------------------------------------------------------------------+
   | Delete permission management        | Separate delete permission configuration at the user level.         | Delete permission directly selected in resource authorization.              |
   +-------------------------------------+---------------------------------------------------------------------+-----------------------------------------------------------------------------+
   | Use case                            | Large organizations managing resources across teams                 | Small teams or scenarios requiring differentiated permissions               |
   +-------------------------------------+---------------------------------------------------------------------+-----------------------------------------------------------------------------+
