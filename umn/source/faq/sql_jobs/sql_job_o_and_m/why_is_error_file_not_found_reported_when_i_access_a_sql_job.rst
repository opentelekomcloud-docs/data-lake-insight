:original_name: dli_03_0207.html

.. _dli_03_0207:

Why Is Error "File not Found" Reported When I Access a SQL Job?
===============================================================

Symptom
-------

Error message "File not Found" is displayed when a SQL job is accessed.

Possible Causes
---------------

-  The system may not be able to locate the specified file path or file due to an incorrect file path or the file not existing.
-  The file is in use.

Solution
--------

-  Check the file path and file name.

   Check whether the file path is correct, including the directory name and file name.

-  Check if any jobs are overwriting the corresponding data.

   If the file cannot be found due to it being in use, it is usually caused by a read-write conflict. You are advised to check if any jobs are overwriting the corresponding data when querying the SQL error table.
