---
layout: post
title: Explore Dynamic Management Views (DMV)
date: 2019-11-16 00:13
author: techforumugm
comments: true
categories: [MS SQLServer]
---
<h2>What is DMV ?</h2>
“<b>DMVs</b>” are query structures built into <b>SQL Server</b> that deliver details about <b>server</b> and database health/performance. <b>DMVs</b> provide a common mechanism to extract “all things <b>SQL</b>” as well as Windows OS performance data
<ul>
	<li>Dynamic management views and functions return internal, implementation-specific state data.</li>
	<li>Their schema and the data they return may change in future releases of SQL Server.</li>
	<li>Dynamic management views and functions in future releases may not be compatible with the dynamic management views and functions in this release. For example, in future releases of SQL Server, Microsoft may augment the definition of any dynamic management view by adding columns to the end of the column list.</li>
	<li>Recommend against using the syntax SELECT * FROM dynamic_management_view_name in production code because the number of columns returned might change and break your application.</li>
</ul>
<h2></h2>
<img class="alignnone size-full wp-image-1034" src="https://techforumugm.files.wordpress.com/2019/11/dmv02.png" alt="DMV02" width="709" height="659" />

Microsoft has introduce DMV's from SQL Server 2012 to SQL Server 2014,  with the syntax “<strong>dm_xtp_</strong>xxxxxxxx” or “<strong>dm_db_xtp_</strong>xxxxxxxx”.All definitions for these views come from the Microsoft documentation or web site.
<ul>
	<li><span style="color:var(--color-text);">sys.dm_server_memory_dumps</span>
<ul>
	<li>The dump type may be a minidump, all-thread dump, or a full dump. The files have an extension of .mdmp.</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_exec_compute_node_status</strong>
<ul>
	<li>Give information about resources of PolyBase nodes like memory, cpu, time,</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_exec_compute_nodes</strong>
<ul>
	<li>Returns the list of type, logical name and IP adress of PolyBase nodes</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_exec_distributed_request_steps</strong>
<ul>
	<li>Give all steps that compose a PolyBase request</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_exec_distributed_requests</strong>
<ul>
	<li>Give the current status of actives queries\</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_exec_distributed_sql_requests</strong>
<ul>
	<li>This view shows the data for the last 1000 requests</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_exec_dms_services</strong>
<ul>
	<li>Give the status of the DMS (Data Movement Service) Service</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_exec_dms_workers</strong>
<ul>
	<li>Show all workers completing DMS steps for the last 1000 queries and active queries</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_exec_external_operations</strong>
<ul>
	<li>returns information of external PolyBase operations</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_exec_external_work</strong>
<ul>
	<li>gives information for the workload per node</li>
</ul>
</li>
</ul>
A useful msdn page resumes all DMVs for these new views <a title="Polybase DMVs" href="https://msdn.microsoft.com/en-us/library/mt146389.aspx" target="_blank" rel="noopener">here</a>

Other dm_exec_xxx views are basically usefull like:
<ul>
	<li><strong>dm_exec_function_stats</strong>
<ul>
	<li>Returns aggregate performance statistics for cached functions.</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_exec_query_optimizer_memory_gateways</strong>
<ul>
	<li>Returns the current status of resource semaphores used to throttle concurrent query optimization.</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_exec_query_parallel_workers</strong>
<ul>
	<li>Returns worker availability information per node</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_exec_session_wait_stats</strong>
<ul>
	<li>Returns information about all the waits encountered by threads that executed for each session</li>
</ul>
</li>
</ul>
<strong>3</strong> new DMVs for the Columstore technology:
<ul>
	<li><strong>dm_column_store_object_pool</strong>
<ul>
	<li>Returns counts of different types of object memory pool usage for columnstore index objects</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_db_column_store_row_group_operational_stats</strong>
<ul>
	<li>Returns current row-level I/O, locking, and access method activity for compressed rowgroups in a columnstore index.</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_db_column_store_row_group_physical_stats</strong>
<ul>
	<li>Provides current rowgroup-level information about all of the columnstore indexes in the current database</li>
</ul>
</li>
</ul>
<strong>2</strong> new DMVs for Stretch Databases in the database context and with rda(remote database archive):
<ul>
	<li><strong>dm_db_rda_migration_status</strong>
<ul>
	<li>To list the migration batch of the table</li>
</ul>
</li>
</ul>
<ul>
	<li><strong>dm_db_rda_migration_status</strong>
<ul>
	<li>For the current database, list of state information of the remote data archive schema update task.</li>
</ul>
</li>
</ul>
