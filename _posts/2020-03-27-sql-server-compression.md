---
layout: post
title: SQL Server Data Compression 
date: 2020-03-27 00:03
author: techforumugm
comments: true
categories: [MS SQLServer]
---
<h2>What is Data Compression ?</h2>
Data compression is a very effective technique to reduce the size of data in the database. Less I/O processes happens due to processing of compressed data and increases the load on CPU requirements. Compression is a great way to get more mileage out of your database.Data compression is a technology that's been around since SQL Server 2008. The idea of data compression is that you can selectively choose tables, indexes, or partitions within a database. Data compression applies to:
<ul>
	<li>Heaps.</li>
	<li>Clustered indexes.</li>
	<li>Non-clustered indexes.</li>
	<li>Partitions.</li>
	<li>Indexed views.</li>
</ul>
<h2>Why Data Compression Required ?</h2>
Large objects i.e. LOB and BLOB are not compressed .I/O can be a bottleneck in moving information between in-and-out of the database. The network speeds are so much slower than processing speed . Here Efficiency gains are happens using the processing of compress data in a database, so that it travels faster. And boost up processing power.
<ul>
	<li>Effective space management</li>
	<li>Efficient cost reduction technique</li>
	<li>Ease of database backup management</li>
	<li>Effective N/W bandwidth utilization</li>
	<li>Safe and faster recovery or restoration</li>
	<li>Better performance – reduces the memory footprint of the system</li>
</ul>
<h2>Types of SQL Server compression</h2>
Data compression is a technology that's been around since SQL Server 2008. There are two types of data compression
<ul>
	<li>Row-level and</li>
	<li>Page-level</li>
</ul>
<h3><u>The row-level </u></h3>
Compression works behind the scenes and converts any fixed length data types into variable length types. The assumption here is that often data is stored at a fixed length type, such as char 100, and they don’t actually fill the entire 100 characters for every record.The concept of compression is extended to all fixed-length data types, including char, int, and float. If you stored the value of 100 in an int column, the SQL Server needn’t use all 32 bits, instead, it simply uses 8 bits (1 byte).
<h3><u><b>Page Compression</b></u></h3>
First, it automatically applies row-level compression on fixed length data fields, so you automatically get those gains by default. Then on top of that, Page compression is works. Page compression is categorized into two types
<ul>
	<li>Prefix compression and</li>
	<li>Dictionary compression.</li>
</ul>
<h4><span style="text-decoration:underline;"><b>Prefix Compression</b></span></h4>
For each page that is being compressed, prefix compression uses the following steps:
<ul>
	<li>For each column, value is identified that can be used to reduce the storage space for the values in each column.</li>
	<li>A row that represents the prefix values for each column is created and stored in the compression information (CI) structure that immediately follows the page header.</li>
	<li>The repeated prefix values in the column are replaced by a reference to the corresponding prefix. If the value in a row does not exactly match the selected prefix value, a partial match can still be indicated.</li>
</ul>
<img class="alignnone size-full wp-image-954" src="https://techforumugm.files.wordpress.com/2020/03/compression.png" alt="Compression" width="346" height="636" />
<h4><span style="text-decoration:underline;"><b>Dictionary Compression</b></span></h4>
After prefix compression has been completed, dictionary compression is applied to the database. Dictionary compression searches for repeated values anywhere on the page and stores them in the CI area. Unlike prefix compression, dictionary compression is not restricted to one column. Dictionary compression can replace repeated values that occur anywhere on a page.

<img class="alignnone size-full wp-image-955" src="https://techforumugm.files.wordpress.com/2020/03/compression02.png" alt="Compression02" width="346" height="587" />
<h2>How can you do the compression ?</h2>
In the following example we've taken the WideWorldImporters sample database of

Microsoft SQL Server.You can easily download the database .
<pre>-------------------------------------------------------------------------
--Step 1 : First view compression settings for objects in the database
-------------------------------------------------------------------------
USE WideWorldImporters;
GO
SELECT S.name AS SchemaName, O.name AS ObjectName, I.name AS IndexName,
I.type_desc AS IndexType, P.data_compression_desc AS Compression
FROM sys.schemas AS S JOIN sys.objects AS O
ON S.schema_id = O.schema_id JOIN sys.indexes AS I
ON O.object_id = I.object_id JOIN sys.partitions AS P
ON I.object_id = P.object_id AND I.index_id = P.index_id
WHERE O.TYPE = 'U'
ORDER BY S.name, O.name, I.index_id;
GO

