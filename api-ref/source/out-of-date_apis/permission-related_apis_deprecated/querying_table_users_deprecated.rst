:original_name: dli_02_0041.html

.. _dli_02_0041:

Querying Table Users (Deprecated)
=================================

Function
--------

This API is used to query users who have permission to access the specified table or column in the table.

.. note::

   This API has been deprecated and is not recommended.

URI
---

-  URI format

   GET /v1.0/{project_id}/databases/{database_name}/tables/{table_name}/users

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Description                                                                                                                                   |
      +===============+===========+===============================================================================================================================================+
      | project_id    | Yes       | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | database_name | Yes       | Name of the database where the table to be queried resides.                                                                                   |
      +---------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | table_name    | Yes       | Name of a table that is to be queried.                                                                                                        |
      +---------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +------------+-----------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type            | Description                                                                                                       |
   +============+===========+=================+===================================================================================================================+
   | is_success | No        | Boolean         | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String          | System prompt. If execution succeeds, the parameter setting may be left blank.                                    |
   +------------+-----------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | privileges | No        | Array <Objects> | Permission information. For details, see :ref:`Table 3 <dli_02_0041__table56781141366>`.                          |
   +------------+-----------+-----------------+-------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0041__table56781141366:

.. table:: **Table 3** **privileges** parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                             |
   +=================+=================+=================+=========================================================================================================================================================================================+
   | is_admin        | No              | Boolean         | Determines whether a user is an administrator. The value **false** indicates that the user is not an administrator, and the value **true** indicates that the user is an administrator. |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | object          | No              | String          | Objects on which a user has permission.                                                                                                                                                 |
   |                 |                 |                 |                                                                                                                                                                                         |
   |                 |                 |                 | -  If the object is in the format of **databases.\ Database name.tables.\ Table name**, the user has permission on the database.                                                        |
   |                 |                 |                 | -  If the object is in the format of **databases.\ Database name.tables.\ Table name\ columns.\ Column name**, the user has permission on the table.                                    |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | privileges      | No              | Array<String>   | Permission of the user on the object.                                                                                                                                                   |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | user_name       | No              | String          | Name of the user who has the permission.                                                                                                                                                |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "",
     "privileges": [
       {
         "is_admin": false,
         "object": "databases.dsstest.tables.csv_par_table",
         "privileges": [
           "SELECT"
         ],
         "user_name": "tent2"
       },
       {
         "is_admin": true,
         "object": "databases.dsstest.tables.csv_par_table",
         "privileges": [
           "ALL"
         ],
         "user_name": "tent4"
       }
     ]
   }

.. note::

   If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
