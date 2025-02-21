:original_name: dli_03_0069.html

.. _dli_03_0069:

How Can I Use the count Function to Perform Aggregation?
========================================================

The correct method for using the count function to perform aggregation is as follows:

.. code-block::

   SELECT
     http_method,
     count(http_method)
   FROM
     apigateway
   WHERE
     service_id = 'ecs' Group BY http_method

Or

.. code-block::

   SELECT
     http_method
   FROM
     apigateway
   WHERE
     service_id = 'ecs' DISTRIBUTE BY http_method

If an incorrect method is used, an error will be reported.

.. code-block::

   SELECT
     http_method,
     count(http_method)
   FROM
     apigateway
   WHERE
     service_id = 'ecs' DISTRIBUTE BY http_method
