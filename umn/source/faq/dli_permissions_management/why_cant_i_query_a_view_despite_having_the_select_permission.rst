:original_name: dli_03_0067.html

.. _dli_03_0067:

Why Can't I Query a View Despite Having the Select Permission?
==============================================================

Symptom
-------

User A created a table named **Table1**.

User B created a view named **View1** based on **Table1**.

After granting user C the select permission on **Table1**, user C failed to query the view.

Current Permission Assignments
------------------------------

-  User A already has: **admin** permission on **Table1**.
-  User B already has: **admin** permission on **View1**.
-  User C already has: **select** permission on **Table1**.

Solution
--------

Different versions of the Spark engine have varying permission requirements for views:

-  **Spark 2.4.**\ *x*: User C needs the view query permission, and user B requires the select permission on **Table1**.

   Permission requirements:

   -  User B should have: admin permission on **View1** and select permission on **Table1** (currently missing).
   -  User C already has: select permission on **Table1**.

   To resolve this, grant user B the select permission on **Table1**, then retry querying **View1** as user C.

-  **Spark 3.3.**\ *x*: User C needs the view query permission, and user C requires the select permission on **Table1**.

   Permission requirements:

   -  User C should have: select permission on **Table1** and select permission on **View1** (currently missing).

   To resolve this, grant user C the select permission on **View1**, then retry querying **View1** as user C.
