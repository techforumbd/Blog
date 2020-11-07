---
layout: post
title: Effective Usage of TempDB
date: 2019-12-07 00:44
author: techforumugm
comments: true
categories: [MS SQLServer]
---
TempDB is a system database in Microsoft SQL Server used as a store of internal objects, row versions, work tables, temporary tables, and indexes. TempDB is available for use to all participants connected to a SQL Server instance (it is a global resource). TempDB is shared across all databases and all connections in SQL Server, it might become a point of contention if not configured correctly. This article will cover a few important performance-related facts about TempDB.

TempDB is typically used in the following cases:

1.Temporary tables are created with the # naming convention

2.In addition, there may be some conflicts if several concurrent sessions are creating such TempTables simultaneously.

3.Indexes are built or rebuilt with the SORT_IN_TEMPDB=ON option. It tends to remove the burden of sorting from the database which owns the index while the rebuilding process is ongoing.

4.TempDB to create work tables which are commonly used in cursor operations – calls by the GROUP BY, ORDER BY, or UNION clauses.

<img class="alignnone size-full wp-image-1042" src="https://techforumugm.files.wordpress.com/2019/08/td02.png" alt="TD02" width="257" height="245" />
<h2><b>Capacity Planning</b></h2>
Determining the appropriate size for TempDB in a SQL Server production environment depends on many factors. As described previously in this article, these factors include the existing workload and the SQL Server features that are used. We recommend that you analyze the existing workload by performing the following tasks in a SQL Server test environment:
<ul>
	<li>Set autogrow on for TempDB.</li>
	<li>Execute individual queries or workload trace files and monitor TempDB space use.</li>
	<li>Execute index maintenance operations, such as rebuilding indexes and monitor TempDB space.</li>
	<li>Use the space-use values from the previous steps to predict your total workload usage; adjust this value for projected concurrent activity, and then set the size of TempDB accordingly.</li>
</ul>
<h2>Monitor Disk Space</h2>
use the sys.dm_db_file_space_usage dynamic management view to monitor the disk space that is used in the TempDB files:

<b>-- Determining the Amount of Free Space in </b><b>TempDB</b>

SELECT SUM(unallocated_extent_page_count) AS [free pages],

(SUM(unallocated_extent_page_count)*1.0/128) AS [free space in MB]

FROM sys.dm_db_file_space_usage;

-<b>- Determining the Amount Space Used by the Version Store</b>

SELECT SUM(version_store_reserved_page_count) AS [version store pages used],

(SUM(version_store_reserved_page_count)*1.0/128) AS [version store space in MB]

FROM sys.dm_db_file_space_usage;

<b>-- Determining the Amount of Space Used by Internal Objects</b>

SELECT SUM(internal_object_reserved_page_count) AS [internal object pages used],

(SUM(internal_object_reserved_page_count)*1.0/128) AS [internal object space in MB]

FROM sys.dm_db_file_space_usage;

<b>-- Determining the Amount of Space Used by User Objects</b>

SELECT SUM(user_object_reserved_page_count) AS [user object pages used],

(SUM(user_object_reserved_page_count)*1.0/128) AS [user object space in MB]

FROM sys.dm_db_file_space_usage;

To monitor the page allocation or deallocation activity in TempDB at the session or task level, you can use the sys.dm_db_session_space_usage and sys.dm_db_task_space_usage dynamic management views. These views can be used to identify large queries, temporary tables, or table variables that are using lots of TempDB disk space.

<b>-- Obtaining the space consumed by internal objects in all currently running tasks in each session</b>

SELECT session_id,  SUM(internal_objects_alloc_page_count) AS task_internal_objects_alloc_page_count,

SUM(internal_objects_dealloc_page_count) AS task_internal_objects_dealloc_page_count

FROM sys.dm_db_task_space_usage

GROUP BY session_id;

<b>-- Obtaining the space consumed by internal objects in the current session for both running and completed tasks</b>

SELECT R2.session_id,  R1.internal_objects_alloc_page_count

+ SUM(R2.internal_objects_alloc_page_count) AS session_internal_objects_alloc_page_count,

R1.internal_objects_dealloc_page_count

