:original_name: dli_02_0245.html

.. _dli_02_0245:

Creating a Template
===================

Function
--------

This API is used to create a user template for the DLI service. A maximum of 100 user templates can be created.

URI
---

-  URI format

   POST /v1.0/{project_id}/streaming/job-templates

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

   +-----------+-----------+------------------+----------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type             | Description                                                                                                                            |
   +===========+===========+==================+========================================================================================================================================+
   | name      | Yes       | String           | Template name. The value can contain 1 to 64 characters.                                                                               |
   +-----------+-----------+------------------+----------------------------------------------------------------------------------------------------------------------------------------+
   | desc      | No        | String           | Template description. Length range: 0 to 512 characters.                                                                               |
   +-----------+-----------+------------------+----------------------------------------------------------------------------------------------------------------------------------------+
   | sql_body  | No        | String           | Stream SQL statement, which includes at least the following three parts: source, query, and sink. Length range: 0 to 2,048 characters. |
   +-----------+-----------+------------------+----------------------------------------------------------------------------------------------------------------------------------------+
   | tags      | No        | Array of Objects | Label of a Flink job template. For details, see :ref:`Table 3 <dli_02_0245__table9391124139>`.                                         |
   +-----------+-----------+------------------+----------------------------------------------------------------------------------------------------------------------------------------+
   | job_type  | No        | String           | Flink job template type.                                                                                                               |
   +-----------+-----------+------------------+----------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0245__table9391124139:

.. table:: **Table 3** tags parameter

   ========= ========= ====== ===========
   Parameter Mandatory Type   Description
   ========= ========= ====== ===========
   key       Yes       String Tag key.
   value     Yes       String Tag key.
   ========= ========= ====== ===========

Response
--------

.. table:: **Table 4** Response parameters

   +------------+-----------+---------+---------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                       |
   +============+===========+=========+===================================================================================================+
   | is_success | No        | Boolean | Indicates whether the request is successful.                                                      |
   +------------+-----------+---------+---------------------------------------------------------------------------------------------------+
   | message    | No        | String  | Message content.                                                                                  |
   +------------+-----------+---------+---------------------------------------------------------------------------------------------------+
   | template   | No        | Object  | Information about job update. For details, see :ref:`Table 5 <dli_02_0245__table14147151125319>`. |
   +------------+-----------+---------+---------------------------------------------------------------------------------------------------+

.. _dli_02_0245__table14147151125319:

.. table:: **Table 5** **template** parameters

   =========== ========= ====== ==================================
   Parameter   Mandatory Type   Description
   =========== ========= ====== ==================================
   template_id No        Long   Template ID.
   name        No        String Template name.
   desc        No        String Template description.
   create_time No        Long   Time when the template is created.
   job_type    No        String Job template type
   =========== ========= ====== ==================================

Example Request
---------------

Create a job template named **simple_stream_sql**.

.. code-block::

   {
       "name": "simple_stream_sql",
       "desc": "Example of quick start",
       "sql_body": "select * from source_table"
   }

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "A template is created successfully.",
       "template": {
           "template_id": 0,
           "name": "IoT_example",
          "desc": "Example of quick start",
           "create_time": 1516952710040,
           "job_type": "flink_sql_job"
       }
   }

Status Codes
------------

:ref:`Table 6 <dli_02_0245__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0245__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 6** Status codes

   =========== ===================================
   Status Code Description
   =========== ===================================
   200         A template is created successfully.
   400         The input parameter is invalid.
   =========== ===================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
