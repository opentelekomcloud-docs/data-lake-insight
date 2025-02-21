:original_name: dli_01_0369.html

.. _dli_01_0369:

Managing DLI Package Tags
=========================

Tags are key-value pairs that you can define to identify cloud resources. They assist you in categorizing and searching for cloud resources. A tag consists of a key and a value.

DLI allows you to add tags to package groups or packages.

#. Log in to the DLI management console and choose **Data Management** > **Package Management**.
#. On the **Package Management** page, locate the desired package, click **More** in the **Operation** column, and select **Tags**.
#. On the page that appears, click **Add/Edit Tag** in the upper left corner.
#. In the **Add/Edit Tag** dialog box, set parameters.

   .. table:: **Table 1** Tag parameters

      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                           |
      +===================================+=======================================================================================================================================================================================================================================================================================================+
      | Tag key                           | You can specify the tag key in either of the following ways:                                                                                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                                                                       |
      |                                   | -  Click the text box and select a predefined tag key from the drop-down list.                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                       |
      |                                   |    To add a predefined tag, you need to create one on TMS and then select it from the tag key drop-down list. You can click **View predefined tags** to go to the **Predefined Tags** page of the TMS console. Then, click **Create Tag** in the upper corner of the page to create a predefined tag. |
      |                                   |                                                                                                                                                                                                                                                                                                       |
      |                                   | -  Enter a tag key in the text box.                                                                                                                                                                                                                                                                   |
      |                                   |                                                                                                                                                                                                                                                                                                       |
      |                                   |    .. note::                                                                                                                                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                                                                       |
      |                                   |       A tag key can contain a maximum of 128 characters. Only letters, numbers, spaces, and special characters ``(_.:+-@)`` are allowed, but the value cannot start or end with a space or start with **\_sys\_**.                                                                                    |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Tag value                         | You can specify the tag value in either of the following ways:                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                       |
      |                                   | -  Click the text box and select a predefined tag value from the drop-down list.                                                                                                                                                                                                                      |
      |                                   | -  Enter a tag value in the text box.                                                                                                                                                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                                                                                       |
      |                                   |    .. note::                                                                                                                                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                                                                       |
      |                                   |       A tag value can contain a maximum of 255 characters. Only letters, numbers, spaces, and special characters ``(_.:+-@)`` are allowed.                                                                                                                                                            |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      -  A maximum of 20 tags can be added.
      -  Only one tag value can be added to a tag key.
      -  The key name in each resource must be unique.

#. Click **OK**.
#. (Optional) To delete a tag, locate the tag in the tag list and click **Delete** in its **Operation** column.
