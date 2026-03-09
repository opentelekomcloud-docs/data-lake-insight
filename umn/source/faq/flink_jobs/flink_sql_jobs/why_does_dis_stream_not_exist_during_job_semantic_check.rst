:original_name: dli_03_0040.html

.. _dli_03_0040:

Why Does DIS Stream Not Exist During Job Semantic Check?
========================================================

To rectify this fault, perform the following steps:

#. Log in to the DIS management console. In the navigation pane, choose **Stream Management**. View the Flink job SQL statements to check whether the DIS stream exists.

#. If the DIS stream in the Flink job has not been created yet, create one by referring to "Creating a DIS Stream" in *Data Lake Insight User Guide*.

   Ensure that the created DIS stream and Flink job are in the same region.

#. If a DIS stream has been created, check whether the DIS stream and the Flink job are in the same region.