+ SUM(R2.internal_objects_dealloc_page_count) AS session_internal_objects_dealloc_page_count

FROM sys.dm_db_session_space_usage AS R1

INNER JOIN sys.dm_db_task_space_usage AS R2 ON R1.session_id = R2.session_id

GROUP BY R2.session_id, R1.internal_objects_alloc_page_count, R1.internal_objects_dealloc_page_count;
<h2>Performance Enhance</h2>
Running out of disk space in TempDB can cause significant disruptions in the SQL Server production environment and can prevent applications that are running from completing operations.Briefly explored that we can monitor TempDB activity using five key DMVs exposed by SQL Server. When using this level of the monitoring process in a production environment, we can determine whether we need more space in TempDB and data files. The following relevant dynamic management views (DMVs) are helpful when investigating the activity in TempDB:

1.sys.dm_db_file_space_usage: This DMV returns some information about the space usage of files in the databases you are interested in. It can be used to examine any database in the instance and the output pertains only to that database. In the context of this article, we shall be using the DMV to examine TempDB.

2.sys.dm_db_session_space_usage: This DMV is exclusive to the TempDB database and returns the number of pages allocated and deallocated by each session for a given database. The page allocations are typically maintained till the session is terminated.

3.sys.dm_db_task_space_usage: This DMV is also exclusive to the TempDB database and provides some information about the number of pages allocated and deallocated by each task for a given database.

4.sys.dm_tran_active_snapshot_database_transactions: This DMV returns the active transactions that generate and may access row versions. This view is relevant when options such as ALLOW_SNAPSHOT_ISOLATION and READ_COMMITTED_SNAPSHOT are enabled.

5.sys.dm_tran_version_store: This DMV provides some information about all version records in the version store. In an active production server, the records in this table could grow significantly. Thus, we need to be careful when querying the DMV.
<h2>Observation of TempDB</h2>
Briefly explored that we can monitor TempDB activity using five key DMVs exposed by SQL Server. When using this level of the monitoring process in a production environment, we can determine whether we need more space in TempDB and data files.

The following relevant dynamic management views (DMVs) are helpful when investigating the activity in TempDB:

1.sys.dm_db_file_space_usage: This DMV returns some information about the space usage of files in the databases you are interested in. It can be used to examine any database in the instance and the output pertains only to that database. In the context of this article, we shall be using the DMV to examine TempDB.

2.sys.dm_db_session_space_usage: This DMV is exclusive to the TempDB database and returns the number of pages allocated and deallocated by each session for a given database. The page allocations are typically maintained till the session is terminated.

3.sys.dm_db_task_space_usage: This DMV is also exclusive to the TempDB database and provides some information about the number of pages allocated and deallocated by each task for a given database.

4.sys.dm_tran_active_snapshot_database_transactions: This DMV returns the active transactions that generate and may access row versions. This view is relevant when options such as ALLOW_SNAPSHOT_ISOLATION and READ_COMMITTED_SNAPSHOT are enabled.

5.sys.dm_tran_version_store: This DMV provides some information about all version records in the version store. In an active production server, the records in this table could grow significantly. Thus, we need to be careful when querying the DMV.
<h2><b>Retention policy of temporal Table  </b><b>  </b></h2>
Let’s work on this example. Create a database and create a temporal table with history retention clause. We are only using 2 days for history retention here because this is a test scenario. You can also define months or years as the retention unit.

CREATE TABLE TestTemporal(

Id INT CONSTRAINT PK_ID PRIMARY KEY,

CustomerName VARCHAR(50),

StartDate DATETIME2 GENERATED ALWAYS AS ROW START NOT NULL,

EndDate DATETIME2 GENERATED ALWAYS AS ROW END NOT NULL,

PERIOD FOR SYSTEM_TIME (StartDate, EndDate)

)

WITH (SYSTEM_VERSIONING = ON

(HISTORY_TABLE = dbo.TestTemporalHistory,

History_retention_period = 2 DAYS

)

)

GO

History retention information can be retrieved from sys.tables system table.

SELECT name, temporal_type_desc, history_retention_period, history_retention_period_unit

FROM sys.tables

GO;
