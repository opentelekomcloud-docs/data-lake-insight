:original_name: dli_01_0440.html

.. _dli_01_0440:

Overview
========

DLI has a comprehensive permission control mechanism and supports fine-grained authentication through Identity and Access Management (IAM). You can create policies in IAM to manage DLI permissions. You can use both the DLI's permission control mechanism and the IAM service for permission management.

Application Scenarios of IAM Authentication
-------------------------------------------

When using DLI on the cloud, enterprise users need to manage DLI resources (queues) used by employees in different departments, including creating, deleting, using, and isolating resources. In addition, data of different departments needs to be managed, including data isolation and sharing.

DLI uses IAM for refined enterprise-level multi-tenant management. IAM provides identity authentication, permissions management, and access control, helping you securely access to your cloud resources.

With IAM, you can use your account to create IAM users for your employees, and assign permissions to the users to control their access to specific resource types. For example, some software developers in your enterprise need to use DLI resources but must not delete them or perform any high-risk operations. To achieve this result, you can create IAM users for the software developers and grant them only the permissions required for using DLI resources.

.. note::

   For a new user, you need to log in for the system to record the metadata before using DLI.

IAM is free to use, and you only need to pay for the resources in your account.

If your account does not need individual IAM users for permissions management, skip over this section.

.. _dli_01_0440__section6224422143120:

DLI System Permissions
----------------------

:ref:`Table 1 <dli_01_0440__table6578220217>` lists all the system-defined roles and policies supported by DLI.

Type: There are roles and policies.

-  Roles: A type of coarse-grained authorization mechanism that defines permissions related to user responsibilities. Only a limited number of service-level roles are available. When using roles to grant permissions, you also need to assign other roles on which the permissions depend. However, roles are not an ideal choice for fine-grained authorization and secure access control.
-  Policies: A type of fine-grained authorization mechanism that defines permissions required to perform operations on specific cloud resources under certain conditions. This mechanism allows for more flexible policy-based authorization, meeting requirements for secure access control. For example, you can grant DLI users only the permissions for managing a certain type of ECSs.

.. _dli_01_0440__table6578220217:

.. table:: **Table 1** DLI system permissions

   +---------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+------------------------------------------------------------------+
   | Role/Policy Name          | Description                                                                                                                                                                                                                                                                                                                                     | Category              | Dependency                                                       |
   +===========================+=================================================================================================================================================================================================================================================================================================================================================+=======================+==================================================================+
   | DLI FullAccess            | Full permissions for DLI.                                                                                                                                                                                                                                                                                                                       | System-defined policy | This role depends on other roles in the same project.            |
   |                           |                                                                                                                                                                                                                                                                                                                                                 |                       |                                                                  |
   |                           |                                                                                                                                                                                                                                                                                                                                                 |                       | -  Creating a datasource connection: **VPC ReadOnlyAccess**      |
   |                           |                                                                                                                                                                                                                                                                                                                                                 |                       | -  Creating a tag: **TMS FullAccess** and **EPS EPS FullAccess** |
   |                           |                                                                                                                                                                                                                                                                                                                                                 |                       | -  Using OBS for storage: **OBS OperateAccess**                  |
   |                           |                                                                                                                                                                                                                                                                                                                                                 |                       | -  Creating an agency: **Security Administrator**                |
   +---------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+------------------------------------------------------------------+
   | DLI ReadOnlyAccess        | Read-only permissions for DLI.                                                                                                                                                                                                                                                                                                                  | System-defined policy | None                                                             |
   |                           |                                                                                                                                                                                                                                                                                                                                                 |                       |                                                                  |
   |                           | With read-only permissions, you can use DLI resources and perform operations that do not require fine-grained permissions. For example, create global variables, create packages and package groups, submit jobs to the default queue, create tables in the default database, create datasource connections, and delete datasource connections. |                       |                                                                  |
   +---------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+------------------------------------------------------------------+
   | Tenant Administrator      | Tenant administrator                                                                                                                                                                                                                                                                                                                            | System-defined role   | None                                                             |
   |                           |                                                                                                                                                                                                                                                                                                                                                 |                       |                                                                  |
   |                           | -  Job execution permissions for DLI resources. After a database or a queue is created, the user can use the ACL to assign rights to other users.                                                                                                                                                                                               |                       |                                                                  |
   |                           | -  Scope: project-level service                                                                                                                                                                                                                                                                                                                 |                       |                                                                  |
   +---------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+------------------------------------------------------------------+
   | DLI Service Administrator | DLI administrator.                                                                                                                                                                                                                                                                                                                              | System-defined role   | None                                                             |
   |                           |                                                                                                                                                                                                                                                                                                                                                 |                       |                                                                  |
   |                           | -  Job execution permissions for DLI resources. After a database or a queue is created, the user can use the ACL to assign rights to other users.                                                                                                                                                                                               |                       |                                                                  |
   |                           | -  Scope: project-level service                                                                                                                                                                                                                                                                                                                 |                       |                                                                  |
   +---------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+------------------------------------------------------------------+

