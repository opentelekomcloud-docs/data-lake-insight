:original_name: dli_03_0086.html

.. _dli_03_0086:

How Do I Merge Small Files?
===========================

If a large number of small files are generated during SQL execution, job execution and table query will take a long time. In this case, you should merge small files.

You are advised to use temporary tables for data transfer. There is a risk of data loss in self-read and self-write operations during unexpected exceptional scenarios.

Run the following SQL statements:

.. code-block::

   INSERT OVERWRITE TABLE tablename
   select  * FROM  tablename
   DISTRIBUTE BY floor(rand()*20)
