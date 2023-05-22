:original_name: dli_02_0042.html

.. _dli_02_0042:

Querying a User's Table Permissions
===================================

Function
--------

This API is used to query the permission of a specified user on a table.

URI
---

-  URI format

   GET /v1.0/{project_id}/databases/{database_name}/tables/{table_name}/users/{user_name}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | database_name | Yes       | String | Name of the database where the table to be queried resides.                                                                                   |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | table_name    | Yes       | String | Name of a table that is to be queried.                                                                                                        |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | user_name     | Yes       | String | Name of the user whose permission is to be queried.                                                                                           |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type             | Description                                                                                                       |
   +============+===========+==================+===================================================================================================================+
   | is_success | No        | Boolean          | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String           | System prompt. If execution succeeds, the parameter setting may be left blank.                                    |
   +------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | user_name  | No        | String           | Name of the user whose permission is to be queried.                                                               |
   +------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | privileges | No        | Array Of objects | Permission information. For details, see :ref:`Table 3 <dli_02_0042__table912853564418>`.                         |
   +------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0042__table912853564418:

.. table:: **Table 3** privileges parameters

   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type             | Description                                                                                                                                          |
   +=================+=================+==================+======================================================================================================================================================+
   | object          | No              | String           | Objects on which a user has permission.                                                                                                              |
   |                 |                 |                  |                                                                                                                                                      |
   |                 |                 |                  | -  If the object is in the format of **databases.\ Database name.tables.\ Table name**, the user has permission on the database.                     |
   |                 |                 |                  | -  If the object is in the format of **databases.\ Database name.tables.\ Table name\ columns.\ Column name**, the user has permission on the table. |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | privileges      | No              | Array of Strings | Permission of the user on a specified object.                                                                                                        |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+

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
         "object": "databases.dsstest.tables.obs_2312",
         "privileges": [
           "DESCRIBE_TABLE"
         ]
       },
       {
         "object": "databases.dsstest.tables.obs_2312.columns.id",
         "privileges": [
           "SELECT"
         ]
       }
     ],
     "user_name": "scuser1"
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0042__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0042__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   =========== =======================
   Status Code Description
   =========== =======================
   200         Authorization succeeds.
   400         Request error.
   500         Internal service error.
   =========== =======================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.
