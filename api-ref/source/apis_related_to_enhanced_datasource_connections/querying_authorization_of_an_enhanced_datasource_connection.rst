:original_name: dli_02_0256.html

.. _dli_02_0256:

Querying Authorization of an Enhanced Datasource Connection
===========================================================

Function
--------

This API is used to query the authorization about an enhanced datasource connection.

URI
---

-  URI format

   GET /v2.0/{project_id}/datasource/enhanced-connections/{connection_id}/privileges

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | connection_id | Yes       | String | Connection ID. Identifies the UUID of a datasource connection.                                                                                |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +---------------+-----------+-----------------+----------------------------------------------------------------------------------------------------------------------------------+
   | Parameter     | Mandatory | Type            | Description                                                                                                                      |
   +===============+===========+=================+==================================================================================================================================+
   | is_success    | No        | Boolean         | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed.                |
   +---------------+-----------+-----------------+----------------------------------------------------------------------------------------------------------------------------------+
   | message       | No        | String          | System prompt. If execution succeeds, the parameter setting may be left blank.                                                   |
   +---------------+-----------+-----------------+----------------------------------------------------------------------------------------------------------------------------------+
   | connection_id | No        | String          | Enhanced datasource connection ID, which is used to identify the UUID of a datasource connection.                                |
   +---------------+-----------+-----------------+----------------------------------------------------------------------------------------------------------------------------------+
   | privileges    | No        | Array of Object | Datasource connection information about each authorized project. For details, see :ref:`Table 3 <dli_02_0256__table7853923368>`. |
   +---------------+-----------+-----------------+----------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0256__table7853923368:

.. table:: **Table 3** **privileges** parameters

   +----------------------+-----------+------------------+------------------------------------------+
   | Parameter            | Mandatory | Type             | Description                              |
   +======================+===========+==================+==========================================+
   | object               | No        | String           | Object information during authorization. |
   +----------------------+-----------+------------------+------------------------------------------+
   | applicant_project_id | No        | String           | ID of an authorized project.             |
   +----------------------+-----------+------------------+------------------------------------------+
   | privileges           | No        | Array of Strings | Authorization operation information.     |
   +----------------------+-----------+------------------+------------------------------------------+

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
               "object": "edsconnections.503fc86a-5e60-4349-92c2-7e399404fa8a",
               "applicant_project_id": "330e068af1334c9782f4226acc00a2e2",
               "privileges": ["BIND_QUEUE"]
           }
       ],
       "connection_id": "503fc86a-5e60-4349-92c2-7e399404fa8a"
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0256__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0256__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 4** Status codes

   =========== ===============================
   Status Code Description
   =========== ===============================
   200         The query is successful.
   400         The input parameter is invalid.
   =========== ===============================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.

.. table:: **Table 5** Error codes

   +------------+-----------------------------------------------------------------+
   | Error Code | Error Message                                                   |
   +============+=================================================================+
   | DLI.0001   | Connection 503fc86a-5e60-4349-92c2-7e399404fa8a does not exist. |
   +------------+-----------------------------------------------------------------+
