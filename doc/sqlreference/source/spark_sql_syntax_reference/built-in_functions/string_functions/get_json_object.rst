:original_name: dli_spark_get_json_object.html

.. _dli_spark_get_json_object:

get_json_object
===============

This function is used to parse the JSON object in a specified JSON path. The function will return **NULL** if the JSON object is invalid.

Syntax
------

.. code-block::

   get_json_object(string <json>, string <path>)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                        |
   +=================+=================+=================+====================================================================================================+
   | json            | Yes             | STRING          | Standard JSON object, in the **{Key:Value, Key:Value,...}** format.                                |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | path            | Yes             | STRING          | Path of the object in JSON format, which starts with $. The meanings of characters are as follows: |
   |                 |                 |                 |                                                                                                    |
   |                 |                 |                 | -  **$** indicates the root node.                                                                  |
   |                 |                 |                 | -  **.** indicates a subnode.                                                                      |
   |                 |                 |                 | -  **[]** indicates the index of an array, which starts from 0.                                    |
   |                 |                 |                 | -  **\*** indicates the wildcard for []. The entire array is returned. \* does not support escape. |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **json** is empty or in invalid JSON format, **NULL** is returned.
   -  If the value of **json** is valid and **path** is specified, the corresponding string is returned.

Example Code
------------

-  Extracts information from the JSON object **src_json.json**. An example command is as follows:

   .. code-block::

      jsonString = {"store": {"fruit":[{"weight":8,"type":"apple"},{"weight":9,"type":"pear"}], "bicycle":{"price":19.95,"color":"red"} }, "email":"amy@only_for_json_udf_test.net", "owner":"Tony" }

   Extracts the information of the **owner** field and returns **Tony**.

   .. code-block::

      select get_json_object(jsonString, '$.owner');

   Extracts the first array information of the **store.fruit** field and returns **{"weight":8,"type":"apple"}**.

   .. code-block::

      select get_json_object(jsonString, '$.store.fruit[0]');

   Extracts information about a field that does not exist and returns **NULL**.

   .. code-block::

      select get_json_object(jsonString, '$.non_exist_key');

-  Extracts information about an array JSON object. An example command is as follows:

   The value **22** is returned.

   .. code-block::

      select get_json_object('{"array":[["a",11],["b",22],["c",33]]}','$.array[1][1]');

   The value **["h00","h11","h22"]** is returned.

   .. code-block::

      select get_json_object('{"a":"b","c":{"d":"e","f":"g","h":["h00","h11","h22"]},"i":"j"}','$.c.h[*]');

   The value **["h00","h11","h22"]** is returned.

   .. code-block::

      select get_json_object('{"a":"b","c":{"d":"e","f":"g","h":["h00","h11","h22"]},"i":"j"}','$.c.h');

   The value **h11** is returned.

   .. code-block::

      select get_json_object('{"a":"b","c":{"d":"e","f":"g","h":["h00","h11","h22"]},"i":"j"}','$.c.h[1]');

-  Extracts information from a JSON object with a period (.). An example command is as follows:

   Create a table.

   .. code-block::

      create table json_table (id string, json string);

   Insert data into the table. The key contains a period (.).

   .. code-block::

      insert into table json_table (id, json) values ("1", "{\"city1\":{\"region\":{\"rid\":6}}}");

   Insert data into the table. The key does not contain a period (.).

   .. code-block::

      insert into table json_table (id, json) values ("2", "{\"city1\":{\"region\":{\"rid\":7}}}");

   Obtain the value of **rid**. If the key is **city1**, **6** is returned. Only [''] can be used for parsing because a period (.) is included.

   .. code-block::

      select get_json_object(json, "$['city1'].region['id']") from json_table where id =1;

   Obtain the value of **rid**. If the key is **city1**, **7** is returned. You can use either of the following methods:

   .. code-block::

      select get_json_object(json, "$['city1'].region['id']") from json_table where id =2;
      select get_json_object(json, "$.city1.region['id']") from json_table where id =2;

-  The **json** parameter is either empty or has an invalid format. An example command is as follows:

   The value **NULL** is returned.

   .. code-block::

      select get_json_object('','$.array[2]');

   The value **NULL** is returned.

   .. code-block::

      select get_json_object('"array":["a",1],"b":["c",3]','$.array[1][1]');

-  A JSON string involves escape. An example command is as follows:

   The value **3** is returned.

   .. code-block::

      select get_json_object('{"a":"\\"3\\"","b":"6"}', '$.a');

   The value **3** is returned.

   .. code-block::

      select get_json_object('{"a":"\'3\'","b":"6"}', '$.a');

-  A JSON object can contain the same key and can be parsed successfully.

   The value **1** is returned.

   .. code-block::

      select get_json_object('{"b":"1","b":"2"}', '$.b');

-  The result is output in the original sorting mode of the JSON string.

   The value **{"b":"3","a":"4"}** is returned.

   .. code-block::

      select get_json_object('{"b":"3","a":"4"}', '$');
