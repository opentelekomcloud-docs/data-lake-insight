:original_name: dli_03_0060.html

.. _dli_03_0060:

Why Is Error "unsupported media Type" Reported When I Subimt a SQL Job?
=======================================================================

In the REST API provided by DLI, the request header can be added to the request URI, for example, **Content-Type**.

**Content-Type** indicates the request body type or format. The default value is **application/json**.

URI for submitting a SQL job: **POST /v1.0/{project_id}/jobs/submit-job**

**Content-Type** can be only **application/json**. If **Content-Type** is set to **text**, "unsupported media Type" is displayed.
