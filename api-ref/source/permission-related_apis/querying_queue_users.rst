:original_name: dli_02_0038.html

.. _dli_02_0038:

Querying Queue Users
====================

Function
--------

This API is used to query names of all users who can use a specified queue.

URI
---

-  URI format

   GET /v1.0/{project_id}/queues/{queue_name}/users

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | queue_name | Yes       | String | Name of a queue.                                                                                                                              |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** **query** parameter description

      +-----------+-----------+---------+------------------------------------------------------------+
      | Parameter | Mandatory | Type    | Description                                                |
      +===========+===========+=========+============================================================+
      | limit     | Yes       | Integer | Number of records to be displayed of the page-based query. |
      +-----------+-----------+---------+------------------------------------------------------------+
      | offset    | Yes       | Integer | Specifies the offset of the page-based query.              |
      +-----------+-----------+---------+------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 3** Response parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                       |
   +=================+=================+=================+===================================================================================================================+
   | is_success      | No              | Boolean         | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | message         | No              | String          | System prompt. If execution succeeds, the parameter setting may be left blank.                                    |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | queue_name      | No              | String          | Name of a queue.                                                                                                  |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | privileges      | No              | Array of Object | Users who are granted with the permission to use this queue and the permission array to which users belong.       |
   |                 |                 |                 |                                                                                                                   |
   |                 |                 |                 | For details, see :ref:`Table 4 <dli_02_0038__table34433526275>`.                                                  |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0038__table34433526275:

.. table:: **Table 4** **privileges** parameters

   +------------+-----------+------------------+-----------------------------------------------------------+
   | Parameter  | Mandatory | Type             | Description                                               |
   +============+===========+==================+===========================================================+
   | is_admin   | No        | Boolean          | Whether the database user is an administrator.            |
   +------------+-----------+------------------+-----------------------------------------------------------+
   | user_name  | No        | String           | Name of the user who has permission on the current queue. |
   +------------+-----------+------------------+-----------------------------------------------------------+
   | privileges | No        | Array of Strings | Permission of the user on the queue.                      |
   +------------+-----------+------------------+-----------------------------------------------------------+

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
         "is_admin": true,
         "privileges": [
           "ALL"
         ],
         "user_name": "tenant1"
       },
       {
         "is_admin": false,
         "privileges": [
           "SUBMIT_JOB"
         ],
         "user_name": "user2"
       }
     ],
     "queue_name": "queue1"
   }

Status Codes
------------

:ref:`Table 5 <dli_02_0038__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0038__tb12870f1c5f24b27abd55ca24264af36:

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

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.
