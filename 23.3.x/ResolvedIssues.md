23.3.0-10
Updated 06/07/2024

Issue Key	Component	Description
VER-93195	Data load / COPY	When the Avro parser would read a byte array that is at most 8 bytes long into a numeric-typed target, it would only accept a single-word numeric as the target type. This has been resolved; now, the Avro parser supports reading short byte arrays into multi-word numeric targets.
VER-93327	Execution Engine	User-Defined Aggregates didn't work with single distinct built-in aggregate in the same query when the input wasn't sorted on grouping columns plus distinct aggregate column. The issue has been resolved.
VER-93447	Backup/DR	LocalStorageLocator did not implement the construct_new() method. When called, it fell back to the StorageLocation.construct_new() method, which raised an error. This issue has been resolved. LocalStorageLocator.construct_new() is now implemented.
VER-93798	Optimizer	In version 23.3.0-9, queries that reused views containing WITH clauses would sometimes fail after several executions of the same query. This issue has been resolved.
VER-93926	Execution Engine	Whether LIKE ANY / ALL read strings as UTF8 character sequences or binary byte arrays depended on whether the collation of the current locale was binary, leading to incorrect results when reading multi-character UTF8 strings in binary-collated locales. This has been resolved. Now, LIKE ANY / ALL always reads UTF8 character sequences, regardless of the current locale's collation.
VER-93935	Client Drivers - ODBC	
The Windows DSN configuration utility no longer sets vertica as the default KerberosServiceName value when editing a DSN.

Starting with version 11.1, providing a value causes the ODBC driver to assume the connection is using Kerberos authentication and communicates to the server that it prefers to use that authentication method, assuming that the user has a grant to a Kerberos authentication method. The KerberosServiceName value might be set in earlier versions of Windows ODBC DSNs. Clearing the value will resolve the issue. This issue only applies to users who have a Kerberos authentication method granted with a lower priority than other authentication methods and use the DSN configuration utility to set up a DSN on Windows.

VER-94034	ComplexTypes, Kafka Integration	Loading JSON/Avro data with Kafka and Flex parsers into tables with many columns suffered from performance degradation. The performance issue has been resolved.
VER-94330	Kafka Integration	The vkconfig --refresh-interval option now functions properly. Setting it to one hour will refresh the lane worker every hour.
VER-94333	Optimizer	NOT LIKE ANY and NOT LIKE ALL are consistent with PostgreSQL now - the phrases LIKE, ILIKE, NOT LIKE, and NOT ILIKE are generally treated as operators in PostgreSQL syntax.
VER-94572	Data load / COPY	In rare cases, copying a JSON to a table using FJsonParser or KafkaJsonParser could cause the server to go down. This issue has been fixed.
VER-94599	FlexTable	The copy of multiple json files to Vertica table using fjsonparser() is successful now, which was causing the initiator node down issue before this fix.
23.3.0-9
Updated 04/11/2024

Issue Key	Component	Description
VER-91668	ResourceManager	If the default resource pool, defined by the DefaultResourcePoolForUsers configuration parameter, was set to a value other than 'general', the user's view incorrectly reported the non-general resource pool as the default pool when the user didn't have that non-general pool set in the profile. This issue has been resolved. The default pool in such cases is now correctly reported as 'general'.
VER-91794	Execution Engine	In rare situations, a logic error in the execution engine "ABuffer" operator would lead to buffer overruns resulting in undefined behavior.  This issue has been fixed.
VER-92114	Catalog Engine	Previously, syslog notifiers could cause the node to go down when attached to certain DC tables. This issue has been resolved.
VER-92125	Optimizer	Queries using the same views repeatedly would sometimes return errors if those views included WITH clauses. The issue has been resolved.
VER-92166	Procedural Languages	Previously, running certain types of queries inside a stored procedure could cause the database to go down. This has been fixed.
VER-92288	Sessions	The ALTER USER statement could not set the idle timeout for a user to the default value, which is defined by the DefaultIdleSessionTimeout configuration parameter. If the empty string was specified, the idle timeout was set to unlimited. This issue has been resolved. You can now set the idle timeout to the DefaultIdleSessionTimeout value by specifying 'default' in the ALTER USER statement.
VER-92660	UI - Management Console	When you upgraded the Management Console from version 12.0.4 to 23.3.0, log in to the Management Console failed and an error message displayed. This issue has been resolved.
VER-92677	Scrutinize	The scrutinize utility produces a tarball of the data it collects. Previously, scrutinize could fail to create this tarball if it encountered a broken symbolic link. This has been fixed, and the size of the tarball is now logged to scrutinize_collection.log.
VER-92749	HTTP	Changing the Vertica server certificate triggers an automatic restart of the built-in HTTPS server. When this happened on a busy system, the nodes could sometimes go down. The issue has been fixed.
VER-92820	Data load / COPY	In COPY, some missing error checks made it so certain invalid input could crash the database. This has been resolved.
23.3.0-8
Updated 02/27/2024

