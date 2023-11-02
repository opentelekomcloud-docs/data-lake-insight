:original_name: dli_08_0241.html

.. _dli_08_0241:

DIS Sink Stream
===============

Function
--------

DLI writes the Flink job output data into DIS. This cloud ecosystem is applicable to scenarios where data is filtered and imported to the DIS stream for future processing.

DIS addresses the challenge of transmitting data outside cloud services to cloud services. DIS builds data intake streams for custom applications capable of processing or analyzing streaming data. DIS continuously captures, transmits, and stores terabytes of data from hundreds of thousands of sources every hour, such as logs, Internet of Things (IoT) data, social media feeds, website clickstreams, and location-tracking events. For more information about DIS, see the *Data Ingestion Service User Guide*.

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "dis",
       region = "",
       channel = "",
       partition_key = "",
       encode= "",
       field_delimiter= ""
     );

Keyword
-------

.. table:: **Table 1** Keyword description

   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter              | Mandatory             | Description                                                                                                                                                                                                                           |
   +========================+=======================+=======================================================================================================================================================================================================================================+
   | type                   | Yes                   | Output channel type. **dis** indicates that data is exported to DIS.                                                                                                                                                                  |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region                 | Yes                   | Region where DIS for storing the data is located.                                                                                                                                                                                     |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ak                     | No                    | Access Key ID (AK).                                                                                                                                                                                                                   |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sk                     | No                    | Specifies the secret access key used together with the ID of the access key.                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | channel                | Yes                   | DIS stream.                                                                                                                                                                                                                           |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | partition_key          | No                    | Group primary key. Multiple primary keys are separated by commas (,). If this parameter is not specified, data is randomly written to DIS partitions.                                                                                 |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode                 | Yes                   | Data encoding format. The value can be **csv**, **json**, or **user_defined**.                                                                                                                                                        |
   |                        |                       |                                                                                                                                                                                                                                       |
   |                        |                       | .. note::                                                                                                                                                                                                                             |
   |                        |                       |                                                                                                                                                                                                                                       |
   |                        |                       |    -  **field_delimiter** must be specified if this parameter is set to **csv**.                                                                                                                                                      |
   |                        |                       |    -  If the encoding format is **json**, you need to configure **enable_output_null** to determine whether to generate an empty field. For details, see the examples.                                                                |
   |                        |                       |    -  **encode_class_name** and **encode_class_parameter** must be specified if this parameter is set to **user_defined**.                                                                                                            |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | field_delimiter        | Yes                   | Separator used to separate every two attributes.                                                                                                                                                                                      |
   |                        |                       |                                                                                                                                                                                                                                       |
   |                        |                       | -  This parameter needs to be configured if the CSV encoding format is adopted. It can be user-defined, for example, a comma (**,**).                                                                                                 |
   |                        |                       | -  This parameter is not required if the JSON encoding format is adopted.                                                                                                                                                             |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json_config            | No                    | If **encode** is set to **json**, you can set this parameter to specify the mapping between the JSON field and the stream definition field. An example of the format is as follows: field1=data_json.field1; field2=data_json.field2. |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | enable_output_null     | No                    | If **encode** is set to **json**, you need to specify this parameter to determine whether to generate an empty field.                                                                                                                 |
   |                        |                       |                                                                                                                                                                                                                                       |
   |                        |                       | If this parameter is set to **true**, an empty field (the value is **null**) is generated. If set to **false**, no empty field is generated. The default value is **true**.                                                           |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode_class_name      | No                    | If **encode** is set to **user_defined**, you need to set this parameter to the name of the user-defined decoding class (including the complete package path). The class must inherit the **DeserializationSchema** class.            |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode_class_parameter | No                    | If **encode** is set to **user_defined**, you can set this parameter to specify the input parameter of the user-defined decoding class. Only one parameter of the string type is supported.                                           |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

None

Example
-------

-  CSV: Data is written to the DIS stream and encoded using CSV. CSV fields are separated by commas (,). If there are multiple partitions, car_owner is used as the key to distribute data to different partitions. An example is as follows: "ZJA710XC", "lilei", "BMW", 700000.

   ::

      CREATE SINK STREAM audi_cheaper_than_30w (
        car_id STRING,
        car_owner STRING,
        car_brand STRING,
        car_price INT
      )
        WITH (
          type = "dis",
          region = "xxx",
          channel = "dlioutput",
          encode = "csv",
          field_delimiter = ","
      );

-  JSON: Data is written to the DIS stream and encoded using JSON. If there are multiple partitions, car_owner and car_brand are used as the keys to distribute data to different partitions. If **enableOutputNull** is set to **true**, an empty field (the value is **null**) is generated. If set to **false**, no empty field is generated. An example is as follows: "car_id ":"ZJA710XC", "car_owner ":"lilei", "car_brand ":"BMW", "car_price ":700000.

   ::

      CREATE SINK STREAM audi_cheaper_than_30w (
        car_id STRING,
        car_owner STRING,
        car_brand STRING,
        car_price INT
      )
        WITH (
          type = "dis",
          channel = "dlioutput",
          region = "xxx",
          partition_key = "car_owner,car_brand",
          encode = "json",
          enable_output_null = "false"
      );
