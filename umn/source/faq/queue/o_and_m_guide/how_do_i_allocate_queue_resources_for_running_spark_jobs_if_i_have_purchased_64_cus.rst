:original_name: dli_03_0088.html

.. _dli_03_0088:

How Do I Allocate Queue Resources for Running Spark Jobs If I Have Purchased 64 CUs?
====================================================================================

In DLI, 64 CU = 64 cores and 256 GB memory.

In a Spark job, if the driver occupies 4 cores and 16 GB memory, the executor can occupy 60 cores and 240 GB memory.