Issue Key	Component	Description
VER-89517	Procedural Languages	Fixed memory leaks that could occur with certain stored procedures.
VER-91235	Backup/DR	On HDFS, vbr tried to delete storage files from the wrong fan-out directory. This has been resolved by providing vbr with the correct fan out directory.
VER-91478	Execution Engine	Since Version 11.1SP1, in some cases, an optimization in the query plan caused queries running under Crunch Scaling mode of COMPUTE_OPTIMIZED to produce wrong results. This issue has been fixed.
VER-91573	Optimizer	The database query debugging configuration parameter "QueryAssertEnabled", when set to 1, could cause replay delete query plans to raise INTERNAL errors, failing to run.  This issue has been resolved.
VER-91715	Optimizer	Queries with identically looking predicates on different tables used in different subqueries where predicates have very different selectivity could result in bad query plans and worse performance due to incorrect estimates on those tables. The issue has been resolved.
VER-91743	Execution Engine	The NULLIF function would infer its output type based on only the first argument. This led to type compatibility errors when the first argument was a small numeric type and the second argument was a much larger numeric type. This has been resolved; now, numeric NULLIF accounts for the types of both arguments when inferring its output type.
VER-91819	Execution Engine	Vertica's execution engine pre-fetches data from disk to reduce wait time during query execution. Memory for the pre-fetch buffers was not reserved with the resource manager, and in some situations a pre-fetch buffer could grow to a large size and bloat the memory footprint of a query until it completed.  Now queries will account for this pre-fetch memory in requests to the resource manager; and several internal changes mitigate the long-term memory footprint of larger-than-average pre-fetch buffers.
VER-92110	DDL - Projection	When we would scan over a projection sorted by two columns (ORDER BY a,b) and materialize only the second one in the sort order (b), we would mistakenly assume the scan is sorted by that column for the purposes of collecting column statistics. This would lead to possible incorrect results when predicate analysis is enabled, and has now been resolved.
23.3.0-7
Updated 01/17/2024

Issue Key	Component	Description
VER-90536	Optimizer	Update statements with subqueries in SET clauses would sometimes return an error. The issue has been resolved.
VER-90857	Optimizer	Create Table As Select statements with repeated occurrences of now() and similar functions were inserting incorrect results into the target table. The issue has been resolved.
VER-91150	Data load / COPY	The upgrade of the C++ AWS SDK in 12.0.2 caused Vertica to make repeated calls to the metadata server for IAM authentication, affecting performance when accessing S3. Vertica now resets the timestamp to prevent excessive pulling.
VER-91190	Optimizer	In version 10.1, Vertica updated its execution engine to sample execution times and selectivity of query predicates and join predicates to run them in the most efficient order.  This has been disruptive to users who wrote queries which depended on a certain evaluation order, in particular that single-table predicates would be evaluated before join conditions.  In particular, queries whose single-table predicates filter out data which would raise a coercion error at the join condition would sometimes raise an error after this change due to the join condition being evaluated first.  Now we have improved this experience by ensuring that join conditions do not raise type coercion errors when they are evaluated before single-table predicates.
23.3.0-6
Updated 11/21/2023

Issue Key	Component	Description
VER-89566	Tuple Mover	When the node with the lowest OID became secondary (for example, during cluster demotion), there might have been an increased number of deadlocks and timeouts due to Data Manipulation Language (DML) statements and internal Tuple Mover tasks. This issue has been resolved.
VER-89632	UI - Management Console	The HTTP Strict-Transport-Security (HSTS) response header was added to all MC responses. This header informs the browser that you should access the site through HTTPS only, and that the browser should automatically convert any HTTP connections to HTTPS.
VER-89771	Scrutinize	The parameter --log-limit determines the maximum size of the vertica log that will be preserved when running scrutinize. The limit is applied to the vertica.log file on all nodes in the cluster. The default value changed from 1GB to unlimited.
VER-89774	Execution Engine	When casting a negative numeric value to an integer and the result of that cast would be 0, then we would incorrectly get an "out of range" error. This has been resolved.
VER-89778	Security	
The following improvements have been made to LDAPLink:

