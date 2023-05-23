:original_name: dli_01_0010.html

.. _dli_01_0010:

Exporting Data from DLI to OBS
==============================

You can export data from a DLI table to OBS. During the export, a folder is created in OBS or the content in the existing folder is overwritten.

Precautions
-----------

-  The exported file can be in JSON format, and the text format can only be UTF-8.
-  Only the data in the DLI table (the table type is **Managed**) can be exported to the OBS bucket, and the export path must be specified to the folder level.
-  Data can be exported across accounts. That is, after account B authorizes account A, account A can export data to the OBS path of account B if account A has the permission to read the metadata and permission information about the OBS bucket of account B and read and write the path.

Procedure
---------

#. You can export data on either the **Data Management** page or the **SQL Editor** page.

   -  To export data on the **Data Management** page:

      a. On the left of the management console, choose **Data Management** > **Databases and Tables**.
      b. Click the name of the database corresponding to the table where data is to be exported to switch to the **Manage Tables** page.
      c. Select the corresponding table (DLI table) and choose **More** > **Export** in the **Operation** column. The **Export Data** page is displayed.

   -  To export data on the **SQL Editor** page:

      a. On the left of the management console, click **SQL Editor**.
      b. In the navigation tree on the left, click **Databases** to see all databases. Click the database name corresponding to the table to which data is to be exported. The tables are displayed.
      c. Click |image1| on the right of the managed table (DLI table) whose data is to be exported, and choose **Export** from the shortcut menu.

#. In the displayed **Export Data** dialog box, specify parameters by referring to :ref:`Table 1 <dli_01_0010__table1903113115020>`.

   .. _dli_01_0010__table1903113115020:

   .. table:: **Table 1** Parameter description

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                       |
      +===================================+===================================================================================================================================================+
      | Databases                         | Database where the current table is located.                                                                                                      |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | Table Name                        | Name of the current table.                                                                                                                        |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | Data Format                       | Format of the file storing data to be exported. Formats other than JSON will be supported in later versions.                                      |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | Queue                             | Select a queue.                                                                                                                                   |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | Compression Format                | Compression format of the data to be exported. The following compression formats are supported:                                                   |
      |                                   |                                                                                                                                                   |
      |                                   | -  none                                                                                                                                           |
      |                                   | -  bzip2                                                                                                                                          |
      |                                   | -  deflate                                                                                                                                        |
      |                                   | -  gzip                                                                                                                                           |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | Storage Path                      | -  Enter or select an OBS path.                                                                                                                   |
      |                                   | -  The export path must be a folder that does not exist in the OBS bucket. Specifically, you need to create a folder in the target OBS directory. |
      |                                   | -  The folder name cannot contain the special characters of **\\/:*?** **"<>\|**, and cannot start or end with a dot (.).                         |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | Export Mode                       | Storage mode of the data to be exported.                                                                                                          |
      |                                   |                                                                                                                                                   |
      |                                   | -  **New OBS directory**: If the specified export directory exists, an error is reported and the export operation cannot be performed.            |
      |                                   | -  **Existing OBS directory (Overwritten)**: If you create a file in the specified directory, the existing file will be overwritten.              |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | Table Header: No/Yes              | Whether the data to be exported contains the table header.                                                                                        |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

#. (Optional) You can view the job status (indicated by **Status**), statements (indicated by **Statement**), and other information about exporting jobs on the **SQL Jobs** page.

   a. Select **EXPORT** from the **Job Type** drop-down list box and specify the time range for exporting data. The jobs meeting the requirements are displayed in the job list.
   b. Click |image2| to view details about an exporting job.

.. |image1| image:: /_static/images/en-us_image_0237994910.png
.. |image2| image:: /_static/images/en-us_image_0206789824.png
