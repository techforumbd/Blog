---
layout: post
title: Insight of Query Execution
date: 2019-10-05 22:15
author: techforumugm
comments: true
categories: [MS SQLServer]
---
It's good to know how a MS SQL Server query is processed by SQL Server database engine.There are six steps by that a query is executed.It'll help to get idea on few matters
<ol>
	<li>Dirty Pages</li>
	<li>Read SQL Server Query execution Plan</li>
	<li>Isolation and Locking</li>
</ol>
Following steps get idea of query execution internals :

Step 1 : Application send modification request

Step 2 :

SQL Server find out the location of the pages related to modified data. In a first preference it search to the buffer and after that disk.

Step 3 :

Identified pages are kept into the buffer cache. This pages are called clean pages because no change has made to those yet now.

Step 4 :

SQL Server lock those pages and execute required modification. Now changed perform to page those are called dirty pages.

<img class="alignnone size-full wp-image-934" src="https://techforumugm.files.wordpress.com/2020/03/execution-plan.png" alt="Execution Plan" width="702" height="419" />

Step 5 :

Now the details log records has generated and stored into the log file (in disk) from buffer cache. It ensures that if any issues happen and the server suddenly shuts down, during database recovery, it reads the transaction log file and prepares the recovery process (UNDO, REDO).It also sends commit acknowledgment to the user. The changed page is still in the buffer cache.Using DMV <b>sys.dm_os_buffer_descriptors</b> to check the dirty pages in memory and use the column is_modified to see the dirty pages:

SELECT db_name(database_id) AS 'Database',count(page_id) AS

'Dirty Pages’  FROM sys.dm_os_buffer_descriptors

WHERE is_modified =1

GROUP BY db_name(database_id)

ORDER BY count(page_id) DESC

<img class="alignnone size-full wp-image-937" src="https://techforumugm.files.wordpress.com/2020/03/execution-plan02.png" alt="Execution Plan02" width="618" height="366" />

Step 6 :

A Checkpoint process writes all dirty pages (available in the buffer cache) and transaction log records to the disk. It also logs checkpoint information in the transaction log. It performs the following tasks as shown in the following image.
<ul>
	<li>It writes the log records from the buffer cache to the disk ( transaction log file)</li>
	<li>It writes all dirty pages ( modified pages since the last checkpoint) to the data file ( MDF/NDF)</li>
</ul>