LDAP synchronizations have been optimized and now are much faster for nested groups.
Query profiling now works with LDAP dryrun functions.
VER-89783	Optimizer	In some circumstances, a UNION query that grouped an expression that coerced a value to a common data type returned an error. This issue has been resolved.
VER-89844	Data Collector	If a notifier was set for some DC tables and then subsequently dropped, it still remained present in those DC table policies. This could cause a very large number of messages in vertica.log and potential node crashes. The issue was resolved by making "DROP NOTIFIER" support the CASCADE logic. Without CASCADE, drop would fail for the notifiers still used by DC tables.
VER-89908	Security	Previously, when configuring a chain of certificates longer than a root CA certificate and a client certificate for internode TLS, the configuration would successfully be applied, but cause the cluster to shut down. This has been fixed.
VER-89916	Backup/DR	Backups to S3 object storage and Google Cloud Storage failed and returned a "Temp path" error. This issue has been resolved.
VER-89961	Data load / COPY	Loading JSON arrays into table columns having different case for JSON key and table column used to fail in some cases. The issue has been fixed.
VER-89987	Kafka Integration	When a notifier was set for the NotifierErrors or NotifierStats Data collector (DC) tables, notifications sent with a Kafka notifier might cause a loop that produced an infinite stream of notifications. This might result in severely degradated node performance. This issue has been resolved. Now, notifications are disabled for these DC tables, and any existing notifiers have been removed from these tables.
VER-90065	ComplexTypes, Data load / COPY	A logic gap in the source code could lead to an infinite loop while loading complex arrays with thousands of elements, causing the DML statement to never complete. This issue has been fixed.
VER-90090	Security	In cases of intermittent network connectivity to an LDAP server, Vertica will now retry bind operations.
VER-90106	Catalog Engine	Queries now run correctly when the files of delete vectors are in different storage locations.
23.3.0-5
Updated 10/10/2023

Issue Key	Component	Description
VER-89178	Spread	Previously, if Vertica received unexpected UDP traffic from its client port, the node could go down. This issue has been resolved.
VER-89272	Security	
The following improvements have been made to LDAPLink:

LDAP synchronizations have been optimized and now are much faster for nested groups.
Query profiling now works with LDAP dryrun functions.
VER-89274	Data load / COPY	If a Parquet query or load were to be interrupted (such as by a LIMIT clause, exception during execution, or user cancellation) while the database has configuration parameter "ParquetColumnReaderSize" set to zero, then Vertica could crash.  This issue has been resolved.
VER-89335	Data Collector	In some environments the io_stats system view was empty. The monitoring functionality has been improved with better detection of I/O devices.
VER-89487	EON, Execution Engine	A LIKE ANY or LIKE ALL expression with a non-constant pattern argument on the right-hand side of the expression sometimes resulted in a crash or incorrect internal error. This issue has been resolved. Now, this type of pattern argument results in a normal error.
23.3.0-4
Updated 04/11/2024

Issue Key	Component	Description
VER-88126	EON	The sync_catalog function failed when MinIO communal storage did not meet read-after-write and list-after-write consistency guarantees. A check was added to bypass this restriction. However, if possible, users should make sure that their MinIO storage is configured for read-after-write and list-after-write consistency.
VER-88631	Admin Tools	The Admintools stop_db command failed and returned an error that described active sessions prevented the shutdown. This issue has been resolved, and now the stop_db command stops the database with no errors.
VER-88924	UI - Management Console	When you provisioned a new database on Amazon Web Services, the operation failed. This issue has been resolved.
VER-88955	Optimizer	In some circumstances, queries with outer joins or cross joins that also utilized Top-k projections caused a server error. The issue has been resolved.
VER-89101	Admin Tools	On SUSE Linux Enterprise Server 15, the systemctl status verticad command failed. This issue has been resolved.
23.3.0-3
Updated 09/06/2023

Issue Key	Component	Description
VER-87331	Optimizer	In some cases, using SQL macros that return string types could result in core dumps. The issue has been resolved.
VER-87967	SDK	Previously, you could not compile Vertica UDx builds with GCC compiler version 13 and higher. This issue has been resolved.
VER-87970	Kafka Integration	In some circumstances, there were long timeouts or the process might hang indefinitely when the KafkaAvroParser accessed the Avro Schema Registry. This issue has been resolved.
VER-88495	Backup/DR	Every time Vertica tries to load a snapshot, it checks all the storage files. The file check costs too much time and is not necessary to do it so often. This check is now disabled.
VER-88496	Client Drivers - ODBC	
Previously, the connection property FastCursorClose was set to {{false}} by default, which prevented you from canceling sqlfetch(); you had to set it to true with {{conn.addToConnString("FastCursorClose=1");}} to cancel requests.

