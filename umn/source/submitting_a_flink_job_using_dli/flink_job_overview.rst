:original_name: dli_01_0403.html

.. _dli_01_0403:

Flink Job Overview
==================

DLI supports two types of Flink jobs:

-  **Flink OpenSource SQL job:**

   -  It is fully compatible with Flink of the community edition, ensuring that jobs can run smoothly on these Flink versions.
   -  DLI Flink has expanded the support for connectors based on Flink of the community edition, supporting Redis and GaussDB(DWS) as new data source types. With this expansion, you can now utilize a wider range of data source types, providing greater flexibility and convenience when working with datasets.
   -  Flink OpenSource SQL jobs are ideal for scenarios where stream processing logic can be defined and executed through SQL statements. This simplifies stream processing, allowing developers to focus more on implementing service logic.

   For how to create a Flink OpenSource SQL job, see :ref:`Creating a Flink OpenSource SQL Job <dli_01_0498>`.

-  **Flink Jar job:**

   -  DLI allows you to submit Flink jobs compiled into JAR files, providing higher flexibility and customization capabilities. It is applicable to scenarios where complex data processing is required.
   -  If the connectors provided by Flink of the community edition cannot meet specific needs, you can use Jar jobs to implement custom connectors or data processing logic.
   -  It is ideal for scenarios where user-defined functions (UDFs) or specific library integration are required. You can use the Flink ecosystem to implement advanced stream processing logic and status management.

   For how to create a Flink Jar job, see :ref:`Creating a Flink Jar Job <dli_01_0457>`.
