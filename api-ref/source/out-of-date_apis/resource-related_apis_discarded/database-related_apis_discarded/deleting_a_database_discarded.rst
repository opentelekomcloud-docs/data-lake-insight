:original_name: dli_02_0030.html

.. _dli_02_0030:

Deleting a Database (Discarded)
===============================

Function
--------

This API is used to delete an empty database. If there are tables in the database to be deleted, delete all tables first. For details about the API used to delete tables, see :ref:`Deleting a Table (Discarded) <dli_02_0035>`.

.. note::

   This API has been discarded and is not recommended.

URI
---

-  URI format

   DELETE /v1.0/{project_id}/databases/{database_name}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | database_name | Yes       | String | Name of the database to be deleted.                                                                                                           |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** query parameter description

      +-----------+-----------+---------+----------------------------------------------------------------------------------------------------------------------------------+
      | Parameter | Mandatory | Type    | Description                                                                                                                      |
      +===========+===========+=========+==================================================================================================================================+
      | cascade   | No        | Boolean | Specifies whether to forcibly delete the database. The value can be **true** or **false**. Default value: **false**.             |
      +-----------+-----------+---------+----------------------------------------------------------------------------------------------------------------------------------+
      | async     | No        | Boolean | Specifies whether to delete the database in asynchronous mode. The value can be **true** or **false**. Default value: **false**. |
      +-----------+-----------+---------+----------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      The following is an example of the URL containing the **query** parameter:

      DELETE /v1.0/{project_id}/databases/{database_name}?cascade=\ *{is_cascade}*\ &async=\ *{is_asyn}*

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
   | job_id          | No              | String          | Returned job ID, which can be used to obtain the job status and result.                                           |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | job_type        | No              | String          | Type of a job. The options are as follows:                                                                        |
   |                 |                 |                 |                                                                                                                   |
   |                 |                 |                 | -  DDL                                                                                                            |
   |                 |                 |                 | -  DCL                                                                                                            |
   |                 |                 |                 | -  IMPORT                                                                                                         |
   |                 |                 |                 | -  EXPORT                                                                                                         |
   |                 |                 |                 | -  QUERY                                                                                                          |
   |                 |                 |                 | -  INSERT                                                                                                         |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------+
   | job_mode        | No              | String          | Job execution mode. The options are as follows:                                                                   |
   |                 |                 |                 |                                                                                                                   |
   |                 |                 |                 | -  **async**: asynchronous                                                                                        |
   |                 |                 |                 | -  **sync**: synchronous                                                                                          |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

-  The following is an example of a successful response in synchronous mode:

   .. code-block::

      {
       "is_success": true,
       "message": "",
       "job_mode": "sync"
      }

-  The following is an example of a successful response in asynchronous mode:

   .. code-block::

      {
       "is_success": true,
       "message": "",
       "job_id": "208b08d4-0dc2-4dd7-8879-ddd4c020d7aa",
       "job_type": "DDL",
       "job_mode": "async"
      }

   .. note::

      -  If the database is deleted asynchronously, you can view the current job status by calling the API for querying job status. For details, see :ref:`Querying Job Status <dli_02_0021>`.
      -  If **cascade** is set to **true**, all tables in the database will be deleted. Exercise caution when performing this operation.

Status Codes
------------

:ref:`Table 4 <dli_02_0030__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0030__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   =========== =======================
   Status Code Description
   =========== =======================
   200         Deletion succeeded.
   400         Request error.
   500         Internal service error.
   =========== =======================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
