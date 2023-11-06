:original_name: dli_03_0086.html

.. _dli_03_0086:

How Do I Merge Small Files?
===========================

If a large number of small files are generated during SQL execution, job execution and table query will take a long time. In this case, you should merge small files.

#. Set the configuration item as follows:

   spark.sql.shuffle.partitions = Number of partitions (number of the generated small files in this case)

#. Execute the following SQL statements:

   .. code-block::

      INSERT OVERWRITE TABLE tablename
      select  * FROM  tablename distribute by rand()
