:original_name: dli_02_0040.html

.. _dli_02_0040:

Querying Database Users (Deprecated)
====================================

Function
--------

This API is used query names of all users who have permission to use or access the database.

.. note::

   This API has been deprecated and is not recommended.

URI
---

-  URI format

   GET /v1.0/{project_id}/databases/{database_name}/users

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Description                                                                                                                                   |
      +===============+===========+===============================================================================================================================================+
      | project_id    | Yes       | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | database_name | Yes       | Name of the database to be queried.                                                                                                           |
      +---------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +---------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter     | Mandatory | Type             | Description                                                                                                                                |
   +===============+===========+==================+============================================================================================================================================+
   | is_success    | No        | Boolean          | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. Example value: **true**. |
   +---------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | message       | No        | String           | System prompt. If execution succeeds, the parameter setting may be left blank. Example value: left blank.                                  |
   +---------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | database_name | No        | String           | Name of the database to be queried. Example value: **dsstest**.                                                                            |
   +---------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | privileges    | No        | Array of objects | Permission information. For details, see :ref:`Table 3 <dli_02_0040__table34433526275>`.                                                   |
   +---------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0040__table34433526275:

.. table:: **Table 3** **privileges** parameters

   +------------+-----------+------------------+-----------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type             | Description                                                                             |
   +============+===========+==================+=========================================================================================+
   | is_admin   | No        | Boolean          | Whether the database user is an administrator. Example value: **true**.                 |
   +------------+-----------+------------------+-----------------------------------------------------------------------------------------+
   | user_name  | No        | String           | Name of the user who has permission on the current database. Example value: **test**.   |
   +------------+-----------+------------------+-----------------------------------------------------------------------------------------+
   | privileges | No        | Array of Strings | Permission of the user on the database. Example value: [**ALTER_TABLE_ADD_PARTITION**]. |
   +------------+-----------+------------------+-----------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "",
     "database_name": "dsstest",
     "privileges": [
       {
         "is_admin": true,
         "privileges": [
           "ALL"
         ],
         "user_name": "test"
       },
       {
         "is_admin": false,
         "privileges": [
           "ALTER_TABLE_ADD_PARTITION"
         ],
         "user_name": "scuser1"
       },
       {
         "is_admin": false,
         "privileges": [
           "CREATE_TABLE"
         ],
         "user_name": "scuser2"
       }
     ]
   }

.. note::

   If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
