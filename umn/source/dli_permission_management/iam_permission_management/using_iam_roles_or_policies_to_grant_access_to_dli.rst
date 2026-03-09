:original_name: dli_01_0451.html

.. _dli_01_0451:

Using IAM Roles or Policies to Grant Access to DLI
==================================================

The role/policy-based authorization model provided by `Identity and Access Management (IAM) <https://docs.otc.t-systems.com/usermanual/iam/iam_01_0026.html>`__ lets you control access to DLI resources. With IAM, you can:

-  Based on the organizational structure of your enterprise, create IAM users in your master account for employees from different departments within the company. This ensures that each employee has unique security credentials and can use DLI resources.
-  Grant users the minimum permissions required to perform specific tasks based on their job responsibilities.
-  Entrust an account or cloud service to perform professional, efficient O&M on your DLI resources.

If your account does not need individual IAM users, you may skip over this section.

:ref:`Process Flow <dli_01_0451__en-us_topic_0000001489537442_section1189416161520>` shows the process flow of role/policy-based authorization.

Prerequisites
-------------

Before granting permissions to a user group, familiarize yourself with the DLI permissions that can be added to the user group and select them as needed.

For details about the system permissions of other services, see `Permissions <https://docs.otc.t-systems.com/permissions/index.html>`__.

.. _dli_01_0451__en-us_topic_0000001489537442_section1189416161520:

Process Flow
------------


.. figure:: /_static/images/en-us_image_0000002415789969.png
   :alt: **Figure 1** Process for granting DLI permissions

   **Figure 1** Process for granting DLI permissions

.. table:: **Table 1** Procedure

   +-----------------------+--------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | No.                   | Step                                                         | Description                                                                                                                                                                                                                                                                                                                                                                                       |
   +=======================+==============================================================+===================================================================================================================================================================================================================================================================================================================================================================================================+
   | 1                     | Create a user group and grant permissions to it.             | Create a user group on the IAM console and grant the **DLI ReadOnlyAccess** permission to it.                                                                                                                                                                                                                                                                                                     |
   +-----------------------+--------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 2                     | Create a user and add them to the user group.                | Create a user on the IAM console and add them to the created user group.                                                                                                                                                                                                                                                                                                                          |
   +-----------------------+--------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 3                     | Log in as the IAM user and verify permissions.               | Log in to the console using the newly created user, switch to the authorized region, and verify permissions.                                                                                                                                                                                                                                                                                      |
   |                       |                                                              |                                                                                                                                                                                                                                                                                                                                                                                                   |
   |                       |                                                              | -  Choose **Service List** > **Data Lake Insight**. If you can view the elastic resource pool list on the **Resources** > **Resource Pool** page but cannot purchase an elastic resource pool by clicking **Buy Resource Pool** in the upper right corner (assuming the current permission includes only **DLI ReadOnlyAccess**), the **DLI ReadOnlyAccess** permission has already taken effect. |
   |                       |                                                              | -  Choose any other service in **Service List**. If a message appears indicating that you have insufficient permissions to access the service, the **DLI ReadOnlyAccess** permission has already taken effect.                                                                                                                                                                                    |
   +-----------------------+--------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 4                     | Create a custom policy and associate it with the user group. | If the predefined DLI permissions in the system do not meet your authorization requirements, you can create custom policies.                                                                                                                                                                                                                                                                      |
   |                       |                                                              |                                                                                                                                                                                                                                                                                                                                                                                                   |
   |                       |                                                              | For details about how to create a custom policy, see :ref:`Creating a Custom Policy <dli_01_0451__en-us_topic_0206789937_section15041715821>`.                                                                                                                                                                                                                                                    |
   +-----------------------+--------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Creating a Custom DLI Policy
----------------------------

If the predefined DLI permissions in the system do not meet your authorization requirements, you can create custom policies. For details about the actions that can be added to custom policies, see "Permission Policies and Supported Actions" in the *Data Lake Insight API Reference*.

