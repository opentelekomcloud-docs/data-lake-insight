:original_name: dli_03_0048.html

.. _dli_03_0048:

How Do I Write Data to Different Elasticsearch Clusters in a Flink Job?
=======================================================================

In a Flink job, you can use **CREATE** statements to define source and sink tables, and specify their connector types and related attributes.

If you need to write data to different Elasticsearch clusters, you need to configure different connection parameters for each cluster and ensure that the Flink job can correctly route data to each cluster.

In this example, the connector type and related attributes are defined for es1 and es2.

Add the following SQL statements to the Flink job:

.. code-block::

   create source stream ssource(xx);
   create sink stream es1(xx) with (xx);
   create sink stream es2(xx) with (xx);
   insert into es1 select * from ssource;
   insert into es2 select * from ssource;
