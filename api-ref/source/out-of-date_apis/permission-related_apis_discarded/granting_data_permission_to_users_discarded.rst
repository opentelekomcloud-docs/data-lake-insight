:original_name: dli_02_0039.html

.. _dli_02_0039:

Granting Data Permission to Users (Discarded)
=============================================

Function
--------

This API is used to grant database or table data usage permission to specified users.

.. note::

   This API has been discarded and is not recommended.

URI
---

-  URI format

   PUT /v1.0/{project_id}/user-authorization

-  Parameter description

   .. table:: **Table 1** URI parameter

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type             | Description                                                                                                                                          |
   +=================+=================+==================+======================================================================================================================================================+
   | user_name       | Yes             | String           | Name of the user who is granted with usage permission on a queue or whose queue usage permission is revoked or updated. Example value: **user2**.    |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | action          | Yes             | String           | Grants or revokes the permission. The parameter value can be **grant**, **revoke**, or **update**. Example value: **grant**.                         |
   |                 |                 |                  |                                                                                                                                                      |
   |                 |                 |                  | -  **grant**: Indicates to grant users with permissions.                                                                                             |
   |                 |                 |                  | -  **revoke**: Indicates to revoke permissions.                                                                                                      |
   |                 |                 |                  | -  **update**: Indicates to clear all the original permissions and assign the permissions in the provided permission array.                          |
   |                 |                 |                  |                                                                                                                                                      |
   |                 |                 |                  | .. note::                                                                                                                                            |
   |                 |                 |                  |                                                                                                                                                      |
   |                 |                 |                  |    Users can perform the **update** operation only when they have been granted with the **grant** and **revoke** permissions.                        |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | privileges      | Yes             | Array of objects | Permission granting information. For details, see :ref:`Table 3 <dli_02_0039__table11117114202212>`. Example value:                                  |
   |                 |                 |                  |                                                                                                                                                      |
   |                 |                 |                  | [ {"object": "databases.db1.tables.tb2.columns.column1","privileges": ["SELECT"]},"object": "databases.db1.tables.tbl","privileges": [ "DROP_TABLE"] |
   +-----------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0039__table11117114202212:

.. table:: **Table 3** privileges parameters

   +-----------------+-----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type             | Description                                                                                                                                |
   +=================+=================+==================+============================================================================================================================================+
   | object          | Yes             | String           | Data objects to be assigned. If they are named:                                                                                            |
   |                 |                 |                  |                                                                                                                                            |
   |                 |                 |                  | -  **databases.**\ *Database name*, data in the entire database will be shared.                                                            |
   |                 |                 |                  |                                                                                                                                            |
   |                 |                 |                  | -  **databases.**\ *Database name*\ **.tables.**\ *Table name*, data in the specified table will be shared.                                |
   |                 |                 |                  |                                                                                                                                            |
   |                 |                 |                  | -  **databases.**\ *Database name*\ **.tables.**\ *Table name*\ **.columns.**\ *Column name*, data in the specified column will be shared. |
   |                 |                 |                  |                                                                                                                                            |
   |                 |                 |                  | -  **jobs.flink.**\ *Flink job ID*, data the specified job will be shared.                                                                 |
   |                 |                 |                  |                                                                                                                                            |
   |                 |                 |                  | -  **groups.**\ *Package group name*, data in the specified package group will be shared.                                                  |
   |                 |                 |                  |                                                                                                                                            |
   |                 |                 |                  | -  **resources.**\ *Package name*, data in the specified package will be shared.                                                           |
   |                 |                 |                  |                                                                                                                                            |
   |                 |                 |                  |    Example value: **databases.db1.tables.tb2.columns.column1**.                                                                            |
   +-----------------+-----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | privileges      | Yes             | Array of Strings | List of permissions to be granted, revoked, or updated. Example value: [**SELECT**].                                                       |
   |                 |                 |                  |                                                                                                                                            |
   |                 |                 |                  | .. note::                                                                                                                                  |
   |                 |                 |                  |                                                                                                                                            |
   |                 |                 |                  |    If **Action** is **Update** and the update list is empty, all permissions of the user in the database or table are revoked.             |
   +-----------------+-----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 4** Response parameters

   +------------+-----------+---------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                                                |
   +============+===========+=========+============================================================================================================================================+
   | is_success | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. Example value: **true**. |
   +------------+-----------+---------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank. Example value: left blank.                                  |
   +------------+-----------+---------+--------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Grant **user2** the permission to query data in the database **db1**, delete the data table **db1.tbl**, and query data in a specified column **db1.tbl.column1** of a data table.

.. code-block::

   {
     "user_name": "user2",
     "action": "grant",
     "privileges": [
       {
         "object": "databases.db1.tables.tb2.columns.column1",
         "privileges": [
           "SELECT"
         ]
       },
       {
         "object": "databases.db1.tables.tbl",
         "privileges": [
           "DROP_TABLE"
         ]
       },
       {
         "object": "databases.db1",
         "privileges": [
           "SELECT"
         ]
       }
     ]
   }

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": ""
   }

Status Codes
------------

:ref:`Table 5 <dli_02_0039__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0039__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 5** Status codes

   =========== =======================
   Status Code Description
   =========== =======================
   200         Authorization succeeds.
   400         Request error.
   500         Internal service error.
   =========== =======================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
