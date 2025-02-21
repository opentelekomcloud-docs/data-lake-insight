:original_name: dli_03_0067.html

.. _dli_03_0067:

Why Can't I Query a View After I'm Granted the Select Table Permission on the View?
===================================================================================

Symptom
-------

User A created Table1.

User B created View1 based on Table1.

After the **Select Table** permission on Table1 is granted to user C, user C fails to query View1.

Possible Causes
---------------

User B does not have the **Select Table** permission on Table1.

Solution
--------

Grant the **Select Table** permission on Table1 to user B. Then, query View1 as user C again.