FastCursorClose is now set to {{true}} by default.

VER-88546	Optimizer	Queries that contained a WITH query that was referred to more than once and also contained multiple distinct aggregates failed with a system error. This issue has been resolved.
VER-88628	Execution Engine	If there are user-created system tables and IS_SYSTEM_TABLE is set to "true" when you upgrade to 23.3.0, queries on some V_CATALOG system tables fail with an assertion error after you complete the upgrade.
VER-88655	Optimizer	Queries that contained a WITH query that was referred to more than once, and also contained joins on tables with segmented projections and SELECT DISTINCT or LIMIT subqueries sometimes produced an incorrect result. This issue has been resolved.
23.3.0-2
Updated 08/29/2023

Issue Key	Component	Description
VER-87253	ComplexTypes, Execution Engine	The optimization that makes it so that EXPLODE on complex types only materializes fields that are needed in the query was not applied to the similar UNNEST function. This has been resolved, and now UNNEST similarly prunes out unused fields from scans/loads.
VER-87775	Admin Tools, Data Collector	If you revived a database and the EnableDataCollector parameter was set to 1, you could not start the database after it was revived. This issue was resolved. To start the database, disable the cluster lease check.
VER-87799	Optimizer	During the planning stage, updates on tables with thousands of columns using thousands of SET USING clauses took a long time. Planning performance for these updates was improved.
VER-87820	Execution Engine	When casting a numeric to an integer, the bounds of acceptable values were based on the NUMERIC(18, 0) type, rather than the INTEGER type. This meant that numbers that were 19 digits, but could still fit in a 64-bit integer, would incorrectly error out. Casting a numeric to an integer now checks according to the proper bounds for the INTEGER type.
VER-87876	Catalog Engine	
Previously, when a cluster lost quorum and switched to read-only mode or stopped, some transaction commits in the queue might get processed. However, due to the loss of quorum, these commits might not have been persisted. These "transient transactions" were reported as successful, but they were lost when the cluster restarted.

Now, when Vertica detects a transient transaction, it issues a WARNING so you can diagnose the problem, and it creates an event in ACTIVE_EVENTS that describes what happened.

VER-87962	Tuple Mover	The Tuple Mover logged a large number of PURGE requests on a projection while another MERGEOUT job was running on the same projection. This issue has been resolved.
VER-87965	Optimizer	Queries with outer joins over subqueries with WHERE clauses that contain AND expressions with constant terms sometimes returned an error. This issue has been resolved.
VER-87975	Optimizer	When creating a UDx side process, Vertica required that the current time zone have a name. This caused a crash when a UDx side process was created under a time zone with a GMT offset rather than a name. This issue has been resolved.
VER-88005	Execution Engine	Queries with large tables stopped the database because the indices that Vertica uses to navigate the tables consumed too much RAM. This issue has been resolved, and now the indices use less RAM.
VER-88115	Client Drivers - ODBC	Previously, the ODBC driver could return 64-bit FLOATs with incorrect values in its last bit, which are not IEEE-compliant. This has been fixed.
VER-88205	Optimizer	In some query plans with segmentation across multiple nodes, Vertica would get an internal optimizer error when trying to prune out unused data edges from the plan. This issue has been resolved.
VER-88227	EON	In rare circumstances, the automatic sync of catalog files to the communal storage stopped working on some nodes. Users could still manually sync with sync_catalog(). The issue has been resolved.
VER-88281	Performance tests	In some cases, the NVL2 function caused Vertica to crash when it returned an array type. This issue has been resolved.
VER-88337	Procedural Languages	When a Stored Procedure executed a subquery that included constraints, it returned an incorrect value. This issue has been resolved.
VER-88341	Backup/DR	Backup and restore operations failed on FIPS-enabled systems. This issue has been resolved.
23.3.0-1
Issue Key	Component	Description
VER‑87918	UI - Management Console	When you provisioned a database from the Management Console, there was a connection issue that prevented the Management Console from communicating with the new database. This issue has been resolved.
23.3.0-0
Updated 07/18/2023

