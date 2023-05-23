:original_name: dli_02_0249.html

.. _dli_02_0249:

Restarting, Scaling Out, and Scaling In Queues
==============================================

Function
--------

This API is used to restart, scale out, and scale in queues.

.. note::

   Only SQL queues in the **Available** status can be restarted. (The queue status is **Available** only after the SQL job is successfully executed.)

URI
---

-  URI format

   PUT /v1.0/{project_id}/queues/{queue_name}/action

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | queue_name | Yes       | String | Name of a queue.                                                                                                                              |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                   |
   +=================+=================+=================+===============================================================================================================================================================================+
   | action          | Yes             | String          | Operations to be performed:                                                                                                                                                   |
   |                 |                 |                 |                                                                                                                                                                               |
   |                 |                 |                 | -  **restart**: Restart a service. Only queues for SQL jobs can be restarted.                                                                                                 |
   |                 |                 |                 | -  **scale_out**: Scale out the queue                                                                                                                                         |
   |                 |                 |                 | -  **scale_in**: Scale in the queue                                                                                                                                           |
   |                 |                 |                 |                                                                                                                                                                               |
   |                 |                 |                 | .. note::                                                                                                                                                                     |
   |                 |                 |                 |                                                                                                                                                                               |
   |                 |                 |                 |    Currently, only **restart**, **scale_out**, and **scale_in** operations are supported.                                                                                     |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | force           | No              | Boolean         | Specifies whether to forcibly restart the queue. This parameter is optional when **action** is set to **restart**. The default value is **false**.                            |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cu_count        | No              | Integer         | Number of CUs to be scaled in or out. This parameter is optional when **action** is set to **scale_out** or **scale_in**. The value of **cu_count** must be a multiple of 16. |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                                 |
   +============+===========+=========+=============================================================================================================================+
   | is_success | No        | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | job_id     | No        | String  | Specifies the job ID returned when **force** is set to **true**.                                                            |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | queue_name | No        | String  | Name of the queue to be scaled in or out.                                                                                   |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | result     | No        | Boolean | Indicates the scaling result.                                                                                               |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

-  Restarting the queue:

   .. code-block::

      {
          "action": "restart",
          "force": "false"
      }

-  Scaling out the queue:

   .. code-block::

      {
          "action": "scale_out",
          "cu_count": 16
      }

Example Response
----------------

-  Set **force** to **false**.

   .. code-block::

      {
          "is_success": true,
          "message": "Restart success"
      }

-  Set **force** to **true**.

   .. code-block::

      {
          "is_success": true,
          "message": "Submit restart job success, it need some time to cancel jobs, please wait for a while and check job status",
          "job_id": "d90396c7-3a25-4944-ad1e-99c764d902e7"
      }

-  Scaling

   .. code-block::

      {
          "queue_name": "myQueue",
          "result": true
      }

Status Codes
------------

:ref:`Table 4 <dli_02_0249__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0249__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   =========== ============================
   Status Code Description
   =========== ============================
   200         The operation is successful.
   400         Request error.
   500         Internal service error.
   =========== ============================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.

.. table:: **Table 5** Error codes

   +------------+----------------------------------------------------------------------------------------------+
   | Error Code | Error Message                                                                                |
   +============+==============================================================================================+
   | DLI.0015   | Token info for token is null, return.                                                        |
   +------------+----------------------------------------------------------------------------------------------+
   | DLI.0013   | X-Auth-Token is not defined in request. It is mandatory. Please define and send the request. |
   +------------+----------------------------------------------------------------------------------------------+
