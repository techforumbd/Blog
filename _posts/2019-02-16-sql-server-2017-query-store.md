---
layout: post
title: SQL Server 2017 : Query Store
date: 2019-02-16 00:59
author: techforumugm
comments: true
categories: [MS SQLServer]
---
<h2>What is Query Store?</h2>
Query store has introduced for performance troubleshooting process. Queries having multiple plans: Store the history of query plans in the system
<ul>
	<li>Execution Plans and runtime statistics</li>
	<li>Stores the insight information of each queries i.e. Resource usage, database usage</li>
	<li>Identify queries that have “gotten slower recently”</li>
	<li>Identify un-efficient plans and force a better plan</li>
</ul>
Make sure this works across server restarts, upgrades, and query recompiled

<img class="alignnone size-full wp-image-998" src="https://techforumugm.files.wordpress.com/2019/01/qs.png" alt="QS" width="409" height="219" />
<h2>Why Query Store ?</h2>
<ul>
	<li>Explore the queries execution</li>
	<li>Resource consuming queries</li>
	<li>Figure out query plan</li>
	<li>Identify possible performance degradation queries</li>
	<li>Figure out why regressions happen</li>
	<li>Force the query processor to use a particular plan</li>
	<li>Query Store is accessible through Transact-SQL.</li>
	<li>Identify and improve ad-hoc workloads</li>
	<li>Pinpoint and fix queries</li>
	<li>Custom reporting and/or alerting through</li>
	<li>Dynamic Management Views (DMVs)</li>
</ul>
<h2><b>How Setup Query Store</b></h2>
Enabled Query Store to capture query execution plans and runtime statistics :

<b>SQL Server Management Studio</b>
<ul>
	<li>Object Explorer and Right click on selected database</li>
	<li>GO to Properties option</li>
	<li>select the Query Store page</li>
	<li>Set Operation Mode =<u>Read Write</u> &amp; Click OK</li>
	<li>Refresh and expand the database</li>
	<li>SQL Server Query Store folder will appear with the list of available built-in reports</li>
</ul>
&nbsp;

<img class="alignnone size-full wp-image-1003" src="https://techforumugm.files.wordpress.com/2019/01/qs2.png" alt="QS2" width="503" height="474" />
<h2><b>How Setup Query Store </b></h2>
<b>Enable Query Store :</b>

ALTER DATABASE [AdventureWorks2017] SET QUERY_STORE = ON;

GO

In the Data Flush Interval (Minutes) option, shows how frequent the query runtime statistics and query execution plans will be flushed from memory of SQL Server instance to disk.

ALTER DATABASE AdventureWorks2017

SET QUERY_STORE = ON

(   DATA_FLUSH_INTERVAL_SECONDS = 900 );

<b>Statistics Collection Interval option:</b>

Statistics Collection Interval option defined aggregation interval of query runtime statistics.

<b>Max Size (MB) option :</b>

is for configuring the maximum size of the SQL Server Query Store. The data in the SQL Server Query Store is stored in the database where the SQL Server Query Store is enabled. The SQL Server Query Store doesn’t auto grow and once the SQL Server Query Store

<img class="alignnone size-full wp-image-1007" src="https://techforumugm.files.wordpress.com/2019/02/qs3.png" alt="QS3" width="332" height="560" />
<h2><b>Query Store Setup Options</b></h2>
<ul>
	<li>Off – The SQL Server Query Store turned off</li>
	<li>Read Only – This mode indicates that new query runtime statistics or executed plans will not be tracked (collected)</li>
	<li>Read Write – Allows capturing query executed plans and query runtime statistics</li>
	<li>Data Flush Interval (Minutes) option, an interval in minutes can be set which shows how frequent the query</li>
</ul>
runtime statistics and query execution plans will be flushed from memory of SQL Server instance to disk.
<ul>
	<li>Statistics Collection Interval option defined aggregation interval of query runtime statistics that should be used inside</li>
</ul>
the SQL Server Query Store. By default, it is set to 60 minutes. Lower value means that granularity of query runtime

statistics is finer, because of that, more intervals occur which requires more disk space for storing query runtime statistics.
<ul>
	<li>Max Size (MB) option is for configuring the maximum size of Query Store. SQL Server Query Store doesn’t auto grow and once the SQL Server Query Store reaches the maximum size, the Operation Mode will be switched to the Read Only mode, automatically, and new query execution plan and query runtime statistics will not be collected:</li>
	<li>Query Store Capture Mode option determines what type of query will be captured. Default, mode <b>All</b>, which means that every executed query will be stored. <b>Auto</b>then try to ignore infrequently executed and other ad hoc queries. When the <b>None</b> value is chosen, then Query Store will not gather information for the new queries and will continue gathering information only on the queries that it has been recorded previously.</li>
</ul>
<ul>
	<li>Size Based Cleanup Mode option is for cleaning the SQL Server Query Store data when the maximum size in the</li>
</ul>
Max Size (MB) option is reached to 90% of capacity.
<ul>
	<li>Stale Query Threshold (Days) option is for defining how long the data will stay in the SQL Server Query Store.</li>
	<li>SQL Server Query Store tab is an option that clears/purges all data in the SQL Server Query Store by</li>
</ul>
pressing the Purge Query Data button.
<h2><b>Query Store troubleshooting</b></h2>
<u><b>By navigating the Query Store sub-folder under the database node :</b></u>

<b>Regressed Queries: </b>Pinpoint queries for which execution metrics have recently regressed (i.e. changed to worse).

<b>Overall Resource Consumption: </b>Analyze the total resource consumption for the database for any of the execution metrics.

<b>Top Resource Consuming Queries: </b>Choose an execution metric of interest and identify queries that had the most extreme values for a provided time interval.

<b>Queries With Forced Plans: </b>Lists previously forced plans using Query Store. Query plan analysis and forcing query plans needs higher level expertise on SQL Server.

<b>Queries With High Variation: </b>Analyze queries with high execution variation as it relates to any of the available dimensions, such as Duration, CPU time, IO, and Memory usage in the desired time interval.

<b>Query Wait Statistics</b>: A useful view to analyze wait categories that are most active in a database, and which queries contribute most to the selected wait category.

<b>Tracked Queries: </b>Track the execution of the most important queries in real time.

<img class="alignnone size-full wp-image-1012" src="https://techforumugm.files.wordpress.com/2019/02/qs4.png" alt="QS4" width="367" height="450" />
