:original_name: dli_01_0407.html

.. _dli_01_0407:

Package Management Overview
===========================

Before running DLI jobs, UDF JAR files or Jar job packages need to be uploaded to the cloud platform for unified management and maintenance.

There are two ways to manage packages:

-  (Recommended) Upload packages to OBS: Upload Jar packages to an OBS bucket in advance and select the OBS path when configuring a job.
-  Upload packages to DLI: Upload Jar packages to an OBS bucket in advance, create a package on the **Data Management** > **Package Management** page of the DLI management console, and select the DLI package when configuring a job.

This section describes how to upload and manage packages on the DLI management console.

.. note::

   -  When using Spark 3.3.1 or later or Flink1.15 or later to run jobs, you are advised to select packages stored in OBS.
   -  When packaging Spark or Flink Jar jobs, do not upload the dependency packages that the platform already has to avoid conflicts with the built-in dependency packages of the platform. Refer to :ref:`DLI Built-in Dependencies <dli_01_0397>` for built-in dependency packages.

Notes and Constraints
---------------------

.. table:: **Table 1** Notes and constraints on package usage

   +-----------------------------------+---------------------------------------------------------------------+
   | Item                              | Description                                                         |
   +===================================+=====================================================================+
   | Package                           | -  A package can be deleted, but a package group cannot be deleted. |
   |                                   | -  The following types of packages can be uploaded:                 |
   |                                   |                                                                     |
   |                                   |    -  **JAR**: JAR file                                             |
   |                                   |    -  **PyFile**: User Python file                                  |
   |                                   |    -  **File**: User file                                           |
   |                                   |    -  **ModelFile**: User AI model file                             |
   +-----------------------------------+---------------------------------------------------------------------+

Package Management Page
-----------------------

.. table:: **Table 2** Package management parameters

   +-----------------------------------+-----------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                         |
   +===================================+=====================================================================================================+
   | Group Name                        | Name of the group to which the package belongs. If the package is not grouped, **--** is displayed. |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------+
   | Package Name                      | Name of a package.                                                                                  |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------+
   | Owner                             | Name of the user who uploads the package.                                                           |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------+
   | Type                              | Type of a package. The following package types are supported:                                       |
   |                                   |                                                                                                     |
   |                                   | -  JAR: JAR file                                                                                    |
   |                                   | -  PyFile: User Python file                                                                         |
   |                                   | -  File: User file                                                                                  |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------+
   | Status                            | Status of the package to be created.                                                                |
   |                                   |                                                                                                     |
   |                                   | -  Uploading: The file is being uploaded.                                                           |
   |                                   | -  Finished: The resource package has been uploaded.                                                |
   |                                   | -  Failed: The resource package upload failed.                                                      |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------+
   | Created                           | Time when a package is created.                                                                     |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------+
   | Updated                           | Time when the package is updated.                                                                   |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------+
   | Operation                         | Manage Permissions: Manage user permissions for a package.                                          |
   |                                   |                                                                                                     |
   |                                   | Delete: Delete the package.                                                                         |
   |                                   |                                                                                                     |
   |                                   | **More**:                                                                                           |
   |                                   |                                                                                                     |
   |                                   | -  **Modify Owner**: Modify the owner of the package.                                               |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------+
