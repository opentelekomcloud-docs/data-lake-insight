:original_name: dli_03_0092.html

.. _dli_03_0092:

How Do I Specify an OBS Path When Creating an OBS Table?
========================================================

Scenario
--------

When creating an OBS table, you must specify a table path in the database. The path format is as follows: obs://xxx/database name/table name.

Correct Example
---------------

.. code-block::

   CREATE TABLE `di_seller_task_activity_30d` (`user_id` STRING COMMENT' user ID...) SORTED as parquet
   LOCATION 'obs://akc-bigdata/akdc.db/di_seller_task_activity_30d'

Incorrect Example
-----------------

.. code-block::

   CREATE TABLE `di_seller_task_activity_30d` (`user_id` STRING COMMENT' user ID...) SORTED as parquet
   LOCATION 'obs://akc-bigdata/akdc.db'

.. note::

   If the specified path is **akdc.db**, data in this path will be cleared when the **insert overwrite** statement is executed.
