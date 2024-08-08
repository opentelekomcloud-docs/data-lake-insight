:original_name: dli_01_0464.html

.. _dli_01_0464:

Managing Flink Templates
========================

Flink templates include sample templates and custom templates. You can modify an existing sample template to meet the actual job logic requirements and save time for editing SQL statements. You can also customize a job template based on your habits and methods so that you can directly invoke or modify the template in later jobs.

Flink template management provides the following functions:

-  :ref:`Flink SQL Sample Template <dli_01_0464__section3576173115914>`
-  :ref:`Custom Templates <dli_01_0464__section4777152184911>`
-  :ref:`Creating a Template <dli_01_0464__section5417513171115>`
-  :ref:`Creating a Job Based on a Template <dli_01_0464__section123515484542>`
-  :ref:`Modifying a Template <dli_01_0464__section735234815411>`
-  :ref:`Deleting a Template <dli_01_0464__section1035264818548>`

.. _dli_01_0464__section3576173115914:

Flink SQL Sample Template
-------------------------

The template list displays existing sample templates for Flink SQL jobs. :ref:`Table 1 <dli_01_0464__table17778105244916>` describes the parameters in the template list.

The scenarios of sample templates can be different, which are subject to the console.

.. table:: **Table 1** Parameters in the Flink SQL sample template list

   +-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter   | Description                                                                                                                                             |
   +=============+=========================================================================================================================================================+
   | Name        | Name of a template, which has 1 to 64 characters and only contains letters, digits, hyphens (-), and underlines (_).                                    |
   +-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Description | Description of a template. It contains 0 to 512 characters.                                                                                             |
   +-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operation   | **Create Job**: Create a job directly by using the template. After a job is created, the system switches to the **Edit** page under **Job Management**. |
   +-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0464__section4777152184911:

Custom Templates
----------------

The custom template list displays all Jar job templates. :ref:`Table 1 <dli_01_0464__table17778105244916>` describes parameters in the custom template list.

.. _dli_01_0464__table17778105244916:

.. table:: **Table 2** Parameters in the custom template list

   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                |
   +===================================+============================================================================================================================================================+
   | Name                              | Name of a template, which has 1 to 64 characters and only contains letters, digits, hyphens (-), and underlines (_).                                       |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Description                       | Description of a template. It contains 0 to 512 characters.                                                                                                |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Created                           | Time when a template is created.                                                                                                                           |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Updated                           | Latest time when a template is modified.                                                                                                                   |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operation                         | -  **Edit**: Modify a template that has been created.                                                                                                      |
   |                                   | -  **Create Job**: Create a job directly by using the template. After a job is created, the system switches to the **Edit** page under **Job Management**. |
   |                                   | -  **More**:                                                                                                                                               |
   |                                   |                                                                                                                                                            |
   |                                   |    -  **Delete**: Delete a created template.                                                                                                               |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0464__section5417513171115:

Creating a Template
-------------------

You can create a template using any of the following methods:

