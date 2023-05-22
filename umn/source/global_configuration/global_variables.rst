:original_name: dli_01_0476.html

.. _dli_01_0476:

Global Variables
================

Scenario
--------

Global variables can be used to simplify complex parameters. For example, long and difficult variables can be replaced to improve the readability of SQL statements.

Creating Variables
------------------

#. In the navigation pane of the DLI console, choose **Global Configuration** > **Global Variables**.

#. On the **Global Variables** page, click **Create** in the upper right corner to create a global variable.

   .. table:: **Table 1** Parameters description

      +-----------+-----------------------------------------------------------------------------------------------------------------------+
      | Parameter | Description                                                                                                           |
      +===========+=======================================================================================================================+
      | Variable  | Name of the created global variable.                                                                                  |
      +-----------+-----------------------------------------------------------------------------------------------------------------------+
      | Sensitive | If the value is sensitive information, such as passwords, set this parameter to **Yes**. Otherwise, set it to **No**. |
      +-----------+-----------------------------------------------------------------------------------------------------------------------+
      | Value     | Global variable value.                                                                                                |
      +-----------+-----------------------------------------------------------------------------------------------------------------------+

#. After creating a global variable, use **{{xxxx}}** in the SQL statement to replace the parameter value set as the global variable. **xxxx** indicates the variable name. For example, if you set global variable **abc** to represent the table name, replace the actual table name with **{{abc}}** in the table creation statement.

   .. code-block::

      create table {{table_name}} (String1 String, int4 int, varchar1 varchar(10))
        partitioned by (int1 int,int2 int,int3 int)

   .. note::

      -  Only the user who creates a global variable can use the variable.
      -  Do not use global variables in **OPTIONS** of the table creation statements.

Modifying Variables
-------------------

On the **Global Variables** page, click **Modify** in the **Operation** column of a variable to modify the variable value.

.. note::

   Only the user who creates a global variable can modify the variable.

Deleting Variables
------------------

On the **Global Variables** page, click **Delete** in the **Operation** column of a variable to delete the variable value.

.. note::

   -  Only the user who creates a global variable can delete the variable.
   -  After a variable is deleted, the variable cannot be used in SQL statements.
