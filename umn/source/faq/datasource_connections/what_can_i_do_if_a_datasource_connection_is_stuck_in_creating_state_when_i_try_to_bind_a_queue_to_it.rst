:original_name: dli_03_0257.html

.. _dli_03_0257:

What Can I Do If a Datasource Connection Is Stuck in Creating State When I Try to Bind a Queue to It?
=====================================================================================================

The possible causes and solutions are as follows:

-  If you have created a queue, do not bind it to a datasource connection immediately. Wait for 5 to 10 minutes. After the cluster is started in the background, the queue can be bound to the datasource connection.
-  If you have changed the network segment of a queue, do not bind it to a datasource connection immediately. Wait for 5 to 10 minutes. After the cluster is re-created in the background, the creation is successful.
