:original_name: dli_01_0367.html

.. _dli_01_0367:

Creating a Package
==================

DLI allows you to submit program packages in batches to the general-use queue for running.

.. note::

   If you need to update a package, you can use the same package or file to upload it to the same location (in the same group) on DLI to overwrite the original package or file.

Prerequisites
-------------

All software packages must be uploaded to OBS for storage in advance.

Procedure
---------

#. On the left of the management console, choose **Data Management** > **Package Management**.

#. On the **Package Management** page, click **Create** in the upper right corner to create a package.

#. In the displayed **Create Package** dialog box, set related parameters by referring to :ref:`Table 1 <dli_01_0367__en-us_topic_0122016946_en-us_topic_0093946917_table19616613171536>`.

   .. _dli_01_0367__en-us_topic_0122016946_en-us_topic_0093946917_table19616613171536:

   .. table:: **Table 1** Parameter description

      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                  |
      +===================================+==============================================================================================================================================================+
      | Package Type                      | The following package types are supported:                                                                                                                   |
      |                                   |                                                                                                                                                              |
      |                                   | -  JAR: JAR file                                                                                                                                             |
      |                                   | -  PyFile: User Python file                                                                                                                                  |
      |                                   | -  File: User file                                                                                                                                           |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Package File Path                 | Select the OBS path of the corresponding packages.                                                                                                           |
      |                                   |                                                                                                                                                              |
      |                                   | .. note::                                                                                                                                                    |
      |                                   |                                                                                                                                                              |
      |                                   |    -  The packages must be uploaded to OBS for storage in advance.                                                                                           |
      |                                   |    -  Only files can be selected.                                                                                                                            |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Group Policy                      | You can select **Use existing group**, **Use new group**, or **No grouping**.                                                                                |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Group Name                        | -  **Use existing group**: Select an existing group.                                                                                                         |
      |                                   | -  **Use new group**: Enter a custom group name.                                                                                                             |
      |                                   | -  **No grouping**: No need to select or enter a group name.                                                                                                 |
      |                                   |                                                                                                                                                              |
      |                                   | .. note::                                                                                                                                                    |
      |                                   |                                                                                                                                                              |
      |                                   |    -  If you select a group, the permission management refers to the permissions of the corresponding package group.                                         |
      |                                   |    -  If no group is selected, the permission management refers to the permissions of the corresponding package.                                             |
      |                                   |                                                                                                                                                              |
      |                                   |    For details about how to manage permissions on package groups and packages, see :ref:`Managing Permissions on Packages and Package Groups <dli_01_0477>`. |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

   After a package is created, you can view and select the package for use on the **Package Management** page.
