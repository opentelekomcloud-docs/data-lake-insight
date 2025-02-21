:original_name: dli_01_0021.html

.. _dli_01_0021:

Creating a SQL Job Template
===========================

To facilitate SQL operation execution, DLI allows you to customize query templates or save the SQL statements in use as templates. After templates are saved, you do not need to compile SQL statements. Instead, you can directly perform the SQL operations using the templates.

SQL templates include sample templates and custom templates. The default sample template contains 22 standard TPC-H query statements, which can meet most TPC-H test requirements. For details, see :ref:`TPC-H Sample Data in the SQL Templates Preset on DLI <dli_01_05111>`.

.. note::

   In the navigation pane on the left, choose **Job Templates** > **SQL Templates**. In the upper right corner of the displayed page, click **Settings**. In the displayed **Settings** dialog box, choose whether to display templates by group.

   If you enable **Display by Group**, the display options are **Expand the first group**, **Expand all**, and **Collapse all**.


Creating a SQL Job Template
---------------------------

You can create a template on either the **Job Templates** or the **SQL Editor** page.

-  To create a template on the **Job Templates** page:

   #. On the left of the management console, choose **Job Templates** > **SQL Templates**.

   #. On the **SQL Templates** page, click **Create Template** to create a template.

      Enter the template name, SQL statement, and description information. For details, see :ref:`Table 1 <dli_01_0021__table8760202135313>`.

      .. _dli_01_0021__table8760202135313:

      .. table:: **Table 1** Parameter description

         +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                        |
         +===================================+====================================================================================================================================================================+
         | Name                              | Indicates the template name.                                                                                                                                       |
         |                                   |                                                                                                                                                                    |
         |                                   | -  A template name can contain only digits, letters, and underscores (_), but cannot start with an underscore (_) or contain only digits. It cannot be left empty. |
         |                                   | -  The template name can contain a maximum of 50 characters.                                                                                                       |
         +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Statement                         | SQL statement to be saved as a template.                                                                                                                           |
         +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Description                       | Description of the template you create.                                                                                                                            |
         +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Group                             | -  Use existing                                                                                                                                                    |
         |                                   | -  Use new                                                                                                                                                         |
         |                                   | -  Do not use                                                                                                                                                      |
         +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Group Name                        | If you select **Use existing** or **Use new**, you need to enter the group name.                                                                                   |
         +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   #. Click **OK**.

-  To create a template on the **SQL Editor** page:

   #. On the left of the management console, click **SQL Editor**.

   #. In the SQL job editing area of the displayed **SQL Editor** page, click **More** in the upper right corner and choose **Save as Template**.

      Enter the template name, SQL statement, and description information. For details, see :ref:`Table 1 <dli_01_0021__table8760202135313>`.

   #. Click **OK**.

Submitting a SQL Job Using a Template
-------------------------------------

Perform the template operation as follows:

#. On the left of the management console, choose **Job Templates** > **SQL Templates**.
#. On the **SQL Templates** page, select a template and click **Execute** in the **Operation** column. The **SQL Editor** page is displayed, and the corresponding SQL statement is automatically entered in the SQL job editing window.
#. In the upper right corner of the SQL job editing window, Click **Execute** to run the SQL statement. After the execution is complete, you can view the execution result below the current SQL job editing window.

Searching for a SQL Job Template
--------------------------------

On the **SQL Templates** page, you can enter the template name keyword in the search box on the upper right corner to search for the desired template.

Modifying a SQL Job Template
----------------------------

Only custom templates can be modified. To modify a template, perform the following steps:

#. On the **SQL Templates** page, locate the target template and click **Modify** in the **Operation** column.
#. In the displayed **Modify Template** dialog box, modify the template name, statement, and description as required.
#. Click **OK**.

Deleting a Template
-------------------

On the **SQL Templates** page, select one or more templates to be deleted and click **Delete** to delete the selected templates.
