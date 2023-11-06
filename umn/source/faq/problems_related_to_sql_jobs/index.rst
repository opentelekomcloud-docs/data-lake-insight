:original_name: dli_03_0020.html

.. _dli_03_0020:

Problems Related to SQL Jobs
============================

-  :ref:`SQL Jobs <dli_03_0200>`
-  :ref:`How Do I Merge Small Files? <dli_03_0086>`
-  :ref:`How Do I Specify an OBS Path When Creating an OBS Table? <dli_03_0092>`
-  :ref:`How Do I Create a Table Using JSON Data in an OBS Bucket? <dli_03_0108>`
-  :ref:`How Do I Set Local Variables in SQL Statements? <dli_03_0087>`
-  :ref:`How Can I Use the count Function to Perform Aggregation? <dli_03_0069>`
-  :ref:`How Do I Synchronize DLI Table Data from One Region to Another? <dli_03_0072>`
-  :ref:`How Do I Insert Table Data into Specific Fields of a Table Using a SQL Job? <dli_03_0191>`
-  :ref:`Why Is Error "path obs://xxx already exists" Reported When Data Is Exported to OBS? <dli_03_0014>`
-  :ref:`Why Is Error "SQL_ANALYSIS_ERROR: Reference 't.id' is ambiguous, could be: t.id, t.id.;" Displayed When Two Tables Are Joined? <dli_03_0066>`
-  :ref:`Why Is Error "The current account does not have permission to perform this operation,the current account was restricted. Restricted for no budget." Reported when a SQL Statement Is Executed? <dli_03_0071>`
-  :ref:`Why Is Error "There should be at least one partition pruning predicate on partitioned table XX.YYY" Reported When a Query Statement Is Executed? <dli_03_0145>`
-  :ref:`Why Is Error "IllegalArgumentException: Buffer size too small. size" Reported When Data Is Loaded to an OBS Foreign Table? <dli_03_0169>`
-  :ref:`Why Is Error "DLI.0002 FileNotFoundException" Reported During SQL Job Running? <dli_03_0189>`
-  :ref:`Why Is a Schema Parsing Error Reported When I Create a Hive Table Using CTAS? <dli_03_0046>`
-  :ref:`Why Is Error "org.apache.hadoop.fs.obs.OBSIOException" Reported When I Run DLI SQL Scripts on DataArts Studio? <dli_03_0173>`
-  :ref:`Why Is Error "UQUERY_CONNECTOR_0001:Invoke DLI service api failed" Reported in the Job Log When I Use CDM to Migrate Data to DLI? <dli_03_0172>`
-  :ref:`Why Is Error "File not Found" Reported When I Access a SQL Job? <dli_03_0207>`
-  :ref:`Why Is Error "DLI.0003: AccessControlException XXX" Reported When I Access a SQL Job? <dli_03_0208>`
-  :ref:`Why Is Error "DLI.0001: org.apache.hadoop.security.AccessControlException: verifyBucketExists on {{bucket name}}: status [403]" Reported When I Access a SQL Job? <dli_03_0209>`
-  :ref:`Why Is Error "The current account does not have permission to perform this operation,the current account was restricted. Restricted for no budget" Reported During SQL Statement Execution? Restricted for no budget. <dli_03_0210>`
-  :ref:`How Do I Troubleshoot Slow SQL Jobs? <dli_03_0196>`
-  :ref:`How Do I View DLI SQL Logs? <dli_03_0091>`
-  :ref:`How Do I View SQL Execution Records? <dli_03_0116>`
-  :ref:`How Do I Eliminate Data Skew by Configuring AE Parameters? <dli_03_0093>`
-  :ref:`What Can I Do If a Table Cannot Be Queried on the DLI Console? <dli_03_0184>`
-  :ref:`The Compression Ratio of OBS Tables Is Too High <dli_03_0013>`
-  :ref:`How Can I Avoid Garbled Characters Caused by Inconsistent Character Codes? <dli_03_0009>`
-  :ref:`Do I Need to Grant Table Permissions to a User and Project After I Delete a Table and Create One with the Same Name? <dli_03_0175>`
-  :ref:`Why Can't I Query Table Data After Data Is Imported to a DLI Partitioned Table Because the File to Be Imported Does Not Contain Data in the Partitioning Column? <dli_03_0177>`
-  :ref:`How Do I Fix the Data Error Caused by CRLF Characters in a Field of the OBS File Used to Create an External OBS Table? <dli_03_0181>`
-  :ref:`Why Does a SQL Job That Has Join Operations Stay in the Running State? <dli_03_0182>`
-  :ref:`The on Clause Is Not Added When Tables Are Joined. Cartesian Product Query Causes High Resource Usage of the Queue, and the Job Fails to Be Executed <dli_03_0187>`
-  :ref:`Why Can't I Query Data After I Manually Add Data to the Partition Directory of an OBS Table? <dli_03_0190>`
-  :ref:`Why Is All Data Overwritten When insert overwrite Is Used to Overwrite Partitioned Table? <dli_03_0212>`
-  :ref:`Why Is a SQL Job Stuck in the Submitting State? <dli_03_0213>`
-  :ref:`Why Is the create_date Field in the RDS Table Is a Timestamp in the DLI query result? <dli_03_0214>`
-  :ref:`What Can I Do If datasize Cannot Be Changed After the Table Name Is Changed in a Finished SQL Job? <dli_03_0215>`
-  :ref:`Why Is the Data Volume Changes When Data Is Imported from DLI to OBS? <dli_03_0231>`

