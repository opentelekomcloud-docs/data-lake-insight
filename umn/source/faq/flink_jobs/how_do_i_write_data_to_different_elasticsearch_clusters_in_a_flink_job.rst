:original_name: dli_03_0048.html

.. _dli_03_0048:

How Do I Write Data to Different Elasticsearch Clusters in a Flink Job?
=======================================================================

Add the following SQL statements to the Flink job:

.. code-block::

   create source stream ssource(xx);
   create sink stream es1(xx) with (xx);
   create sink stream es2(xx) with (xx);
   insert into es1 select * from ssource;
   insert into es2 select * from ssource;