-  Creating a template on the **Template Management** page

   #. In the left navigation pane of the DLI management console, choose **Job Templates** > **Flink Templates**.

   #. Click **Create Template** in the upper right corner of the page. The **Create Template** dialog box is displayed.

   #. Specify **Name** and **Description**.

      .. table:: **Table 3** Template parameters

         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                                                                                                                                                                         |
         +===================================+=====================================================================================================================================================================================================================================================================================================================+
         | Name                              | Name of a template, which has 1 to 64 characters and only contains letters, digits, hyphens (-), and underlines (_).                                                                                                                                                                                                |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | .. note::                                                                                                                                                                                                                                                                                                           |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   |    The template name must be unique.                                                                                                                                                                                                                                                                                |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Description                       | Description of a template. It contains 0 to 512 characters.                                                                                                                                                                                                                                                         |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Tags                              | Tags used to identify cloud resources. A tag includes the tag key and tag value. If you want to use the same tag to identify multiple cloud resources, that is, to select the same tag from the drop-down list box for all services, you are advised to create predefined tags on the Tag Management Service (TMS). |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | .. note::                                                                                                                                                                                                                                                                                                           |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   |    -  A maximum of 20 tags can be added.                                                                                                                                                                                                                                                                            |
         |                                   |    -  Only one tag value can be added to a tag key.                                                                                                                                                                                                                                                                 |
         |                                   |    -  The key name in each resource must be unique.                                                                                                                                                                                                                                                                 |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | -  Tag key: Enter a tag key name in the text box.                                                                                                                                                                                                                                                                   |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   |    .. note::                                                                                                                                                                                                                                                                                                        |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   |       A tag key can contain a maximum of 128 characters. Only letters, digits, spaces, and special characters\ ``(_.:=+-@)`` are allowed, but the value cannot start or end with a space or start with **\_sys\_**.                                                                                                 |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   | -  Tag value: Enter a tag value in the text box.                                                                                                                                                                                                                                                                    |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   |    .. note::                                                                                                                                                                                                                                                                                                        |
         |                                   |                                                                                                                                                                                                                                                                                                                     |
         |                                   |       A tag value can contain a maximum of 255 characters. Only letters, digits, spaces, and special characters\ ``(_.:=+-@)`` are allowed. The value cannot start or end with a space.                                                                                                                             |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   #. Click **OK** to enter the editing page.

      The :ref:`Table 4 <dli_01_0464__table57746157116>` describes the parameters on the template editing page.

      .. _dli_01_0464__table57746157116:

      .. table:: **Table 4** Template parameters

         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                                  |
         +===================================+==============================================================================================================================================================================+
         | Name                              | You can modify the template name.                                                                                                                                            |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Description                       | You can modify the template description.                                                                                                                                     |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Saving Mode                       | -  **Save Here**: Save the modification to the current template.                                                                                                             |
         |                                   | -  **Save as New**: Save the modification as a new template.                                                                                                                 |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | SQL statement editing area        | In the area, you can enter detailed SQL statements to implement business logic. For details about how to compile SQL statements, see Data Lake Insight SQL Syntax Reference. |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Save                              | Save the modifications.                                                                                                                                                      |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Create Job                        | Use the current template to create a job.                                                                                                                                    |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Format                            | Format SQL statements. After SQL statements are formatted, you need to compile SQL statements again.                                                                         |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Theme Settings                    | Change the font size, word wrap, and page style (black or white background).                                                                                                 |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   #. In the SQL statement editing area, enter SQL statements to implement service logic. For details about how to compile SQL statements, see Data Lake Insight SQL Syntax Reference.

   #. After the SQL statement is edited, click **Save** in the upper right corner to complete the template creation.

   #. (Optional) If you do not need to modify the template, click **Create Job** in the upper right corner to create a job based on the current template. For details about how to create a job, see :ref:`Creating a Flink SQL Job <dli_01_0455>`, and :ref:`Creating a Flink Jar Job <dli_01_0457>`.

-  Creating a template based on an existing job template

   #. In the left navigation pane of the DLI management console, choose **Job Templates** > **Flink Templates**. Click the **Custom Templates** tab.
   #. In the row where the desired template is located in the custom template list, click **Edit** under **Operation** to enter the **Edit** page.
   #. After the modification is complete, set **Saving Mode** to **Save as New**.
   #. Click **Save** in the upper right corner to save the template as a new one.

-  Creating a template using a created job

   #. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.
   #. Click **Create Job** in the upper right corner. The **Create Job** page is displayed.
   #. Specify parameters as required.
   #. Click **OK** to enter the editing page.
   #. After the SQL statement is compiled, click **Set as Template**.
   #. In the **Set as Template** dialog box that is displayed, specify **Name** and **Description** and click **OK**.

-  Creating a template based on the existing job

   #. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.
   #. In the job list, locate the row where the job that you want to set as a template resides, and click **Edit** in the **Operation** column.
   #. After the SQL statement is compiled, click **Set as Template**.
   #. In the **Set as Template** dialog box that is displayed, specify **Name** and **Description** and click **OK**.

.. _dli_01_0464__section123515484542:

Creating a Job Based on a Template
----------------------------------

You can create jobs based on sample templates or custom templates.

#. In the left navigation pane of the DLI management console, choose **Job Templates** > **Flink Templates**.
#. In the sample template list, click **Create Job** in the **Operation** column of the target template. For details about how to create a job, see :ref:`Creating a Flink SQL Job <dli_01_0455>` and :ref:`Creating a Flink Jar Job <dli_01_0457>`.

.. _dli_01_0464__section735234815411:

Modifying a Template
--------------------

After creating a custom template, you can modify it as required. The sample template cannot be modified, but you can view the template details.

#. In the left navigation pane of the DLI management console, choose **Job Templates** > **Flink Templates**. Click the **Custom Templates** tab.
#. In the row where the template you want to modify is located in the custom template list, click **Edit** in the **Operation** column to enter the **Edit** page.
#. In the SQL statement editing area, modify the SQL statements as required.
#. Set **Saving Mode** to **Save Here**.
#. Click **Save** in the upper right corner to save the modification.

.. _dli_01_0464__section1035264818548:

Deleting a Template
-------------------

You can delete a custom template as required. The sample templates cannot be deleted. Deleted templates cannot be restored. Exercise caution when performing this operation.

#. In the left navigation pane of the DLI management console, choose **Job Templates** > **Flink Templates**. Click the **Custom Templates** tab.

#. In the custom template list, select the templates you want to delete and click **Delete** in the upper left of the custom template list.

   Alternatively, you can delete a template by performing the following operations: In the custom template list, locate the row where the template you want to delete resides, and click **More** > **Delete** in the **Operation** column.

#. In the displayed dialog box, click **Yes**.
