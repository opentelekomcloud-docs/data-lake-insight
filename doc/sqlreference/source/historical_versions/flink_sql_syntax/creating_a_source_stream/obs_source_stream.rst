:original_name: dli_08_0236.html

.. _dli_08_0236:

OBS Source Stream
=================

Function
--------

Create a source stream to obtain data from OBS. DLI reads data stored by users in OBS as input data for jobs. OBS applies to various scenarios, such as big data analysis, cloud-native application program data, static website hosting, backup/active archive, and deep/cold archive.

OBS is an object-based storage service. It provides massive, secure, highly reliable, and low-cost data storage capabilities. For more information about OBS, see the *Object Storage Service Console Operation Guide*.

Syntax
------

::

   CREATE SOURCE STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "obs",
       region = "",
       bucket = "",
       object_name = "",
       row_delimiter = "\n",
       field_delimiter = '',
       version_id = ""
     );

Keyword
-------

.. table:: **Table 1** Keyword description

   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                       |
   +=======================+=======================+===================================================================================================================================================================================================================================================+
   | type                  | Yes                   | Data source type. **obs** indicates that the data source is OBS.                                                                                                                                                                                  |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region                | Yes                   | Region to which OBS belongs.                                                                                                                                                                                                                      |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode                | No                    | Data encoding format. The value can be **csv** or **json**. The default value is **csv**.                                                                                                                                                         |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ak                    | No                    | Access Key ID (AK).                                                                                                                                                                                                                               |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sk                    | No                    | Secret access key used together with the ID of the access key.                                                                                                                                                                                    |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | bucket                | Yes                   | Name of the OBS bucket where data is located.                                                                                                                                                                                                     |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | object_name           | Yes                   | Name of the object stored in the OBS bucket where data is located. If the object is not in the OBS root directory, you need to specify the folder name, for example, **test/test.csv**. For the object file format, see the **encode** parameter. |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | row_delimiter         | Yes                   | Separator used to separate every two rows.                                                                                                                                                                                                        |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | field_delimiter       | No                    | Separator used to separate every two attributes.                                                                                                                                                                                                  |
   |                       |                       |                                                                                                                                                                                                                                                   |
   |                       |                       | -  This parameter is mandatory when **encode** is **csv**. You use custom attribute separators.                                                                                                                                                   |
   |                       |                       | -  If **encode** is **json**, you do not need to set this parameter.                                                                                                                                                                              |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | quote                 | No                    | Quoted symbol in a data format. The attribute delimiters between two quoted symbols are treated as common characters.                                                                                                                             |
   |                       |                       |                                                                                                                                                                                                                                                   |
   |                       |                       | -  If double quotation marks are used as the quoted symbol, set this parameter to **\\u005c\\u0022** for character conversion.                                                                                                                    |
   |                       |                       | -  If a single quotation mark is used as the quoted symbol, set this parameter to a single quotation mark (').                                                                                                                                    |
   |                       |                       |                                                                                                                                                                                                                                                   |
   |                       |                       | .. note::                                                                                                                                                                                                                                         |
   |                       |                       |                                                                                                                                                                                                                                                   |
   |                       |                       |    -  Currently, only the CSV format is supported.                                                                                                                                                                                                |
   |                       |                       |    -  After this parameter is specified, ensure that each field does not contain quoted symbols or contains an even number of quoted symbols. Otherwise, parsing will fail.                                                                       |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | version_id            | No                    | Version number. This parameter is optional and required only when the OBS bucket or object has version settings.                                                                                                                                  |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

When creating a source stream, you can specify a time model for subsequent calculation. Currently, DLI supports two time models: Processing Time and Event Time. For details about the syntax, see :ref:`Configuring Time Models <dli_08_0107>`.

Example
-------

-  The **input.csv** file is read from the OBS bucket. Rows are separated by **'\\n'** and columns are separated by **','**.

   To use the test data, create an **input.txt** file, copy and paste the following text data, and save the file as **input.csv**. Upload the **input.csv** file to the target OBS bucket directory. For example, upload the file to the **dli-test-obs01** bucket directory.

   .. code-block::

      1,2,3,4,1403149534
      5,6,7,8,1403149535

   The following is an example for creating the table:

   ::

      CREATE SOURCE STREAM car_infos (
        car_id STRING,
        car_owner STRING,
        car_brand STRING,
        car_price INT,
        car_timestamp LONG
      )
        WITH (
          type = "obs",
          bucket = "dli-test-obs01",
          region = "xxx",
          object_name = "input.csv",
          row_delimiter = "\n",
          field_delimiter = ","
      );

-  The **input.json** file is read from the OBS bucket. Rows are separated by **'\\n'**.

   .. code-block::

      CREATE SOURCE STREAM obs_source (
        str STRING
      )
        WITH (
          type = "obs",
          bucket = "obssource",
          region = "xxx",
          encode = "json",
          row_delimiter = "\n",
          object_name = "input.json"
      );
