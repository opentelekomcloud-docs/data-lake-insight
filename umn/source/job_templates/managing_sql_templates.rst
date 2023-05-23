:original_name: dli_01_0021.html

.. _dli_01_0021:

Managing SQL Templates
======================

To facilitate SQL operation execution, DLI allows you to customize query templates or save the SQL statements in use as templates. After templates are saved, you do not need to compile SQL statements. Instead, you can directly perform the SQL operations using the templates.

SQL templates include sample templates and custom templates. The default sample template contains 22 standard TPC-H query statements, which can meet most TPC-H test requirements. For details, see :ref:`TPC-H Sample Data in the SQL Template <dli_01_05111>`.

SQL template management provides the following functions:

-  :ref:`Sample Templates <dli_01_0021__section1039614583412>`
-  :ref:`Custom Templates <dli_01_0021__section1616314111518>`
-  :ref:`Creating a Template <dli_01_0021__section73391334165211>`
-  :ref:`Executing the Template <dli_01_0021__section1936164995213>`
-  :ref:`Searching for a Template <dli_01_0021__section1045610354536>`
-  :ref:`Modifying a Template <dli_01_0021__section08698165316>`
-  :ref:`Deleting a Template <dli_01_0021__section1317681345320>`

Table Settings
--------------

In the upper right corner of the **SQL Template** page, click **Set Property** to determine whether to display templates by group.

If you select **Display by Group**, the following display modes are available:

-  Expand the first group
-  Expand all
-  Collapse All

.. _dli_01_0021__section1039614583412:

Sample Templates
----------------

The current sample template contains 22 standard TPC-H query statements. You can view the template name, description, and statements. For details about TPC-H examples, see :ref:`TPC-H Sample Data in the SQL Template <dli_01_05111>`.

.. table:: **Table 1** Template management parameters

   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                       |
   +===================================+===================================================================================================================================================================================================================================================+
   | Name                              | Indicates the template name.                                                                                                                                                                                                                      |
   |                                   |                                                                                                                                                                                                                                                   |
   |                                   | -  A template name can contain only digits, letters, and underscores (_), but cannot start with an underscore (_) or contain only digits. It cannot be left empty.                                                                                |
   |                                   | -  The template name can contain a maximum of 50 characters.                                                                                                                                                                                      |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Description                       | Description of the template you create.                                                                                                                                                                                                           |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Statement                         | SQL statement created as the template.                                                                                                                                                                                                            |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operation                         | **Execute**: After you click this button, the system switches to the **SQL Editor** page, where you can modify or directly perform the statement as required. For details, see :ref:`Executing the Template <dli_01_0021__section1936164995213>`. |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

The existing sample templates apply to the following scenarios:

-  Price summary report query
-  Minimum cost supplier analysis
-  Shipping priority analysis
-  Analysis of order priority check
-  Analysis of the number of local suppliers
-  Analysis of forecasted income changes
-  Freight volume analysis
-  National market share analysis
-  Profit estimation analysis by product type
-  Analysis of returned parts
-  Analysis of key inventory indicators
-  Freight mode and command priority analysis
-  Consumer allocation analysis
-  Promotion effect analysis
-  Analysis of the supplier with the largest contribution
-  Analysis of the relationship between parts and suppliers
-  Revenue analysis of small-lot orders
-  Customer analysis for large orders
-  Discounted revenue analysis
-  Analysis of potential component improvements
-  Analysis of suppliers who fail to deliver goods on time
-  Global sales opportunity analysis

.. _dli_01_0021__section1616314111518:

Custom Templates
----------------

The custom template list displays all templates you have created. You can view the template name, description, statements, and more.

.. table:: **Table 2** Template management parameters

   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                          |
   +===================================+======================================================================================================================================================================================================================================================+
   | Name                              | Indicates the template name.                                                                                                                                                                                                                         |
   |                                   |                                                                                                                                                                                                                                                      |
   |                                   | -  A template name can contain only digits, letters, and underscores (_), but cannot start with an underscore (_) or contain only digits. It cannot be left empty.                                                                                   |
   |                                   | -  The template name can contain a maximum of 50 characters.                                                                                                                                                                                         |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Description                       | Description of the template you create.                                                                                                                                                                                                              |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Statement                         | SQL statement created as the template.                                                                                                                                                                                                               |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operation                         | -  **Execute**: After you click this button, the system switches to the **SQL Editor** page, where you can modify or directly perform the statement as required. For details, see :ref:`Executing the Template <dli_01_0021__section1936164995213>`. |
   |                                   | -  **Modify**: Click **Modify**. In the displayed **Modify Template** dialog box, modify the template information as required. For details, see :ref:`Modifying a Template <dli_01_0021__section08698165316>`.                                       |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0021__section73391334165211:

Creating a Template
-------------------

You can create a template on either the **Job Templates** or the **SQL Editor** page.

-  To create a template on the **Job Templates** page:

   #. On the left of the management console, choose **Job Templates** > **SQL Templates**.

   #. On the **SQL Templates** page, click **Create Template** to create a template.

      Enter the template name, SQL statement, and description information. For details, see :ref:`Table 3 <dli_01_0021__table8760202135313>`.

      .. _dli_01_0021__table8760202135313:

      .. table:: **Table 3** Parameter description

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

      Enter the template name, SQL statement, and description information. For details, see :ref:`Table 3 <dli_01_0021__table8760202135313>`.

   #. Click **OK**.

.. _dli_01_0021__section1936164995213:

Executing the Template
----------------------

Perform the template operation as follows:

#. On the left of the management console, choose **Job Templates** > **SQL Templates**.
#. On the **SQL Templates** page, select a template and click **Execute** in the **Operation** column. The **SQL Editor** page is displayed, and the corresponding SQL statement is automatically entered in the SQL job editing window.
#. In the upper right corner of the SQL job editing window, Click **Execute** to run the SQL statement. After the execution is complete, you can view the execution result below the current SQL job editing window.

.. _dli_01_0021__section1045610354536:

Searching for a Template
------------------------

On the **SQL Templates** page, you can enter the template name keyword in the search box on the upper right corner to search for the desired template.

.. _dli_01_0021__section08698165316:

Modifying a Template
--------------------

Only custom templates can be modified. To modify a template, perform the following steps:

#. On the **SQL Templates** page, locate the target template and click **Modify** in the **Operation** column.
#. In the displayed **Modify Template** dialog box, modify the template name, statement, and description as required.
#. Click **OK**.

.. _dli_01_0021__section1317681345320:

Deleting a Template
-------------------

On the **SQL Templates** page, select one or more templates to be deleted and click **Delete** to delete the selected templates.
