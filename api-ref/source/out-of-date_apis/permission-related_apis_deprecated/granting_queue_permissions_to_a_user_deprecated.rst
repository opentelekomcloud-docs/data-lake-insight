:original_name: dli_02_0037.html

.. _dli_02_0037:

Granting Queue Permissions to a User (Deprecated)
=================================================

Function
--------

This API is used to share a specific queue with other users. You can grant users with the permission to use the specified queue or revoke the permission.

.. note::

   This API has been deprecated and is not recommended.

URI
---

-  URI format

   PUT /v1.0/{project_id}/queues/user-authorization

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

   +-----------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type             | Description                                                                                                                                                                                                                                             |
   +=================+=================+==================+=========================================================================================================================================================================================================================================================+
   | queue_name      | Yes             | String           | Name of a queue. Example value: **queue1**.                                                                                                                                                                                                             |
   +-----------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | user_name       | Yes             | String           | Name of the user who is granted with usage permission on a queue or whose queue usage permission is revoked or updated. Example value: **tenant2**.                                                                                                     |
   +-----------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | action          | Yes             | String           | Grants or revokes the permission. The parameter value can be **grant**, **revoke**, or **update**. Users can perform the **update** operation only when they have been granted with the **grant** and **revoke** permissions. Example value: **grant**. |
   |                 |                 |                  |                                                                                                                                                                                                                                                         |
   |                 |                 |                  | -  **grant**: Indicates to grant users with permissions.                                                                                                                                                                                                |
   |                 |                 |                  | -  **revoke**: Indicates to revoke permissions.                                                                                                                                                                                                         |
   |                 |                 |                  | -  **update**: Indicates to clear all the original permissions and assign the permissions in the provided permission array.                                                                                                                             |
   +-----------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | privileges      | Yes             | Array of Strings | List of permissions to be granted, revoked, or updated. The following permissions are supported: Example value: [**DROP_QUEUE**, **SUBMIT_JOB**].                                                                                                       |
   |                 |                 |                  |                                                                                                                                                                                                                                                         |
   |                 |                 |                  | -  **SUBMIT_JOB**: indicates to submit a job.                                                                                                                                                                                                           |
   |                 |                 |                  | -  **CANCEL_JOB**: indicates to cancel a job.                                                                                                                                                                                                           |
   |                 |                 |                  | -  **DROP_QUEUE**: indicates to a delete a queue.                                                                                                                                                                                                       |
   |                 |                 |                  | -  **GRANT_PRIVILEGE**: indicates to assign a permission.                                                                                                                                                                                               |
   |                 |                 |                  | -  **REVOKE_PRIVILEGE**: indicates to revoke a permission.                                                                                                                                                                                              |
   |                 |                 |                  | -  **SHOW_PRIVILEGES**: indicates to view the permissions of other users                                                                                                                                                                                |
   |                 |                 |                  | -  **RESTART**: indicates to restart the queue.                                                                                                                                                                                                         |
   |                 |                 |                  | -  **SCALE_QUEUE**: indicates to change the queue specifications.                                                                                                                                                                                       |
   |                 |                 |                  |                                                                                                                                                                                                                                                         |
   |                 |                 |                  |    .. note::                                                                                                                                                                                                                                            |
   |                 |                 |                  |                                                                                                                                                                                                                                                         |
   |                 |                 |                  |       If the update list is empty, all permissions of the queue granted to the user are revoked.                                                                                                                                                        |
   +-----------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+---------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                                                          |
   +============+===========+=========+======================================================================================================================================================+
   | is_success | No        | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. Example value: **true**. |
   +------------+-----------+---------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank. Example value: left blank.                                            |
   +------------+-----------+---------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Grant a user the permission to submit jobs on queue1 and delete queue1.

.. code-block::

   {
       "queue_name": "queue1",
       "user_name": "tenant2",
       "action": "grant",
       "privileges" : ["DROP_QUEUE", "SUBMIT_JOB"]
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

:ref:`Table 4 <dli_02_0037__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0037__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   ============ =======================
   Status Codes Description
   ============ =======================
   200          Authorization succeeds.
   400          Request error.
   500          Internal service error.
   ============ =======================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