You can create custom policies in either of the following two ways:

-  Visual editor: Select cloud services, actions, resources, and request conditions. This does not require knowledge of policy syntax.
-  JSON: Create a JSON policy or edit an existing one.

Policy Fields
-------------

In the following example, an IAM user is granted the permission to create tables across all databases in all regions.

.. code-block::

   {
       "Version": "1.1",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "dli:database:createTable"
               ],
               "Resource": [
                   "dli:*:*:database:*"
               ]
           }
       ]
   }

-  Version

   Version: 1.1. Policy: A fine-grained authorization strategy that defines the permissions required to perform actions on a specific cloud resource under certain conditions.

-  Effect

   Function. The value can be **Allow** and **Deny**. If both **Allow** and **Deny** are found in statements, the **Deny** overrides the **Allow**.

-  Action

   Specific action on a resource. A maximum of 100 actions are allowed.

   .. note::

      -  The format is *Service name*\ **:**\ *Resource type*\ **:**\ *Action*, for example, **dli:queue:submit_job**.
      -  *Service name*: product name, such as **dli**, **evs**, or **vpc**. Only lowercase letters are allowed. Resource types and actions are not case-sensitive. You can use an asterisk (*) to represent all actions.
      -  *Resource type*: For details, see :ref:`Table 5 <dli_01_0451__en-us_topic_0206789937_table17762145616578>`.
      -  *Action*: action registered with IAM.

-  Condition

   Determines when a policy is in effect. A condition consists of a condition key and a condition operator.

   A key in the **Condition** element of a statement. There are global and service-specific condition keys.

   -  Global-level condition key: The prefix is **g:**, which applies to all actions. For details, see the condition key description in "Policy Syntax".
   -  Service-level condition key: applies only to actions of the specific service.

   An operator must be used together with a condition key to form a complete condition statement. For details, see :ref:`Table 2 <dli_01_0451__en-us_topic_0206789937_table198664321589>`.

   IAM provides a set of DLI predefined condition keys. The following table lists the DLI predefined condition keys.

   .. _dli_01_0451__en-us_topic_0206789937_table198664321589:

   .. table:: **Table 2** DLI request conditions

      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
      | Condition Key   | Type            | Operator        | Description                                                                                            |
      +=================+=================+=================+========================================================================================================+
      | g:CurrentTime   | Global          | Date and time   | Time when an authentication request is received                                                        |
      |                 |                 |                 |                                                                                                        |
      |                 |                 |                 | .. note::                                                                                              |
      |                 |                 |                 |                                                                                                        |
      |                 |                 |                 |    The time is expressed in the format defined by **ISO 8601**, for example, **2012-11-11T23:59:59Z**. |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
      | g:MFAPresent    | Global          | Boolean         | Whether multi-factor authentication is used during user login                                          |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
      | g:UserId        | Global          | String          | ID of the current login user                                                                           |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
      | g:UserName      | Global          | String          | Current login username                                                                                 |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
      | g:ProjectName   | Global          | String          | Project that you have logged in to                                                                     |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
      | g:DomainName    | Global          | String          | Domain that you have logged in to                                                                      |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+

-  Resource

   The format is *Service name*\ **:**\ *Region*\ **:**\ *Domain ID*\ **:**\ *Resource type*\ **:**\ *Resource path*. The wildcard (``*``) indicates all options. For details about the resource types and path, see :ref:`Table 5 <dli_01_0451__en-us_topic_0206789937_table17762145616578>`.

   Example:

   **dli:*:*:queue:\*** indicates all queues.

.. _dli_01_0451__en-us_topic_0206789937_section15041715821:

Creating a Custom Policy
------------------------

You can set actions and resources at varying levels based on scenarios.

