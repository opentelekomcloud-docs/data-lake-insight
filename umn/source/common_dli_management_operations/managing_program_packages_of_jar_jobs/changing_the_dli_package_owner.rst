:original_name: dli_01_0478.html

.. _dli_01_0478:

Changing the DLI Package Owner
==============================

DLI allows you to change the owner of a package group or package.

#. Log in to the DLI management console and choose **Data Management** > **Package Management**.
#. On the **Package Management** page, locate the package whose owner you want to change, click **More** in the **Operation** column, and select **Modify Owner**.

   -  If the package has been grouped, you can change the owner of the group or package by selecting **Group** or **Packages** for **Select Type** and entering the new owner's username in **Username**.
   -  If the package has not been grouped, change its owner directly.

   .. table:: **Table 1** Description

      +-----------------------------------+-----------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                         |
      +===================================+=====================================================================================================+
      | **Group Name**                    | -  If you select a group when creating a package, the name of the group is displayed.               |
      |                                   | -  If no group is selected when creating a package, this parameter is not displayed.                |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------+
      | **Name**                          | Name of a package.                                                                                  |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------+
      | **Select Type**                   | -  If you select a group when creating a package, you can change the owner of the group or package. |
      |                                   | -  If no group is selected when creating a package, this parameter is not displayed.                |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------+
      | Username                          | Name of the package owner.                                                                          |
      |                                   |                                                                                                     |
      |                                   | .. note::                                                                                           |
      |                                   |                                                                                                     |
      |                                   |    The username is the name of an existing IAM user.                                                |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------+

#. Click **OK**.