For details, see :ref:`Creating an IAM User and Granting Permissions <dli_01_0418>`.

DLI Permission Types
--------------------

:ref:`Table 2 <dli_01_0440__table184167440814>` lists the DLI service permissions. For details about the resources that can be controlled by DLI, see :ref:`Table 4 <dli_01_0451__table17762145616578>`.

.. _dli_01_0440__table184167440814:

.. table:: **Table 2** DLI permission types

   +-----------------------------------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Permission Type                   | Subtype                           | Console Operations                                                                                                                                                   | SQL Syntax                                                                                                                                                   |
   +===================================+===================================+======================================================================================================================================================================+==============================================================================================================================================================+
   | Queue Permissions                 | Queue management permissions      | For details, see :ref:`Queue Permission Management <dli_01_0015>`.                                                                                                   | None                                                                                                                                                         |
   +-----------------------------------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Queue usage permission            |                                                                                                                                                                      |                                                                                                                                                              |
   +-----------------------------------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Data Permissions                  | Database permissions              | For details, see :ref:`Configuring Database Permissions on the DLI Console <dli_01_0447>` and :ref:`Configuring Table Permissions on the DLI Console <dli_01_0448>`. | For details, see **SQL Syntax of Batch Jobs** > **Data Permissions Management** > **Data Permissions List** in the *Data Lake Insight SQL Syntax Reference*. |
   +-----------------------------------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Table permissions                 |                                                                                                                                                                      |                                                                                                                                                              |
   +-----------------------------------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Column permissions                |                                                                                                                                                                      |                                                                                                                                                              |
   +-----------------------------------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Job Permissions                   | Flink job permissions             | For details, see :ref:`Configuring Flink Job Permissions <dli_01_0479>`.                                                                                             | None                                                                                                                                                         |
   +-----------------------------------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Package Permissions               | Package group permissions         | For details, see :ref:`Configuring DLI Package Permissions <dli_01_0477>`.                                                                                           | None                                                                                                                                                         |
   +-----------------------------------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Package permissions               |                                                                                                                                                                      |                                                                                                                                                              |
   +-----------------------------------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Datasource Connection Permissions | Datasource connection permissions | For details, see :ref:`Datasource Authentication Permission Management <dli_01_0480>`.                                                                               | None                                                                                                                                                         |
   +-----------------------------------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

An Internet company mainly provides game and music services. DLI is used to analyze user behaviors and assist decision making.

As shown in :ref:`Figure 1 <dli_01_0440__fig3861162219415>`, the **Leader of the Basic Platform Team** has applied for a **Tenant Administrator** account to manage and use cloud services. The **Leader of the Basic Platform Team** creates a subaccount with the **DLI Service Administrator** permission to manage and use DLI, as the **Big Data Platform Team** requires DLI for data analysis. The **Leader of the Basic Platform Team** creates a **Queue A** and assigns it to **Data Engineer A** to analyze the gaming data. A **Queue B** is also assigned to **Data Engineer B** to analyze the music data. Besides granting the queue usage permission, the **Leader of the Basic Platform Team** grants data (except the database) management and usage permissions to the two engineers.

.. _dli_01_0440__fig3861162219415:

.. figure:: /_static/images/en-us_image_0206789784.png
   :alt: **Figure 1** Granting permissions

   **Figure 1** Granting permissions

The **Data Engineer A** creates a table named **gameTable** for storing game prop data and a table named **userTable** for storing game user data. The music service is a new service. To explore potential music users among existing game users, the **Data Engineer A** assigns the query permission on the **userTable** to the **Data Engineer B**. In addition, **Data Engineer B** creates a table named **musicTable** for storing music copyrights information.

:ref:`Table 3 <dli_01_0440__table1190715568239>` describes the queue and data permissions of **Data Engineer A** and **Data Engineer B**.

.. _dli_01_0440__table1190715568239:

.. table:: **Table 3** Permission description

   +--------------+---------------------------------------------------+----------------------------------------------------+
   | User         | Data Engineer A (game data analysis)              | Data Engineer B (music data analysis)              |
   +==============+===================================================+====================================================+
   | Queues       | Queue A (queue usage permission)                  | Queue B (queue usage permission)                   |
   +--------------+---------------------------------------------------+----------------------------------------------------+
   | Data (Table) | gameTable (table management and usage permission) | musicTable (table management and usage permission) |
   +--------------+---------------------------------------------------+----------------------------------------------------+
   |              | userTable (table management and usage permission) | userTable (table query permission)                 |
   +--------------+---------------------------------------------------+----------------------------------------------------+

.. note::

   The queue usage permission includes job submitting and terminating permissions.
