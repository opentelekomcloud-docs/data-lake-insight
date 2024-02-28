:original_name: dli_01_0476.html

.. _dli_01_0476:

Global Variables
================

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

Modifying a Global Variable
---------------------------

On the **Global Variables** page, click **Modify** in the **Operation** column of a variable to modify the variable value.

.. note::

   If there are multiple global variables with the same name in the same project under an account, delete the redundant global variables to ensure that the global variables are unique in the same project. In this case, all users who have the permission to modify the global variables can change the variable values.

Deleting a Global Variable
--------------------------

On the **Global Variables** page, click **Delete** in the **Operation** column of a variable to delete the variable value.

.. note::

   -  If there are multiple global variables with the same name in the same project under an account, delete the global variables created by the user first. If there are only unique global variables, all users who have the delete permission can delete the global variables.
   -  After a variable is deleted, the variable cannot be used in SQL statements.
