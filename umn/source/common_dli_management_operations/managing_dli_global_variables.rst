:original_name: dli_01_0476.html

.. _dli_01_0476:

Managing DLI Global Variables
=============================

What Is a Global Variable?
--------------------------

DLI allows you to set variables that are frequently used during job development as global variables on the DLI management console. This avoids repeated definitions during job editing and reduces development and maintenance costs. Global variables can be used to replace long and difficult variables, simplifying complex parameters and improving the readability of SQL statements.

This section describes how to create a global variable.

Creating a Global Variable
--------------------------

#. In the navigation pane of the DLI console, choose **Global Configuration** > **Global Variables**.

#. On the **Global Variables** page, click **Create** in the upper right corner to create a global variable.

   .. table:: **Table 1** Global variable parameters

      ========= ====================================
      Parameter Description
      ========= ====================================
      Variable  Name of the created global variable.
      Value     Global variable value.
      ========= ====================================

#. After creating a global variable, use **{{xxxx}}** in the SQL statement to replace the parameter value set as the global variable. **xxxx** indicates the variable name. For example, if you set global variable **abc** to represent the table name, replace the actual table name with **{{abc}}** in the table creation statement.

   .. code-block::

      create table {{table_name}} (String1 String, int4 int, varchar1 varchar(10))
        partitioned by (int1 int,int2 int,int3 int)

   .. note::

      Do not use global variables in **OPTIONS** of the table creation statements.

   **Related operations:**

   -  **Modifying a global variable**

      On the **Global Variables** page, locate a desired variable and click **Modify** in the **Operation** column.

      .. note::

         If there are multiple global variables with the same name in the same project under an account, delete the redundant global variables to ensure that the global variables are unique in the same project. In this case, all users who have the permission to modify the global variables can change the variable values.

   -  **Deleting a global variable**

      On the **Global Variables** page, click **Delete** in the **Operation** column of a variable to delete the variable value.

      .. note::

         -  If there are multiple global variables with the same name in the same project under an account, delete the global variables created by the user first. If there are only unique global variables, all users who have the delete permission can delete the global variables.
         -  After a variable is deleted, the variable cannot be used in SQL statements.

Permissions Management for Global Variables
-------------------------------------------

You can assign different users different global variables through permission settings. The administrator and owners of global variables have all permissions. You do not need to set permissions for them, and their global variable permissions cannot be modified by other users.

When setting global variables for a new user, the user's group must have the **Tenant Guest** permission.

-  **Granting permissions on a global variable to a user**

   #. In the navigation pane on the left of the DLI console, choose **Global Configuration** > **Global Variables**. On the displayed page, locate a desired global variable and click **Set Permission** in the **Operation** column. On the displayed **User Permissions** page, you can grant, set, and revoke permissions on the global variable to users.

   #. Click **Grant Permission** in the upper right corner.

      .. _dli_01_0476__table1382533416195:

      .. table:: **Table 2** Global variable parameters

         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                                     |
         +===================================+=================================================================================================================================================================================+
         | Username                          | Name of the IAM user who is granted permissions                                                                                                                                 |
         |                                   |                                                                                                                                                                                 |
         |                                   | .. note::                                                                                                                                                                       |
         |                                   |                                                                                                                                                                                 |
         |                                   |    This username must be an existing IAM username.                                                                                                                              |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Permissions                       | -  **Update**: This permission allows you to update the global variable.                                                                                                        |
         |                                   | -  **Delete**: This permission allows you to delete the global variable.                                                                                                        |
         |                                   | -  **Grant Permission**: This permission allows you to grant permissions of the global variable to other users.                                                                 |
         |                                   | -  **Revoke Permission**: This permission allows you to revoke the global variable permissions that other users have but cannot revoke the global variable owner's permissions. |
         |                                   | -  **View Other User's Permissions**: This permission allows you to view the global variable permissions of other users.                                                        |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  **Granting permissions on a global variable to a user**

   On the **User Permissions** page, locate a desired IAM user and click **Set Permission** in the **Operation** column. :ref:`Table 2 <dli_01_0476__table1382533416195>` lists the permission parameters.

   If all permission options are grayed out, it means you do not have the authority to modify the permissions on this global variable. You can request the modification permission from users who have authorization, such as the administrator or group owners.

-  **Revoking permissions on a global variable from a user**

   On the **User Permissions** page, locate a desired IAM user and click **Revoke Permission** in the **Operation** column. Once the revoke operation is complete, the IAM user will no longer have any permissions on the global variable.
