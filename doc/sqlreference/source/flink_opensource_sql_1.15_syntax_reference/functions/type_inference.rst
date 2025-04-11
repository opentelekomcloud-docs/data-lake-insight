:original_name: dli_08_15083.html

.. _dli_08_15083:

Type Inference
==============

Scenario
--------

Type inference summarizes the logic for validating input arguments and deriving data types for both the parameters and the result of a function. From a logical perspective, the planner needs information about expected types, precision, and scale. From a JVM perspective, the planner needs information about how internal data structures are represented as JVM objects when calling a user-defined function.

Flink's user-defined functions implement an automatic type inference extraction that derives data types from the function's class and its evaluation methods via reflection. However, this implicit reflective extraction approach is not always successful, for example, the Row type commonly used in UDTF cannot be extracted.

Flink 1.11 introduced a UDF registration interface and used a type inference approach, which does not support **getResultType** overload to declare the returned type in Flink 1.10. If you use this approach, the following exception will be thrown:

.. code-block::

   Caused by: org.apache.flink.table.api.ValidationException: Cannot extract a data type from a pure 'org.apache.flink.types.Row' class. Please use annotations to define field names and field types.

With Flink 1.15, the extraction process can be supported by annotating affected parameters, classes, or methods with @DataTypeHint and @FunctionHint.

Code Samples
------------

The table ecosystem (similar to the SQL standard) is a strongly typed API. Therefore, both function parameters and return types must be mapped to a `data type <https://ci.apache.org/projects/flink/flink-docs-release-1.12/dev/table/types.html>`__.

If more advanced type inference logic is required, an implementer can explicitly override the **getTypeInference()** method in every user-defined function.

However, the annotation approach is recommended because it keeps custom type inference logic close to the affected locations and falls back to the default behavior for the remaining implementation.

.. code-block::

   importorg.apache.flink.table.annotation.DataTypeHint;
   importorg.apache.flink.table.annotation.FunctionHint;
   importorg.apache.flink.table.functions.FunctionContext;
   importorg.apache.flink.table.functions.TableFunction;
   importorg.apache.flink.types.Row;
   publicclassUdfTableFunctionextendsTableFunction<Row>{
       /**
        * Initialization, which is optional
        *@paramcontext
        */
       @Override
        public void open(FunctionContextcontext) {  }

       @FunctionHint(output=@DataTypeHint("ROW<s STRING, i INT>"))
       publicvoideval(String str, String split) {
           for (String s: str.split(split)) {
               Row row=new Row(2);
               row.setField(0, s);
               row.setField(1, s.length());
               collect(row);
           }
       }
       /**
       * The following is optional.
       */
      @Override
      public void close() {}
   }

Use Example
-----------

The UDTF supports CROSS JOIN and LEFT JOIN. When the UDTF is used, the **LATERAL** and **TABLE** keywords must be included.

-  CROSS JOIN: does not output the data of a row in the left table if the UDTF does not output the result for the data of the row.
-  LEFT JOIN: outputs the data of a row in the left table even if the UDTF does not output the result for the data of the row, but pads null with UDTF-related fields.

.. code-block::

   CREATE FUNCTION udtf_test AS 'com.company.udf.TableFunction';-- CROSS JOIN
   INSERT INTO sink_stream select subValue, length FROM source_stream, LATERAL
   TABLE(udtf_test(attr, ',')) as T(subValue, length);-- LEFT JOIN
   INSERT INTO sink_stream select subValue, length FROM source_stream LEFT JOIN
   LATERAL
   TABLE(udtf_test(attr, ',')) as T(subValue, length) ON TRUE;
