:original_name: dli_08_15037.html

.. _dli_08_15037:

Overview
========

GaussDB(DWS) is an online data processing database based on the cloud infrastructure and platform and helps you mine and analyze massive sets of data. DLI reads data of Flink jobs from GaussDB(DWS). GaussDB(DWS) database kernel is compliant with PostgreSQL. The PostgreSQL database can store data of more complex types and delivers space information services, multi-version concurrent control (MVCC), and high concurrency. It applies to location applications, financial insurance, and e-commerce.

DLI Flink 1.15 now offers two GaussDB(DWS) connector options for accessing GaussDB data:

-  **GaussDB(DWS)'s self-developed GaussDB(DWS) connector (recommended)**: This option focuses on the performance of and direct interaction with GaussDB(DWS), allowing users to easily and flexibly read and write data.

   You can use GaussDB(DWS)'s self-developed GaussDB(DWS) connector by creating UDFs. For details about how to create a UDF, see :ref:`UDFs <dli_08_15082>`.

-  **DLI's GaussDB(DWS) connector (discarded and not recommended)**: This option allows users to customize sink and source functions to meet specific data read and write needs.

   For how to use DLI's GaussDB(DWS) connector, see :ref:`Table 1 <dli_08_15037__table3954102713514>`.

   .. _dli_08_15037__table3954102713514:

   .. table:: **Table 1** Supported GaussDB(DWS) connector types

      +-----------------+----------------------------------------------------------------------+
      | Type            | Instruction                                                          |
      +=================+======================================================================+
      | Source table    | :ref:`GaussDB(DWS) Source Table (Not Recommended) <dli_08_15104>`    |
      +-----------------+----------------------------------------------------------------------+
      | Result table    | :ref:`GaussDB(DWS) Result Table (Not Recommended) <dli_08_15105>`    |
      +-----------------+----------------------------------------------------------------------+
      | Dimension table | :ref:`GaussDB(DWS) Dimension Table (Not Recommended) <dli_08_15106>` |
      +-----------------+----------------------------------------------------------------------+