#. Define an action.

   The format is *Service name*\ **:**\ *Resource type*\ **:**\ *Action*. You can use wildcards **\***. Example:

   .. table:: **Table 3** Action

      ==================== ========================================
      Action               Description
      ==================== ========================================
      dli:queue:submit_job Submission operations on a DLI queue
      dli:queue:\*         All operations on a DLI queue
      dli:``*``:\*         All operations on all DLI resource types
      ==================== ========================================

#. Define a resource.

   The format is *Service name*\ **:**\ *Region*\ **:**\ *Domain ID*\ **:**\ *Resource type*\ **:**\ *Resource path*. The wildcard (``*``) indicates all resources. You can flexibly set these five fields. The *Resource path* field can be set with varying levels of access control based on the specific scenario. If you need to set permissions for all resources under this service, you can leave this field unspecified.

   For details about how to define a resource, see :ref:`Table 4 <dli_01_0451__en-us_topic_0206789937_table16314044101614>`.

   For details about the resource types and resource paths, see :ref:`Table 5 <dli_01_0451__en-us_topic_0206789937_table17762145616578>`.

   .. _dli_01_0451__en-us_topic_0206789937_table16314044101614:

   .. table:: **Table 4** Resource

      +--------------------------------------------------+-----------------------------------------------------------------------------+
      | Resource                                         | Description                                                                 |
      +==================================================+=============================================================================+
      | DLI:``*``:``*``:table:databases.dbname.tables.\* | DLI, any region, any account ID, all table resources of database **dbname** |
      +--------------------------------------------------+-----------------------------------------------------------------------------+
      | DLI:``*``:``*``:database:databases.dbname        | DLI, any region, any account ID, resource of database **dbname**            |
      +--------------------------------------------------+-----------------------------------------------------------------------------+
      | DLI:``*``:``*``:queue:queues.\*                  | DLI, any region, any account ID, any queue resource                         |
      +--------------------------------------------------+-----------------------------------------------------------------------------+
      | DLI:``*``:``*``:jobs:jobs.flink.1                | DLI, any region, any account ID, Flink job whose ID is 1                    |
      +--------------------------------------------------+-----------------------------------------------------------------------------+

   .. _dli_01_0451__en-us_topic_0206789937_table17762145616578:

   .. table:: **Table 5** DLI resources and their paths

      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Type                | Resource                                  | Path                                                                              | Description                                                                                                                                                                                                                             |
      +=====================+===========================================+===================================================================================+=========================================================================================================================================================================================================================================+
      | elasticresourcepool | DLI elastic resource pool                 | DLI:``*``:``*``:elasticresourcepool:elasticresourcepools\ *.name*                 | -  The path prefix for **elasticresourcepool** is fixed as **DLI:*:*:elasticresourcepool:**.                                                                                                                                            |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:elasticresourcepool:elasticresourcepools.\*** indicates any DLI elastic resource pool.                                                                                                              |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   |    **DLI:*:*:elasticresourcepool:elasticresourcepools.pool01** indicates a specific DLI elastic resource pool named **pool01**.                                                                                                         |
      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | queue               | DLI queue                                 | DLI:``*``:``*``:queue:queues\ *.queuename*                                        | -  The path prefix for **queue** is fixed as **DLI:*:*:queue:**.                                                                                                                                                                        |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:queue:queues.\*** indicates any DLI queue.                                                                                                                                                          |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   |    **DLI:*:*:queue:queues.queue01** indicates a specific DLI queue named **queue01**.                                                                                                                                                   |
      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | database            | DLI database                              | DLI:``*``:``*``:database:databases\ *.dbname*                                     | -  The path prefix for **database** is fixed as **DLI:*:*:database:**.                                                                                                                                                                  |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:database:databases.\*** indicates any DLI database.                                                                                                                                                 |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   |    **DLI:*:*:database:databases.db01** indicates a specific DLI database named **db01**.                                                                                                                                                |
      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | table               | DLI table                                 | DLI:``*``:``*``:table:databases.\ *dbname*.tables\ *.tbname*                      | -  The path prefix for **table** is fixed as **DLI:*:*:table:**.                                                                                                                                                                        |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:able:databases..tables.\*** indicates any DLI table.                                                                                                                                                |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   |    **DLI:*:*:table:databases.db01.tables.tb01** indicates a DLI table named **tb01** in the **db01** database.                                                                                                                          |
      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | column              | DLI column                                | DLI:``*``:``*``:column:databases.\ *dbname*.tables.\ *tbname*.columns\ *.colname* | -  The path prefix for **column** is fixed as **DLI:*:*:column:**.                                                                                                                                                                      |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:column:databases..tables..columns.\*** indicates any DLI column.                                                                                                                                    |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   |    **DLI:*:*:column:databases.db01.tables.tb01.columns.col01** indicates a DLI column named **col01** in the **tb01** table of the **db01** database.                                                                                   |
      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | jobs                | DLI Flink job                             | DLI:``*``:``*``:jobs:jobs.flink\ *.jobid*                                         | -  The path prefix for **jobs** is fixed as **DLI:*:*:jobs:**.                                                                                                                                                                          |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:jobs:jobs.flink.\*** indicates any DLI Flink job.                                                                                                                                                   |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   |    **DLI:*:*:jobs:jobs.flink.123456** indicates a DLI Flink job with ID of **123456**.                                                                                                                                                  |
      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | resource            | DLI package                               | DLI:``*``:``*``:resource:resources.\ *resourcename*                               | -  The path prefix for **resource** is fixed as **DLI:*:*:resource:**.                                                                                                                                                                  |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:resource:resources.\*** indicates any DLI package.                                                                                                                                                  |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   |    **DLI:*:*:resource:resources.jar01** indicates a DLI package named **jar01**.                                                                                                                                                        |
      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | group               | DLI package group                         | DLI:``*``:``*``:group:groups.\ *groupname*                                        | -  The path prefix for **group** is fixed as **DLI:*:*:group:**.                                                                                                                                                                        |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:group:groups.\*** indicates any DLI package group.                                                                                                                                                  |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   |    **DLI:*:*:group:groups.group01** indicates a DLI package group named **group01**.                                                                                                                                                    |
      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | datasourceauth      | DLI datasource authentication information | DLI:``*``:``*``:datasourceauth:datasourceauth.\ *name*                            | -  The path prefix for **datasourceauth** is fixed as **DLI:*:*:datasourceauth:**.                                                                                                                                                      |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:datasourceauth:datasourceauth.\*** indicates any DLI datasource authentication information.                                                                                                         |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   |    **DLI:*:*:datasourceauth:datasourceauth.auth01** indicates DLI datasource authentication information named **auth01**.                                                                                                               |
      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | edsconnections      | Enhanced datasource connection            | DLI:``*``:``*``:edsconnections:edsconnections.\ *Connection ID*                   | -  The path prefix for **edsconnections** is fixed as **DLI:*:*:edsconnections:**.                                                                                                                                                      |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:edsconnections:edsconnections.\*** indicates any DLI enhanced datasource connection.                                                                                                                |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   |    **DLI:*:*:edsconnections:edsconnections.conn01** indicates a DLI enhanced datasource connection with connection ID of **conn01**.                                                                                                    |
      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | variable            | DLI global variable                       | DLI:``*``:``*``:variable:variables.\ *name*                                       | -  The path prefix for **variable** is fixed as **DLI:*:*:variable:**.                                                                                                                                                                  |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:variable:variables.\*** indicates any DLI global variable.                                                                                                                                          |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   |    **DLI:*:*:variable:variables.var01** indicates a DLI global variable named **var01**.                                                                                                                                                |
      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | sqldefendrule       | SQL inspection rule                       | DLI:``*``:``*``:sqldefendrule:sqldefendes.\ ``*``                                 | -  The path prefix for **sqldefendrule** is fixed as **DLI:*:*:sqldefendrule:**.                                                                                                                                                        |
      |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:sqldefendrule:sqldefendes.\*** indicates all SQL inspection rules. (The **sqldefendrule** resource path inherently includes wildcard characters, so there is no need to specify them additionally.) |
      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | catalog             | DLI data catalog                          | DLI:``*``:``*``:catalog:catalogs.\ *name*                                         | -  The path prefix for **catalog** is fixed as **DLI:*:*:catalog:**.                                                                                                                                                                    |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:catalog:catalogs.\*** indicates any DLI data catalog.                                                                                                                                               |
      |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
      |                     |                                           |                                                                                   |    **DLI:*:*:catalog:catalogs.cat01** indicates a DLI data catalog named **cat01**.                                                                                                                                                     |
      +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Combine all of the preceding fields into a JSON string to create a complete policy. You can set multiple actions and resources, and you also have the option to create policies through the IAM console. For example:

   Create a policy that grants users the permission to create and delete databases, submit jobs for any queue, and delete tables under any account ID in any region of DLI.

   .. code-block::

      {
          "Version": "1.1",
          "Statement": [
              {
                  "Effect": " Allow",
                  "Action": [
                        "dli:database:createDatabase",
                        "dli:database:dropDatabase",
                        "dli:queue:submitJob",
                        "dli:table:dropTable"
                  ],
                  "Resource": [
                        "dli:*:*:database:*",
                        "dli:*:*:queue:*",
                        "dli:*:*:table:*"
                  ]
              }
          ]
      }

