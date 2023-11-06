:original_name: dli_01_0451.html

.. _dli_01_0451:

Creating a Custom Policy
========================

Custom policies can be created as a supplement to the system policies of DLI. You can add actions to custom policies. For the actions supported for custom policies, see "Permissions Policies and Supported Actions" in the *Elastic Volume Service API Reference*.

You can create custom policies in either of the following two ways:

-  Visual editor: Select cloud services, actions, resources, and request conditions without the need to know policy syntax.
-  JSON: Create a policy in the JSON format from scratch or based on an existing policy.

. This section describes common DLI custom policies.

Policy Field Description
------------------------

The following example assumes that the authorized user has the permission to create tables in all databases in all regions:

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

   **1.1** indicates a fine-grained permission policy that defines permissions required to perform operations on specific cloud resources under certain conditions.

-  Effect

   The value can be **Allow** and **Deny**. If both **Allow** and **Deny** are found in statements, the **Deny** overrides the **Allow**.

-  Action

   Specific operation on a resource. A maximum of 100 actions are allowed.

   .. note::

      -  The format is *Service name*\ **:**\ *Resource type*\ **:**\ *Action*, for example, **dli:queue:submit_job**.
      -  *Service name*: product name, such as **dli**, **evs**, and **vpc**. Only lowercase letters are allowed. Resource types and operations are not case-sensitive. You can use an asterisk (*) to represent all operations.
      -  *Resource type*: For details, see :ref:`Table 4 <dli_01_0451__table17762145616578>`.
      -  *Action*: action registered in IAM.

-  Condition

   Conditions determine when a policy takes effect. A condition consists of a condition key and operator.

   A condition key is a key in the **Condition** element of a statement. There are global and service-level condition keys.

   -  Global condition keys (prefixed with **g:**) apply to all actions. For details, see condition key description in Policy Syntax.
   -  Service-level condition keys apply only to operations of the specific service.

   An operator is used together with a condition key to form a complete condition statement. For details, see :ref:`Table 1 <dli_01_0451__table198664321589>`.

   IAM provides a set of DLI predefined condition keys. The following table lists the predefined condition keys of DLI.

   .. _dli_01_0451__table198664321589:

   .. table:: **Table 1** DLI request conditions

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
      | g:UserName      | Global          | String          | Current login user                                                                                     |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
      | g:ProjectName   | Global          | String          | Project that you have logged in to                                                                     |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
      | g:DomainName    | Global          | String          | Domain that you have logged in to                                                                      |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+

-  Resource

   The format is *Service name:Region*\ **:**\ *Domain ID*\ **:**\ *Resource type*\ **:**\ *Resource path*. The wildcard (``*``) indicates all options. For details about the resource types and path, see :ref:`Table 4 <dli_01_0451__table17762145616578>`.

   Example:

   **dli:*:*:queue:\*** indicates all queues.


Creating a Custom Policy
------------------------

You can set actions and resources of different levels based on scenarios.

#. Define an action.

   The format is *Service name*\ **:**\ *Resource type*\ **:**\ *Action*. The wildcard is **\***. Example:

   .. table:: **Table 2** Action

      ==================== ========================================
      Action               Description
      ==================== ========================================
      dli:queue:submit_job Submission operations on a DLI queue
      dli:queue:\*         All operations on a DLI queue
      dli:``*``:\*         All operations on all DLI resource types
      ==================== ========================================

   For more information about the relationship between operations and system permissions, see :ref:`Common Operations Supported by DLI System Policy <dli_01_0441>`.

#. Define a resource.

   The format is *Service name*\ **:**\ *Region*\ **:**\ *Domain ID*\ **:**\ :ref:`Resource type:Resource path <dli_01_0451__table17762145616578>`. The wildcard (``*``) indicates all resources. The five fields can be flexibly set. Different levels of permission control can be set for resource paths based on scenario requirements. If you need to set all resources of the service, you do not need to specify this field. For details about the definition of Resource, see :ref:`Table 3 <dli_01_0451__table16314044101614>`. For details about the resource types and paths in Resource, see :ref:`Table 4 <dli_01_0451__table17762145616578>`.

   .. _dli_01_0451__table16314044101614:

   .. table:: **Table 3** Resource

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

   .. _dli_01_0451__table17762145616578:

   .. table:: **Table 4** DLI resources and their paths

      +----------------+---------------------------------------------+------------------------------------------------+
      | Resource Type  | Resource Names                              | Path                                           |
      +================+=============================================+================================================+
      | queue          | DLI queue                                   | queues.queuename                               |
      +----------------+---------------------------------------------+------------------------------------------------+
      | database       | DLI database                                | databases.dbname                               |
      +----------------+---------------------------------------------+------------------------------------------------+
      | table          | DLI table                                   | databases.dbname.tables.tbname                 |
      +----------------+---------------------------------------------+------------------------------------------------+
      | column         | DLI column                                  | databases.dbname.tables.tbname.columns.colname |
      +----------------+---------------------------------------------+------------------------------------------------+
      | jobs           | DLI Flink job                               | jobs.flink.jobid                               |
      +----------------+---------------------------------------------+------------------------------------------------+
      | resource       | DLI package                                 | resources.resourcename                         |
      +----------------+---------------------------------------------+------------------------------------------------+
      | group          | DLI package group                           | groups.groupname                               |
      +----------------+---------------------------------------------+------------------------------------------------+
      | datasourceauth | DLI cross-source authentication information | datasourceauth.name                            |
      +----------------+---------------------------------------------+------------------------------------------------+
      | edsconnections | Enhanced datasource connection              | edsconnections.\ *connection ID*               |
      +----------------+---------------------------------------------+------------------------------------------------+

#. Combine all the preceding fields into a JSON file to form a complete policy. You can set multiple actions and resources. You can also create a policy on the visualized page provided by IAM. For example:

   The authorized user has the permission to create and delete any database, submit jobs for any queue, and delete any table under any account ID in any region of DLI.

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

   -  Allow users to create tables in all databases of all regions:

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

   -  Allow users to query column **col** in the table **tb** of the database **db**:

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

   A deny policy must be used together with other policies. That is, a user can set a deny policy only after being assigned some operation permissions. Otherwise, the deny policy does not take effect.

   If the permissions assigned to a user contain both Allow and Deny actions, the Deny actions take precedence over the Allow actions.

   -  Deny users to create or delete databases, submit jobs (except the default queue), or delete tables.

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

   -  Deny users to submit jobs in the demo queue.

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
