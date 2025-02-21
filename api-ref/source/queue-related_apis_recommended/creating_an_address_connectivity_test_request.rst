:original_name: dli_02_0284.html

.. _dli_02_0284:

Creating an Address Connectivity Test Request
=============================================

Function
--------

This API is used to send an address connectivity test request to a specified queue and insert the test address into the table.

URI
---

-  URI format

   POST /v1.0/{project_id}/queues/{queue_name}/connection-test

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

   +-----------+-----------+--------+-------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                             |
   +===========+===========+========+=========================================================================+
   | address   | Yes       | String | Test address. The format is *IP address or domain name*\ **:**\ *port*. |
   +-----------+-----------+--------+-------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                                 |
   +============+===========+=========+=============================================================================================================================+
   | is_success | Yes       | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | message    | Yes       | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | task_id    | Yes       | Integer | Request ID                                                                                                                  |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Test the connectivity between the queue and the address **iam.**\ *xxx*\ **.com:443**.

.. code-block::

   {
       "address": "iam.xxx.com:443"
   }

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "check connectivity to address:iam.xxx.com with port: 443 successfully",
       "task_id": 9
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0284__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0284__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 4** Status codes

   =========== ================================
   Status Code Description
   =========== ================================
   200         The job is created successfully.
   400         Request failure.
   500         Internal service error.
   =========== ================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