Example Custom Policies
-----------------------

-  Example 1: Allow policies

   -  Allow users to create tables across all databases in all regions.

      .. code-block::

         {
             "Version": "1.1",
             "Statement": [
                 {
                     "Effect": "Allow",
                     "Action": [
                         "dli:database:createTable"
                     ],
                     "Resource": [
                         "dli:*:*:database:*"
                     ]
                 }
             ]
         }

   -  Allow users to query column **col** in the table **tb** of the database **db**.

      .. code-block::

         {
             "Version": "1.1",
             "Statement": [
                 {
                     "Effect": "Allow",
                     "Action": [
                         "dli:column:select"
                     ],
                     "Resource": [
                         "dli:*:*:column:databases.db.tables.tb.columns.col"
                     ]
                 }
             ]
         }

-  Example 2: Deny policies

   A deny policy must be used together with other policies. Users need to be granted some operation permission policies first before a deny policy can be set within those permissions. Otherwise, if the user has no permissions at all, the deny policy has no practical effect.

   In the policies granted to a user, if an action has both Allow and Deny, the Deny takes precedence.

   -  Deny users to create or delete databases, submit jobs (except the **default** queue), or delete tables.

      .. code-block::

         {
             "Version": "1.1",
             "Statement": [
                 {
                     "Effect": "Deny",
                     "Action": [
                         "dli:database:createDatabase",
                         "dli:database:dropDatabase",
                         "dli:queue:submitJob",
                         "dli:table:dropTable"
                     ],
                     "Resource": [
                         "dli:*:*:database:*",
                         "dli:*:*:queue:*",
                         "dli:*:*:table:*"
                     ]
                 }
             ]
         }

   -  Deny users to submit jobs on the **demo** queue.

      .. code-block::

         {
             "Version": "1.1",
             "Statement": [
                 {
                     "Effect": "Deny",
                     "Action": [
                         "dli:queue:submitJob"
                     ],
                     "Resource": [
                         "dli:*:*:queue:queues.demo"
                     ]
                 }
             ]
         }

