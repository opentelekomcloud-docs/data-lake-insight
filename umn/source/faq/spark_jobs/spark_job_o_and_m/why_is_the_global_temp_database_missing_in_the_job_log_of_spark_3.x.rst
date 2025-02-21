:original_name: dli_03_0272.html

.. _dli_03_0272:

Why Is the global_temp Database Missing in the Job Log of Spark 3.x?
====================================================================

Symptom
-------

I cannot find the global_temp database in the Spark 3.\ *x* job log.

Possible Causes
---------------

The global_temp database is the default built-in database of Spark 3.\ *x* and is Spark's global temporary view.

When a Spark job is registered with ViewManager, the system checks whether the database exists in Metastore. If it is, the Spark job fails to be executed.

If the Spark 3.\ *x* job log contains a record of accessing catalogs to query the database and a message indicating that the non-existence of the database is to ensure that Spark jobs can run properly, you do not need to perform any operations.