.. toctree::
   :maxdepth: 1
   :hidden: 

   sql_jobs
   how_do_i_merge_small_files
   how_do_i_specify_an_obs_path_when_creating_an_obs_table
   how_do_i_create_a_table_using_json_data_in_an_obs_bucket
   how_do_i_set_local_variables_in_sql_statements
   how_can_i_use_the_count_function_to_perform_aggregation
   how_do_i_synchronize_dli_table_data_from_one_region_to_another
   how_do_i_insert_table_data_into_specific_fields_of_a_table_using_a_sql_job
   why_is_error_path_obs_xxx_already_exists_reported_when_data_is_exported_to_obs
   why_is_error_sql_analysis_error_reference_t.id_is_ambiguous_could_be_t.id_t.id.;_displayed_when_two_tables_are_joined
   why_is_error_the_current_account_does_not_have_permission_to_perform_this_operationthe_current_account_was_restricted._restricted_for_no_budget._reported_when_a_sql_statement_is_executed
   why_is_error_there_should_be_at_least_one_partition_pruning_predicate_on_partitioned_table_xx.yyy_reported_when_a_query_statement_is_executed
   why_is_error_illegalargumentexception_buffer_size_too_small._size_reported_when_data_is_loaded_to_an_obs_foreign_table
   why_is_error_dli.0002_filenotfoundexception_reported_during_sql_job_running
   why_is_a_schema_parsing_error_reported_when_i_create_a_hive_table_using_ctas
   why_is_error_org.apache.hadoop.fs.obs.obsioexception_reported_when_i_run_dli_sql_scripts_on_dataarts_studio
   why_is_error_uquery_connector_0001invoke_dli_service_api_failed_reported_in_the_job_log_when_i_use_cdm_to_migrate_data_to_dli
   why_is_error_file_not_found_reported_when_i_access_a_sql_job
   why_is_error_dli.0003_accesscontrolexception_xxx_reported_when_i_access_a_sql_job
   why_is_error_dli.0001_org.apache.hadoop.security.accesscontrolexception_verifybucketexists_on_{{bucket_name}}_status_[403]_reported_when_i_access_a_sql_job
   why_is_error_the_current_account_does_not_have_permission_to_perform_this_operationthe_current_account_was_restricted._restricted_for_no_budget_reported_during_sql_statement_execution_restricted_for_no_budget.
   how_do_i_troubleshoot_slow_sql_jobs
   how_do_i_view_dli_sql_logs
   how_do_i_view_sql_execution_records
   how_do_i_eliminate_data_skew_by_configuring_ae_parameters
   what_can_i_do_if_a_table_cannot_be_queried_on_the_dli_console
   the_compression_ratio_of_obs_tables_is_too_high
   how_can_i_avoid_garbled_characters_caused_by_inconsistent_character_codes
   do_i_need_to_grant_table_permissions_to_a_user_and_project_after_i_delete_a_table_and_create_one_with_the_same_name
   why_cant_i_query_table_data_after_data_is_imported_to_a_dli_partitioned_table_because_the_file_to_be_imported_does_not_contain_data_in_the_partitioning_column
   how_do_i_fix_the_data_error_caused_by_crlf_characters_in_a_field_of_the_obs_file_used_to_create_an_external_obs_table
   why_does_a_sql_job_that_has_join_operations_stay_in_the_running_state
   the_on_clause_is_not_added_when_tables_are_joined._cartesian_product_query_causes_high_resource_usage_of_the_queue_and_the_job_fails_to_be_executed
   why_cant_i_query_data_after_i_manually_add_data_to_the_partition_directory_of_an_obs_table
   why_is_all_data_overwritten_when_insert_overwrite_is_used_to_overwrite_partitioned_table
   why_is_a_sql_job_stuck_in_the_submitting_state
   why_is_the_create_date_field_in_the_rds_table_is_a_timestamp_in_the_dli_query_result
   what_can_i_do_if_datasize_cannot_be_changed_after_the_table_name_is_changed_in_a_finished_sql_job
   why_is_the_data_volume_changes_when_data_is_imported_from_dli_to_obs
