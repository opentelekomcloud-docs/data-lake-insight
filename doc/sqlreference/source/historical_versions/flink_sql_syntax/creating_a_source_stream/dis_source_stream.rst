:original_name: dli_08_0235.html

.. _dli_08_0235:

DIS Source Stream
=================

Function
--------

Create a source stream to read data from DIS. DIS accesses user data and Flink job reads data from the DIS stream as input data for jobs. Flink jobs can quickly remove data from producers using DIS source sources for continuous processing. Flink jobs are applicable to scenarios where data outside the cloud service is imported to the cloud service for filtering, real-time analysis, monitoring reports, and dumping.

DIS addresses the challenge of transmitting data outside cloud services to cloud services. DIS builds data intake streams for custom applications capable of processing or analyzing streaming data. DIS continuously captures, transmits, and stores terabytes of data from hundreds of thousands of sources every hour, such as logs, Internet of Things (IoT) data, social media feeds, website clickstreams, and location-tracking events. For more information about DIS, see the *Data Ingestion Service User Guide*.

Syntax
------

.. code-block::

   CREATE SOURCE STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "dis",
       region = "",
       channel = "",
       partition_count = "",
       encode = "",
       field_delimiter = "",
       offset= "");

Keywords
--------

.. table:: **Table 1** Keywords

   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter              | Mandatory             | Description                                                                                                                                                                                                                                                                                                                                                               |
   +========================+=======================+===========================================================================================================================================================================================================================================================================================================================================================================+
   | type                   | Yes                   | Data source type. **dis** indicates that the data source is DIS.                                                                                                                                                                                                                                                                                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region                 | Yes                   | Region where DIS for storing the data is located.                                                                                                                                                                                                                                                                                                                         |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ak                     | No                    | Access Key ID (AK).                                                                                                                                                                                                                                                                                                                                                       |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sk                     | No                    | Specifies the secret access key used together with the ID of the access key.                                                                                                                                                                                                                                                                                              |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | channel                | Yes                   | Name of the DIS stream where data is located.                                                                                                                                                                                                                                                                                                                             |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | partition_count        | No                    | Number of partitions of the DIS stream where data is located. This parameter and **partition_range** cannot be configured at the same time. If this parameter is not specified, data of all partitions is read by default.                                                                                                                                                |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | partition_range        | No                    | Range of partitions of a DIS stream, data in which is ingested by the DLI job. This parameter and **partition_count** cannot be configured at the same time. If this parameter is not specified, data of all partitions is read by default.                                                                                                                               |
   |                        |                       |                                                                                                                                                                                                                                                                                                                                                                           |
   |                        |                       | If you set this parameter to **[0:2]**, data will be read from partitions 1, 2, and 3.                                                                                                                                                                                                                                                                                    |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode                 | Yes                   | Data encoding format. The value can be **csv**, **json**, **xml**, **email**, **blob**, or **user_defined**.                                                                                                                                                                                                                                                              |
   |                        |                       |                                                                                                                                                                                                                                                                                                                                                                           |
   |                        |                       | -  **field_delimiter** must be specified if this parameter is set to **csv**.                                                                                                                                                                                                                                                                                             |
   |                        |                       | -  **json_config** must be specified if this parameter is set to **json**.                                                                                                                                                                                                                                                                                                |
   |                        |                       | -  **xml_config** must be specified if this parameter is set to **xml**.                                                                                                                                                                                                                                                                                                  |
   |                        |                       | -  **email_key** must be specified if this parameter is set to **email**.                                                                                                                                                                                                                                                                                                 |
   |                        |                       | -  If this parameter is set to **blob**, the received data is not parsed, only one stream attribute exists, and the data format is ARRAY[TINYINT].                                                                                                                                                                                                                        |
   |                        |                       | -  **encode_class_name** and **encode_class_parameter** must be specified if this parameter is set to **user_defined**.                                                                                                                                                                                                                                                   |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | field_delimiter        | No                    | Attribute delimiter. This parameter is mandatory only when the CSV encoding format is used. You can set this parameter, for example, to a comma (,).                                                                                                                                                                                                                      |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | quote                  | No                    | Quoted symbol in a data format. The attribute delimiters between two quoted symbols are treated as common characters.                                                                                                                                                                                                                                                     |
   |                        |                       |                                                                                                                                                                                                                                                                                                                                                                           |
   |                        |                       | -  If double quotation marks are used as the quoted symbol, set this parameter to **\\u005c\\u0022** for character conversion.                                                                                                                                                                                                                                            |
   |                        |                       | -  If a single quotation mark is used as the quoted symbol, set this parameter to a single quotation mark (').                                                                                                                                                                                                                                                            |
   |                        |                       |                                                                                                                                                                                                                                                                                                                                                                           |
   |                        |                       | .. note::                                                                                                                                                                                                                                                                                                                                                                 |
   |                        |                       |                                                                                                                                                                                                                                                                                                                                                                           |
   |                        |                       |    -  Currently, only the CSV format is supported.                                                                                                                                                                                                                                                                                                                        |
   |                        |                       |    -  After this parameter is specified, ensure that each field does not contain quoted symbols or contains an even number of quoted symbols. Otherwise, parsing will fail.                                                                                                                                                                                               |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json_config            | No                    | When the encoding format is JSON, you need to use this parameter to specify the mapping between JSON fields and stream definition fields. The format is **field1=data_json.field1; field2=data_json.field2; field3=$**, where **field3=$** indicates that the content of field3 is the entire JSON string.                                                                |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | xml_config             | No                    | If **encode** is set to **xml**, you need to set this parameter to specify the mapping between the xml field and the stream definition field. An example of the format is as follows: field1=data_xml.field1; field2=data_xml.field2.                                                                                                                                     |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | email_key              | No                    | If **encode** is set to **email**, you need to set the parameter to specify the information to be extracted. You need to list the key values that correspond to stream definition fields. Multiple key values are separated by commas (,), for example, "Message-ID, Date, Subject, body". There is no keyword in the email body and DLI specifies "body" as the keyword. |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode_class_name      | No                    | If **encode** is set to **user_defined**, you need to set this parameter to the name of the user-defined decoding class (including the complete package path). The class must inherit the **DeserializationSchema** class.                                                                                                                                                |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode_class_parameter | No                    | If **encode** is set to **user_defined**, you can set this parameter to specify the input parameter of the user-defined decoding class. Only one parameter of the string type is supported.                                                                                                                                                                               |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | offset                 | No                    | -  If data is imported to the DIS stream after the job is started, this parameter will become invalid.                                                                                                                                                                                                                                                                    |
   |                        |                       |                                                                                                                                                                                                                                                                                                                                                                           |
   |                        |                       | -  If the job is started after data is imported to the DIS stream, you can set the parameter as required.                                                                                                                                                                                                                                                                 |
   |                        |                       |                                                                                                                                                                                                                                                                                                                                                                           |
   |                        |                       |    For example, if **offset** is set to **100**, DLI starts from the 100th data record in DIS.                                                                                                                                                                                                                                                                            |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | start_time             | No                    | Start time for reading DIS data.                                                                                                                                                                                                                                                                                                                                          |
   |                        |                       |                                                                                                                                                                                                                                                                                                                                                                           |
   |                        |                       | -  If this parameter is specified, DLI reads data read from the specified time. The format is **yyyy-MM-dd HH:mm:ss**.                                                                                                                                                                                                                                                    |
   |                        |                       | -  If neither **start_time** nor **offset** is specified, DLI reads the latest data.                                                                                                                                                                                                                                                                                      |
   |                        |                       | -  If **start_time** is not specified but **offset** is specified, DLI reads data from the data record specified by **offset**.                                                                                                                                                                                                                                           |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | enable_checkpoint      | No                    | Whether to enable the checkpoint function. The value can be **true** (enabled) or **false** (disabled). The default value is **false**.                                                                                                                                                                                                                                   |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | checkpoint_app_name    | No                    | ID of a DIS consumer. If a DIS stream is consumed by different jobs, you need to configure the consumer ID for each job to avoid checkpoint confusion.                                                                                                                                                                                                                    |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | checkpoint_interval    | No                    | Interval of checkpoint operations on the DIS source operator. The value is in the unit of seconds. The default value is **60**.                                                                                                                                                                                                                                           |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

When creating a source stream, you can specify a time model for subsequent calculation. Currently, DLI supports two time models: Processing Time and Event Time. For details about the syntax, see :ref:`Configuring Time Models <dli_08_0107>`.

Example
-------

-  In CSV encoding format, DLI reads data from the DIS stream and records it as codes in CSV format. The codes are separated by commas (,).

   ::

      CREATE SOURCE STREAM car_infos (
        car_id STRING,
        car_owner STRING,
        car_age INT,
        average_speed INT,
        total_miles INT,
        car_timestamp LONG
      )
        WITH (
          type = "dis",
          region = "xxx",
          channel = "dliinput",
          encode = "csv",
          field_delimiter = ","
      );

-  In JSON encoding format, DLI reads data from the DIS stream and records it as codes in JSON format. For example, {"car":{"car_id":"ZJA710XC", "car_owner":"coco", "car_age":5, "average_speed":80, "total_miles":15000, "car_timestamp":1526438880}}

   ::

      CREATE SOURCE STREAM car_infos (
        car_id STRING,
        car_owner STRING,
        car_age INT,
        average_speed INT,
        total_miles INT,
        car_timestamp LONG
      )
        WITH (
          type = "dis",
          region = "xxx",
          channel = "dliinput",
          encode = "json",
          json_config = "car_id=car.car_id;car_owner =car.car_owner;car_age=car.car_age;average_speed =car.average_speed ;total_miles=car.total_miles;"
      );

-  In XML encoding format, DLI reads data from the DIS stream and records it as codes in XML format.

   ::

      CREATE SOURCE STREAM person_infos (
          pid BIGINT,
          pname STRING,
          page int,
          plocation STRING,
          pbir DATE,
          phealthy BOOLEAN,
          pgrade ARRAY[STRING]
      )
        WITH (
          type = "dis",
          region = "xxx",
          channel = "dis-dli-input",
          encode = "xml",
          field_delimiter = ",",
          xml_config = "pid=person.pid;page=person.page;pname=person.pname;plocation=person.plocation;pbir=person.pbir;pgrade=person.pgrade;phealthy=person.phealthy"
      );

   An example of XML data is as follows:

   ::

      <?xml version="1.0" encoding="utf-8"?>

      <root>
        <person>
          <pid>362305199010025042</pid>
          <pname>xiaoming</pname>
          <page>28</page>
          <plocation>xxx</plocation>
          <pbir>1990-10-02</pbir>
          <phealthy>true</phealthy>
          <pgrade>[A,B,C]</pgrade>
        </person>
      </root>

-  In EMAIL encoding format, DLI reads data from the DIS stream and records it as a complete Email.

   ::

      CREATE SOURCE STREAM email_infos (
        Event_ID String,
        Event_Time Date,
        Subject String,
        From_Email String,
        To_EMAIL String,
        CC_EMAIL Array[String],
        BCC_EMAIL String,
        MessageBody String,
        Mime_Version String,
        Content_Type String,
        charset String,
        Content_Transfer_Encoding String
      )
        WITH (
          type = "dis",
          region = "xxx",
          channel = "dliinput",
          encode = "email",
          email_key = "Message-ID, Date, Subject, From, To, CC, BCC, Body, Mime-Version, Content-Type, charset, Content_Transfer_Encoding"
      );

   An example of email data is as follows:

   .. code-block::

      Message-ID: <200906291839032504254@sample.com>
      Date: Fri, 11 May 2001 09:54:00 -0700 (PDT)
      From: user1@sample.com
      To: user2@sample.com, user3@sample.com
      Subject:  "Hello World"
      Cc: user4@sample.com, user5@sample.com
      Mime-Version: 1.0
      Content-Type: text/plain; charset=us-ascii
      Content-Transfer-Encoding: 7bit
      Bcc: user6@sample.com, user7@sample.com
      X-From: user1
      X-To: user2, user3
      X-cc: user4, user5
      X-bcc:
      X-Folder: \user2_June2001\Notes Folders\Notes inbox
      X-Origin: user8
      X-FileName: sample.nsf

      Dear Associate / Analyst Committee:

      Hello World!

      Thank you,

      Associate / Analyst Program
      user1