DLI Resources
-------------

Resources are objects that exist within a service. In DLI, resources include the following, and you can select specific resources when creating custom policies by specifying the resource path.

.. table:: **Table 6** DLI resources and their paths

   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Type                | Resource                                  | Path                                                                              | Description                                                                                                                                                                                                                             |
   +=====================+===========================================+===================================================================================+=========================================================================================================================================================================================================================================+
   | elasticresourcepool | DLI elastic resource pool                 | DLI:``*``:``*``:elasticresourcepool:elasticresourcepools\ *.name*                 | -  The path prefix for **elasticresourcepool** is fixed as **DLI:*:*:elasticresourcepool:**.                                                                                                                                            |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:elasticresourcepool:elasticresourcepools.\*** indicates any DLI elastic resource pool.                                                                                                              |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   |    **DLI:*:*:elasticresourcepool:elasticresourcepools.pool01** indicates a specific DLI elastic resource pool named **pool01**.                                                                                                         |
   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | queue               | DLI queue                                 | DLI:``*``:``*``:queue:queues\ *.queuename*                                        | -  The path prefix for **queue** is fixed as **DLI:*:*:queue:**.                                                                                                                                                                        |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:queue:queues.\*** indicates any DLI queue.                                                                                                                                                          |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   |    **DLI:*:*:queue:queues.queue01** indicates a specific DLI queue named **queue01**.                                                                                                                                                   |
   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | database            | DLI database                              | DLI:``*``:``*``:database:databases\ *.dbname*                                     | -  The path prefix for **database** is fixed as **DLI:*:*:database:**.                                                                                                                                                                  |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:database:databases.\*** indicates any DLI database.                                                                                                                                                 |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   |    **DLI:*:*:database:databases.db01** indicates a specific DLI database named **db01**.                                                                                                                                                |
   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table               | DLI table                                 | DLI:``*``:``*``:table:databases.\ *dbname*.tables\ *.tbname*                      | -  The path prefix for **table** is fixed as **DLI:*:*:table:**.                                                                                                                                                                        |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:able:databases..tables.\*** indicates any DLI table.                                                                                                                                                |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   |    **DLI:*:*:table:databases.db01.tables.tb01** indicates a DLI table named **tb01** in the **db01** database.                                                                                                                          |
   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | column              | DLI column                                | DLI:``*``:``*``:column:databases.\ *dbname*.tables.\ *tbname*.columns\ *.colname* | -  The path prefix for **column** is fixed as **DLI:*:*:column:**.                                                                                                                                                                      |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:column:databases..tables..columns.\*** indicates any DLI column.                                                                                                                                    |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   |    **DLI:*:*:column:databases.db01.tables.tb01.columns.col01** indicates a DLI column named **col01** in the **tb01** table of the **db01** database.                                                                                   |
   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | jobs                | DLI Flink job                             | DLI:``*``:``*``:jobs:jobs.flink\ *.jobid*                                         | -  The path prefix for **jobs** is fixed as **DLI:*:*:jobs:**.                                                                                                                                                                          |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:jobs:jobs.flink.\*** indicates any DLI Flink job.                                                                                                                                                   |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   |    **DLI:*:*:jobs:jobs.flink.123456** indicates a DLI Flink job with ID of **123456**.                                                                                                                                                  |
   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource            | DLI package                               | DLI:``*``:``*``:resource:resources.\ *resourcename*                               | -  The path prefix for **resource** is fixed as **DLI:*:*:resource:**.                                                                                                                                                                  |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:resource:resources.\*** indicates any DLI package.                                                                                                                                                  |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   |    **DLI:*:*:resource:resources.jar01** indicates a DLI package named **jar01**.                                                                                                                                                        |
   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | group               | DLI package group                         | DLI:``*``:``*``:group:groups.\ *groupname*                                        | -  The path prefix for **group** is fixed as **DLI:*:*:group:**.                                                                                                                                                                        |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:group:groups.\*** indicates any DLI package group.                                                                                                                                                  |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   |    **DLI:*:*:group:groups.group01** indicates a DLI package group named **group01**.                                                                                                                                                    |
   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | datasourceauth      | DLI datasource authentication information | DLI:``*``:``*``:datasourceauth:datasourceauth.\ *name*                            | -  The path prefix for **datasourceauth** is fixed as **DLI:*:*:datasourceauth:**.                                                                                                                                                      |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:datasourceauth:datasourceauth.\*** indicates any DLI datasource authentication information.                                                                                                         |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   |    **DLI:*:*:datasourceauth:datasourceauth.auth01** indicates DLI datasource authentication information named **auth01**.                                                                                                               |
   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | edsconnections      | Enhanced datasource connection            | DLI:``*``:``*``:edsconnections:edsconnections.\ *Connection ID*                   | -  The path prefix for **edsconnections** is fixed as **DLI:*:*:edsconnections:**.                                                                                                                                                      |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:edsconnections:edsconnections.\*** indicates any DLI enhanced datasource connection.                                                                                                                |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   |    **DLI:*:*:edsconnections:edsconnections.conn01** indicates a DLI enhanced datasource connection with connection ID of **conn01**.                                                                                                    |
   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | variable            | DLI global variable                       | DLI:``*``:``*``:variable:variables.\ *name*                                       | -  The path prefix for **variable** is fixed as **DLI:*:*:variable:**.                                                                                                                                                                  |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:variable:variables.\*** indicates any DLI global variable.                                                                                                                                          |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   |    **DLI:*:*:variable:variables.var01** indicates a DLI global variable named **var01**.                                                                                                                                                |
   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sqldefendrule       | SQL inspection rule                       | DLI:``*``:``*``:sqldefendrule:sqldefendes.\ ``*``                                 | -  The path prefix for **sqldefendrule** is fixed as **DLI:*:*:sqldefendrule:**.                                                                                                                                                        |
   |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:sqldefendrule:sqldefendes.\*** indicates all SQL inspection rules. (The **sqldefendrule** resource path inherently includes wildcard characters, so there is no need to specify them additionally.) |
   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | catalog             | DLI data catalog                          | DLI:``*``:``*``:catalog:catalogs.\ *name*                                         | -  The path prefix for **catalog** is fixed as **DLI:*:*:catalog:**.                                                                                                                                                                    |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   | -  Supports wildcard (*): **DLI:*:*:catalog:catalogs.\*** indicates any DLI data catalog.                                                                                                                                               |
   |                     |                                           |                                                                                   |                                                                                                                                                                                                                                         |
   |                     |                                           |                                                                                   |    **DLI:*:*:catalog:catalogs.cat01** indicates a DLI data catalog named **cat01**.                                                                                                                                                     |
   +---------------------+-------------------------------------------+-----------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

