:original_name: dli_01_0672.html

.. _dli_01_0672:

Configuring Data Catalog Permissions on the DLI Console
=======================================================

Scenario
--------

-  DLI data catalogs support authorization on the DLI console or authentication through IAM. By setting permissions, you can grant varying data catalog permissions to different users.
-  Administrators and data catalog owners (users who create data catalogs on DLI) have all data catalog permissions by default. You do not need to set permissions for them, and other users cannot modify their data catalog permissions.

Precautions
-----------

-  Data catalog permissions are non-inherited permissions, meaning they apply only to the current data catalog. Databases and tables within the data catalog cannot inherit any permissions from it.
-  Data catalog owners or users with the **Assign catalog access to specified users** permission can grant permissions to data catalogs.
-  If you create a data catalog with the same name after deleting an existing one, the permissions will not be inherited and must be regranted to users.

Data Catalog Permissions
------------------------

You can also use IAM to grant data catalog permissions to specified users. For details about data catalog permissions, see :ref:`Table 1 <dli_01_0672__table15953174015178>`.

.. _dli_01_0672__table15953174015178:

.. table:: **Table 1** Data catalog permissions

   +----------------------------------------+------------------------------------------+----------------------------------+-------------------------+-------------------------+
   | Operation                              | Permission Set (service:resource:action) | Authorization on the DLI Console | API-based Authorization | IAM-based Authorization |
   +========================================+==========================================+==================================+=========================+=========================+
   | Unbinding a data catalog               | dli:catalog:unbind                       | Supported                        | Supported               | Supported               |
   +----------------------------------------+------------------------------------------+----------------------------------+-------------------------+-------------------------+
   | Querying data catalog binding details  | dli:catalog:get                          | Supported                        | Supported               | Supported               |
   +----------------------------------------+------------------------------------------+----------------------------------+-------------------------+-------------------------+
   | Granting permissions                   | dli: catalog: grantPrivilege             | Supported                        | Supported               | Supported               |
   +----------------------------------------+------------------------------------------+----------------------------------+-------------------------+-------------------------+
   | Revoking permissions                   | dli: catalog: revokePrivilege            | Supported                        | Supported               | Supported               |
   +----------------------------------------+------------------------------------------+----------------------------------+-------------------------+-------------------------+
   | Viewing permissions of other users     | dli: catalog: showPrivileges             | Supported                        | Supported               | Supported               |
   +----------------------------------------+------------------------------------------+----------------------------------+-------------------------+-------------------------+
   | Binding a data catalog                 | dli:catalog:bind                         | Not supported                    | Supported               | Supported               |
   +----------------------------------------+------------------------------------------+----------------------------------+-------------------------+-------------------------+
   | Querying the data catalog binding list | dli:catalog:list                         | Not supported                    | Supported               | Supported               |
   +----------------------------------------+------------------------------------------+----------------------------------+-------------------------+-------------------------+

Granting Data Catalog Permissions to a New User on the DLI Management Console
-----------------------------------------------------------------------------

Grant permissions to a new user or project that previously did not have permissions on this data catalog.

#. In the navigation pane on the left of the management console, choose **SQL Editor**.

#. On the displayed **Catalog** tab, locate the data catalog you want to view, click |image1|, and select **Permissions**.

#. On the displayed page, click **Grant Permission** in the upper right corner. In the dialog box that appears, enter the username you want to grant permissions to and select required permissions. For details about the permissions, see :ref:`Table 2 <dli_01_0672__table88751410195512>`.

   .. _dli_01_0672__table88751410195512:

   .. table:: **Table 2** Parameter descriptions

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                       |
      +===================================+===================================================================================================================================================================================================+
      | Username                          | Name of an IAM user you wish to grant permissions to.                                                                                                                                             |
      |                                   |                                                                                                                                                                                                   |
      |                                   | .. note::                                                                                                                                                                                         |
      |                                   |                                                                                                                                                                                                   |
      |                                   |    The username must be an existing IAM username and has been used to log in to the DLI management console.                                                                                       |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Permission                        | Selecting a permission grants it to a user, while deselecting a permission revokes it from the user.                                                                                              |
      |                                   |                                                                                                                                                                                                   |
      |                                   | Data catalog permissions are non-inherited permissions, meaning they apply only to the current data catalog. Databases and tables within the data catalog cannot inherit any permissions from it. |
      |                                   |                                                                                                                                                                                                   |
      |                                   | -  **Unbind catalogs**: permission to unbind the data catalog from DLI.                                                                                                                           |
      |                                   | -  **Query the details of bound catalogs**: permission to view data catalog binding information. This permission is required if you need to use the data catalog when submitting jobs.            |
      |                                   | -  **Assign catalog access to specified users**: permission to grant permissions on a data catalog to specified users.                                                                            |
      |                                   | -  **Revoke catalog access from specified users**: permission to revoke permissions on a data catalog from specified users.                                                                       |
      |                                   | -  **View catalog access rights for other users**: permission to view the permissions of other users in the current data catalog.                                                                 |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

Modifying Permissions on a Data Catalog
---------------------------------------

If a user has certain permissions on a data catalog, you can modify or revoke those permissions for the user.

.. note::

   If the options in **Set Permission** are gray, the corresponding account does not have the permission to modify the data catalog. You can request the **Assign catalog access to specified users** and **Revoke catalog access from specified users** permissions from administrators, data catalog owners, or other authorized users with permission-granting permissions.

#. On the **User Permissions** page, locate the user you wish to set permissions for.

   -  If the user is an IAM user, you can set permissions for it.
   -  If the user is already an administrator, you can only view the permissions information.

#. In the **Operation** column of the IAM user or project, click **Set Permission**. The **Set Permission** dialog box appears.

   For details about data catalog permissions, see :ref:`Table 2 <dli_01_0672__table88751410195512>`.

#. Select or deselect the permissions and click **OK**.

.. |image1| image:: /_static/images/en-us_image_0000002337417937.png
