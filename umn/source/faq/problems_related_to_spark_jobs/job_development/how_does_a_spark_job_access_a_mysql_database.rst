:original_name: dli_03_0127.html

.. _dli_03_0127:

How Does a Spark Job Access a MySQL Database?
=============================================

You can use DLI Spark jobs to access data in the MySQL database using either of the following methods:

-  Solution 1: Purchase a pay-per-use queue, create an enhanced datasource connection, and read data from the MySQL database through a datasource table. You need to write Java or Scala code to implement this solution.
-  Solution 2: Use CDM to import data from the MySQL database to an OBS bucket, and then use a Spark job to read data from the OBS bucket. If you already have a CDM cluster, this solution is simpler than solution 1 and does not involve any other database.
