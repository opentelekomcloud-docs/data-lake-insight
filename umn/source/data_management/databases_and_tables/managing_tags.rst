:original_name: dli_01_0552.html

.. _dli_01_0552:

Managing Tags
=============

Tag Management
--------------

A tag is a key-value pair that you can customize to identify cloud resources. It helps you to classify and search for cloud resources. A tag consists of a tag key and a tag value. If you use tags in other cloud services, you are advised to create the same tag (key-value pairs) for cloud resources used by the same business to keep consistency.

DLI supports the following two types of tags:

-  Resource tags: non-global tags created on DLI

-  Predefined tags: global tags created on Tag Management Service (TMS).

This section describes how to add, modify, and delete tags for databases and tables.

Database Tags
-------------

#. In the navigation pane on the left, choose **Data Management** > **Databases and Tables**.

#. Locate the row that contains the target database, and click **More** > **Tags** in the **Operation** column.

#. The tag management page is displayed, and the tags (if there are) are displayed.

#. On the displayed page, click **Add/Edit Tag**. The **Add/Edit Tag** dialog box is displayed.

   Enter a tag key and a tag value in the text boxes and click **Add**.

   .. table:: **Table 1** Tag parameters

      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                               |
      +===================================+===========================================================================================================================================================================================================================================================================================================+
      | Tag key                           | You can specify the tag key in either of the following ways:                                                                                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   | -  Click the text box for tag key and select a predefined tag key from the drop-down list.                                                                                                                                                                                                                |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |    To add a predefined tag, you need to create one on TMS and then select it from the **Tag key** drop-down list. You can click **View predefined tags** to go to the **Predefined Tags** page of the TMS console. Then, click **Create Tag** in the upper corner of the page to create a predefined tag. |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   | -  Enter a tag key in the text box.                                                                                                                                                                                                                                                                       |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |    .. note::                                                                                                                                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |       A tag key can contain a maximum of 128 characters. Only letters, digits, spaces, and special characters\ ``(_.:=+-@)`` are allowed, but the value cannot start or end with a space or start with **\_sys\_**.                                                                                       |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Tag value                         | You can specify the tag value in either of the following ways:                                                                                                                                                                                                                                            |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   | -  Click the tag value text box and select a predefined tag value from the drop-down list.                                                                                                                                                                                                                |
      |                                   | -  Enter a tag value in the text box.                                                                                                                                                                                                                                                                     |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |    .. note::                                                                                                                                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |       A tag value can contain a maximum of 255 characters. Only letters, digits, spaces, and special characters\ ``(_.:=+-@)`` are allowed. The value cannot start or end with a space.                                                                                                                   |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      -  A maximum of 20 tags can be added.
      -  Only one tag value can be added to a tag key.
      -  The key name in each resource must be unique.

#. Click **OK**. The database tag is added.

   To delete a tag, click **Delete** in the **Operation** column of the target tag.

Table Tags
----------

#. In the navigation pane on the left, choose **Data Management** > **Databases and Tables**.

#. Click a database name to view the tables in the database.

#. Locate the row that contains the target table and click **More** > **Tag** in the **Operation** column.

#. The tag management page is displayed, and the tags (if there are) are displayed.

#. On the displayed page, click **Add/Edit Tag**. The **Add/Edit Tag** dialog box is displayed.

   Enter a tag key and a tag value in the text boxes and click **Add**

   .. table:: **Table 2** Tag parameters

      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                               |
      +===================================+===========================================================================================================================================================================================================================================================================================================+
      | Tag key                           | You can specify the tag key in either of the following ways:                                                                                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   | -  Click the text box for tag key and select a predefined tag key from the drop-down list.                                                                                                                                                                                                                |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |    To add a predefined tag, you need to create one on TMS and then select it from the **Tag key** drop-down list. You can click **View predefined tags** to go to the **Predefined Tags** page of the TMS console. Then, click **Create Tag** in the upper corner of the page to create a predefined tag. |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   | -  Enter a tag key in the text box.                                                                                                                                                                                                                                                                       |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |    .. note::                                                                                                                                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |       A tag key can contain a maximum of 128 characters. Only letters, digits, spaces, and special characters\ ``(_.:=+-@)`` are allowed, but the value cannot start or end with a space or start with **\_sys\_**.                                                                                       |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Tag value                         | You can specify the tag value in either of the following ways:                                                                                                                                                                                                                                            |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   | -  Click the tag value text box and select a predefined tag value from the drop-down list.                                                                                                                                                                                                                |
      |                                   | -  Enter a tag value in the text box.                                                                                                                                                                                                                                                                     |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |    .. note::                                                                                                                                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                                                                           |
      |                                   |       A tag value can contain a maximum of 255 characters. Only letters, digits, spaces, and special characters\ ``(_.:=+-@)`` are allowed. The value cannot start or end with a space.                                                                                                                   |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      -  A maximum of 20 tags can be added.
      -  Only one tag value can be added to a tag key.
      -  The key name in each resource must be unique.

#. Click **OK**. The table tag is added.

   To delete a tag, click **Delete** in the **Operation** column of the target tag.
