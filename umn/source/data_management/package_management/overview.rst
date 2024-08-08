:original_name: dli_01_0407.html

.. _dli_01_0407:

Overview
========

Package management provides the following functions:

-  :ref:`Managing Package Permissions <dli_01_0477>`
-  :ref:`Creating a Package <dli_01_0367>`
-  :ref:`Deleting a Package <dli_01_0369>`

   .. note::

      You can delete program packages in batches.

-  :ref:`Modifying the Owner <dli_01_0478>`

Notes and Constraints
---------------------

-  A package can be deleted, but a package group cannot be deleted.
-  The following types of packages can be uploaded:

   -  **JAR**: JAR file
   -  **PyFile**: User Python file
   -  **File**: User file
   -  **ModelFile**: User AI model file

Package Management Page
-----------------------

.. table:: **Table 1** Package management parameters

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