Issue Key	Component	Description
VER-82827	ResourceManager	
Previously, user-defined global resource pools could have the same name as subcluster resource pools. This is no longer supported. However, different subclusters can still have resource pools with the same name.

When you create a new subcluster resource pool, Vertica checks whether there is any global pool with the same external name, and vice versa. For example:

=> SELECT name, subcluster_name FROM RESOURCE_POOLS WHERE name = 'test_pool';

   name    |   subcluster_name

--------{-}{{-}}{{-}}+{{-}}{{-}}{-}-------------------

 test_pool | secondary_subcluster

(1 row)

=> CREATE RESOURCE POOL test_pool  maxmemorysize '2G';

ROLLBACK 4593:  Resource pool "test_pool" already exists

VER-83661	Data Export, S3	Export to Parquet sometimes logged errors in a DC table for successful exports. This issue has been resolved.
VER-83998	Admin Tools, Security	Paramiko has been upgraded to 2.10.1 to address CVE-2022-24302.
VER-84276	Execution Engine	Predicate reordering optimization moved a comparison against a constant ahead of SIP filters, but the SIP filter needed to be evaluated after the constant predicate. This issue has been resolved: now, predicates are not reordered when a stateful SIP filter needs to be evaluated in a particular order.
VER-84493	Optimizer	Queries eligible for TOPK projections that were also eligible for elimination of no-op joins would sometimes exit with internal error. The issue has been resolved.
VER-84894	Client Drivers – OLEDB	Previously, OLEDB connections could timeout, causing SSAS tools to crash. This issue has been resolved.
VER-85008	Hadoop	A logic error in pushing predicates down to prune Parquet row groups and ORC stripes would sometimes result in false positives, pruning data which actually should have passed the predicate.  This issue is known to occur when using IN or NOT IN expressions and applying an expression to transform the ORC or Parquet column on the left-hand side of the expression.  This issue has been resolved.
VER-85153	AP-Geospatial	If you nested multiple geospatial functions when reading from a Parquet file, there was an issue finding usable memory that made the database crash. This issue has been resolved.
VER-85187	Execution Engine	When pushing down predicates of a query that involved a WITH clause being turned into a shared temp relation, an IS NULL predicate on the preserved side of a left outer join was pushed below the join. As a result, rows that should have been filtered out were erroneously included in the result set. This issue has been resolved by updating the predicate pushdown logic.
VER-85260	Execution Engine	LIKE operators that were qualified by ANY and ALL did not correctly evaluate multiple string constant arguments. This issue has been resolved.
VER-85311	FlexTable	In some cases COMPUTE_FLEXTABLE_KEYS used to assign non-string data types to keys where string data type was more suitable. The algorithm was improved to prefer string types in those cases.
VER-85626	Execution Engine	Expressions resembling expr = ANY(string_to_array(list_of_string_constants)) had a logic error that resulted in undefined behavior.  This issue has been resolved.
VER-86015	Admin Tools	The default logrotate configuration uses "dateext" with the default date format. Using the default date format limits log rotation to no more than once per day. For more frequent rotations, administrators can edit their logrotate configuration files to use the "dateformat" string. For details, see the linux man page on logrotate.
VER-86019	Kubernetes	If you used VerticaDB operator v1.10.0 and you ran a sidecar container with your VerticaDB custom resource, the operator sometimes failed to restart the Vertica process. This issue was resolved.
VER-86060	Recovery	When you applied a swap partition event to one table, the other table involved in the same swap partition event was removed from the dirty transactions list. This issue has been resolved. Now, both tables involved in the same swap partition event are in the dirty transaction list.
VER-86098	FlexTable	FCSVParser used to load empty strings for values that matched with COPY NULL parameter instead of loading NULL. This issue happened only on pure flex tables. The issue has been resolved.
VER-86132	Client Drivers - VSQL	Previously, Vertica would load an incorrect number of files if you attempted to load more than 65,535 files with COPY FROM LOCAL. This has been fixed; COPY FROM LOCAL now properly loads up to 4,294,967,295 files.
VER-86198	Execution Engine	When a database had many storage locations, query and other operations such as analyze_statistics() were sometimes slow. This issue has been resolved.
VER-86223	ComplexTypes	The flex JSON and Avro parsers did not always correctly handle excessively large ARRAY[VARCHAR] inputs. In certain cases this would lead to undefined behavior resulting in a crash.  This issue has been resolved.
VER-86438	Machine Learning	Loading certain types of data sometimes led to an empty string being non-empty, which produced a variety of errors. This issue has been resolved.
VER-86442	ComplexTypes	In some circumstances, queries that had valid scalar data types were returning a VIAssert error. This issue has been resolved.
VER-86494	Client Drivers – Python, Sessions	Loading certain data could sometimes cause an empty string to not actually be empty, which could lead to a variety of errors. This issue has been resolved.
VER-86500	Execution Engine	In some circumstances, the database crashed with errors when you upgraded from Vertica version 11.1.1 and higher to Vertica version 12.0.4. This issue has been resolved.
VER-86507	Catalog Engine	Truncating a local temporary table unnecessarily required a global catalog lock, as temporary tables are session scoped. This issue has been resolved.
VER-86578	DDL	In versions 12.0.2 and 12.0.3, the QUERY_REQUESTS system table displayed incorrectly when a query was executing. This issue has been resolved.
VER-86692	Optimizer	Merge queries with an INTO...USING clause that calls a subquery would sometimes return an error when merging into a table with Set Using/Default query columns. The issue has been resolved.
VER-86701	Catalog Engine, Security	When upgrading a database, any user or role with the same name as a predefined role is renamed.
VER-86708	Optimizer	In some circumstances, queries that had valid scalar data types were returning a VIAssert error. This issue has been resolved.
VER-86709	Execution Engine	In some contexts, equivalent numeric types were considered incompatible with each other, which resulted in errors. This issue has been resolved.
VER-86724	Catalog Engine, Performance tests	In version 12.0.0, querying system tables could be slower than in previous versions. Version 12.0.4-8 adjusts the system table segmentation to improve system table queries.
VER-86734	Kafka Integration, Security	Previously, using a Kafka Notifier with SASL_SSL or SASL_PLAINTEXT would incorrectly use SSL instead. This issue has been resolved.
VER-86804	Security	Previously, adapter_parameters values in the NOTIFIER system table would be truncated if they exceeded 128 characters. This limit has been increased to 8196 characters.
VER-86833	Execution Engine	When evaluating check constraints on tables with multiple projections with different sort orders, Vertica would sometimes read the data from the table incorrectly. This issue has been resolved.
VER-86864	EON	Under certain circumstances, depending on the frequency and length of the depot fetching activity, a file could not be re-fetched after its eviction—either automatic or cleared manually—unless the node was restarted.  This issue has been resolved.
VER-86901	Backup/DR	When each node in a cluster pointed to a different backup location, the backup location was non-deterministic, and there were inconsistent failures. This issue has been resolved.
VER-86984	Optimizer	If a user-defined SQL function that returns a string was nested within a call to TRIM which was nested within a call to NULLIF (for example: "NULLIF(TRIM(user_function(value),' '))"), Vertica could return an invalid result or the error "ERROR: ICU locale operation error: 'U_BUFFER_OVERFLOW_ERROR'". This issue has been resolved.
VER-86993	ComplexTypes	Previously, the flex table and Kafka parsers could crash if they tried to load array data that is too large for the target table.  This behavior was fixed but introduced a change where those array values would cause the whole row to be rejected instead of setting the array value to NULL.  Now, the default behavior is to set the data cell to NULL if the array value is too large. This can be overridden with the "reject_on_materialized_type_error" parameter, which will have the rows be rejected instead.
VER-87003	Execution Engine	Because the explode function is a 1:N transform function, using  ORDER BY in its OVER clause has an undefined effect.  Previously, using an ORDER BY clause in the OVER clause of Explode could result in an INTERNAL error if the configuration parameter TryPruneUnusedDataEdges was set to 1.  This issue has been resolved.
VER-87431	ComplexTypes	The flex and kafka parsers would erroneously not respect the parameter "reject_on_materialized_type_error" in cases where an array was too large for the target column, and no element was rejected.  Previously, such values would always be rejected. This has been corrected, and now if "reject_on_materialized_type_error" is false, those values will be set to NULL instead.
VER-87537	Documentation, Installation Program	Some characters did not render correctly when specific commands were copied and pasted from the documentation. This issue has been resolved.
VER-87803	ComplexTypes, Execution Engine	When rewriting a CROSS JOIN UNNEST query into an equivalent query that puts the UNNEST in a subquery, requesting scalar columns from a table with larger complex columns could lead to an INTERNAL error. This has been resolved.
VER-87856	DevOps	Fixed RPM digests by installing a newer version of the RPM on our build container when building RPMs.