DLI Request Condition
---------------------

Request conditions are useful in determining when a custom policy is in effect. A request condition consists of condition keys and operators. Condition keys are either global or service-level and are used in the Condition element of a policy statement. Global condition keys (starting with **g:**) are available for operations of all services, while service-level condition keys (starting with a service name such as **dli**) are available only for operations of a specific service. An operator must be used together with a condition key to form a complete condition statement.

IAM provides a set of DLI predefined condition keys. The following table lists the DLI predefined condition keys.

.. table:: **Table 7** DLI request conditions

   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
   | Condition Key   | Type            | Operator        | Description                                                                                            |
   +=================+=================+=================+========================================================================================================+
   | g:CurrentTime   | Global          | Date and time   | Time when an authentication request is received                                                        |
   |                 |                 |                 |                                                                                                        |
   |                 |                 |                 | .. note::                                                                                              |
   |                 |                 |                 |                                                                                                        |
   |                 |                 |                 |    The time is expressed in the format defined by **ISO 8601**, for example, **2012-11-11T23:59:59Z**. |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
   | g:MFAPresent    | Global          | Boolean         | Whether multi-factor authentication is used during user login                                          |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
   | g:UserId        | Global          | String          | ID of the current login user                                                                           |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
   | g:UserName      | Global          | String          | Current login username                                                                                 |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
   | g:ProjectName   | Global          | String          | Project that you have logged in to                                                                     |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
   | g:DomainName    | Global          | String          | Domain that you have logged in to                                                                      |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