-------------------------------------------------------------------------
-- Step 2 : Check the space used by each table:
-------------------------------------------------------------------------
SELECT 
    t.NAME AS TableName,
    i.name as indexName,
    sum(p.rows) as RowCounts,
    sum(a.total_pages) as TotalPages, 
    sum(a.used_pages) as UsedPages, 
    sum(a.data_pages) as DataPages,
    (sum(a.total_pages) * 8) / 1024 as TotalSpaceMB, 
    (sum(a.used_pages) * 8) / 1024 as UsedSpaceMB, 
    (sum(a.data_pages) * 8) / 1024 as DataSpaceMB
FROM 
    sys.tables t
INNER JOIN      
    sys.indexes i ON t.OBJECT_ID = i.object_id
INNER JOIN 
    sys.partitions p ON i.object_id = p.OBJECT_ID 
    AND i.index_id = p.index_id
INNER JOIN 
    sys.allocation_units a ON p.partition_id = a.container_id
WHERE 
    t.NAME NOT LIKE 'dt%' AND
    i.OBJECT_ID &gt; 255 AND   
    i.index_id &lt;= 1
GROUP BY 
    t.NAME, i.object_id, i.index_id, i.name 
ORDER BY 
   TotalSpaceMB desc

-----------------------------------------------------------------------
--Step 3:  Pick a single Table 
----------------------------------------------------------------------- 
USE WideWorldImporters;
GO
SELECT S.name AS SchemaName, O.name AS ObjectName, I.name AS IndexName,
	I.type_desc AS IndexType, P.data_compression_desc AS Compression
	FROM sys.schemas AS S JOIN sys.objects AS O
	ON S.schema_id = O.schema_id JOIN sys.indexes AS I
	ON O.object_id = I.object_id JOIN sys.partitions AS P
	ON I.object_id = P.object_id AND I.index_id = P.index_id
	WHERE O.TYPE = 'U' and o.name ='PurchaseOrderLines'
	ORDER BY S.name, O.name, I.index_id;

</pre>
&nbsp;

&nbsp;
<pre>-----------------------------------------------------------------------
/* Step 4 : To estimate compression, 
            run the following system stored procedure  */
-----------------------------------------------------------------------
EXEC sp_estimate_data_compression_savings 
@schema_name = 'Sales', 
@object_name = 'OrderLines', 
@index_id = NULL, 
@partition_number = NULL, 
@data_compression = 'ROW'
GO
EXEC sp_estimate_data_compression_savings
@schema_name = 'Purchasing',
@object_name = 'PurchaseOrderLines',
@index_id = NULL,
@partition_number = NULL,
@data_compression = 'Page';
GO</pre>
&nbsp;
<pre>-----------------------------------------------------------------------
-- Step 5: Let’s get things compressed:
-----------------------------------------------------------------------
ALTER INDEX PK_SalesOrderDetail_SalesOrderID_SalesOrderDetailID 
ON Sales.SalesOrderDetail 
REBUILD PARTITION = ALL 
WITH (DATA_COMPRESSION = PAGE);
Go

ALTER INDEX IX_SalesOrderDetail_ProductID
ON Sales.SalesOrderDetail 
REBUILD PARTITION = ALL 
WITH (DATA_COMPRESSION = PAGE);
GO</pre>
&nbsp;
<pre>-----------------------------------------------------------------------
--Step 6 : To view any space to give back with a fairly simple query:
-----------------------------------------------------------------------
SELECT name,
s.used / 128.0 AS SpaceUsedInMB,
size / 128.0 - s.used / 128.0 AS AvailableSpaceInMB
FROM sys.database_files
CROSS APPLY (SELECT CAST(FILEPROPERTY(name, 'SpaceUsed') AS INT)) s(used)
WHERE FILEPROPERTY(name, 'SpaceUsed') IS NOT NULL;

</pre>
&nbsp;
<pre>----------------------------------------------------------------
/*  Step 7: Execute the Shrink command to giving back 
            some storage to the file system:  */
-----------------------------------------------------------------
Use WideWorldImporters
GO
DBCC SHRINKFILE (N'TestDB_log', 6)
Go</pre>
