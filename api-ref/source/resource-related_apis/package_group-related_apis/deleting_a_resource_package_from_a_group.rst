:original_name: dli_02_0173.html

.. _dli_02_0173:

Deleting a Resource Package from a Group
========================================

Function
--------

This API is used to delete resource packages in a group in a **Project**.

URI
---

-  URI format

   DELETE /v2.0/{project_id}/resources/{resource_name}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | resource_name | Yes       | String | Name of the resource package that is uploaded.                                                                                                |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** **query** parameter description

      +-----------+-----------+--------+---------------------------------------------------------------------------+
      | Parameter | Mandatory | Type   | Description                                                               |
      +===========+===========+========+===========================================================================+
      | group     | No        | String | Name of the package group returned when the resource package is uploaded. |
      +-----------+-----------+--------+---------------------------------------------------------------------------+

   .. note::

      The following is an example of the URL containing the **query** parameter:

      DELETE /v2.0/{project_id}/resources/{resource_name}\ *?group={group}*

Request
-------

None

Response
--------

-  Code 200 is returned if you successfully delete a resource package.
-  Code 404 is returned if you initiate a request to delete a resource package that does not exist.

Example Request
---------------

None

Example Response
----------------

None

Status Codes
------------

:ref:`Table 3 <dli_02_0173__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0173__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 3** Status codes

   =========== ===================
   Status Code Description
   =========== ===================
   200         Deletion succeeded.
   404         Not found.
   =========== ===================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.
