:original_name: dli_02_0247.html

.. _dli_02_0247:

Deleting a Template
===================

Function
--------

This API is used to delete a template. A template used by jobs can also be deleted.

URI
---

-  URI format

   DELETE /v1.0/{project_id}/streaming/job-templates/{template_id}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +-------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter   | Mandatory | Type   | Description                                                                                                                                   |
      +=============+===========+========+===============================================================================================================================================+
      | project_id  | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +-------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | template_id | Yes       | String | Template ID.                                                                                                                                  |
      +-------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

-  Parameter description

   .. table:: **Table 2** Response parameters

      +------------+-----------+---------+----------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type    | Description                                                                                                    |
      +============+===========+=========+================================================================================================================+
      | is_success | No        | Boolean | Indicates whether the response is successful. Value **true** indicates success.                                |
      +------------+-----------+---------+----------------------------------------------------------------------------------------------------------------+
      | message    | No        | String  | Message content.                                                                                               |
      +------------+-----------+---------+----------------------------------------------------------------------------------------------------------------+
      | template   | No        | Object  | Information about the template to be deleted. For details, see :ref:`Table 3 <dli_02_0247__table42271991538>`. |
      +------------+-----------+---------+----------------------------------------------------------------------------------------------------------------+

   .. _dli_02_0247__table42271991538:

   .. table:: **Table 3** **template** parameters

      =========== ========= ==== ============
      Parameter   Mandatory Type Description
      =========== ========= ==== ============
      template_id No        Long Template ID.
      =========== ========= ==== ============

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "is_success": "true",
       "message": "The template is deleted successfully.",
       "template": {
           "template_id": 2
       }
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0247__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0247__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 4** Status codes

   =========== ===================================
   Status Code Description
   =========== ===================================
   200         A template is deleted successfully.
   400         The input parameter is invalid.
   =========== ===================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.
