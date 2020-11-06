---
layout: post
title: Explore Column Stored Index
date: 2019-09-23 00:45
author: techforumugm
comments: true
categories: [MS SQLServer]
---
<h3><b>What is a column store index?</b></h3>
A columnstore index is a type of data structure that’s used to store, manage and retrieve data that is stored in a columnar-style database.<img class="alignnone size-full wp-image-1189" src="https://techforumugm.files.wordpress.com/2020/03/csi01.png" alt="CSI01" width="1259" height="533" />
<h3>Why Column Store Index is used ?</h3>
<ul>
	<li>Best for queries that scan/aggregate large sets of data.</li>
	<li>IO Statistics - dramatically reduces # of logical reads.</li>
	<li>Smart IO and caching using aggressive read-ahead read strategy.</li>
	<li>In a regular index, indexed data from each row kept together on single page – and the data in each column spread across all pages of index</li>
	<li>Best for data warehouse/mart queries that scan/aggregate large amounts of data–might lower need for OLAP aggregation</li>
	<li>In a regular index, indexed data from each row kept together on single page – and the data in each column spread across all pages of index</li>
	<li>More and more functionality in DB engine (xVelocity, CDC)</li>
	<li>Some queries might run at least 10x faster (or more)</li>
	<li>Reduced storage costs and enhance performance.</li>
	<li>In Data Warehouses/Data Marts (in a star schema) ,queries performance is gained</li>
	<li>Load from Data Warehouses/Marts into OLAP Cubes</li>
	<li>SSAS OLAP Databases that use the ROLAP methodology or pass-through mode “might” benefit (more so in SQL 2014)</li>
	<li>New Analysis Services Tabular Model uses xVelocity engine</li>
</ul>
<h3><b>How to Create a column store index?</b></h3>
Expand the  table. Under Indexes, right click and select New Index and then Clustered Column store Index as shown below :

Using SQL Server Management Studio :

[gallery ids="1195,1196" type="rectangular"]

Using T-SQL
<pre>CREATE CLUSTERED COLUMNSTORE INDEX [IXProductID] ON [dbo].[Product]

WITH (DROP_EXISTING = OFF, COMPRESSION_DELAY = 0, DATA_COMPRESSION = COLUMNSTORE) 
ON [PRIMARY]

GO</pre>
&nbsp;
<h3><b>Difference Between Column store vs. Row store index</b></h3>
<b>Row store :</b>
<ul>
	<li>Row store data is logically organized by rows and columns, and is physically stored in row-oriented data pages.</li>
	<li>Row store indexes perform best on queries that seek data by searching for a particular value or retrieving a small range of values.</li>
</ul>
<b>Column store :</b>
<ul>
	<li>The column store index is also logically organized as a table with rows and columns, but the data is physically stored in a column-wise data format.</li>
	<li>Column store indexes work well for mostly read-only queries with large data sets, like data warehousing workloads.</li>
</ul>
&nbsp;

<img class="alignnone size-full wp-image-1199" src="https://techforumugm.files.wordpress.com/2019/09/csi04.png" alt="CSI04" width="690" height="332" />
<table width="640">
<tbody>
<tr>
<td width="307">                Row Store Index</td>
<td width="333">                  Column Store Index</td>
</tr>
<tr>
<td width="307">Row store indexes tend to be better for online transaction processing (OLTP) workloads, which use more update and seek operations.</td>
<td width="333">Column store indexes tend to be better for online analytical processing (OLAP) workloads, which use more read operations.</td>
</tr>
<tr>
<td width="307">Row store indexes tend to be better at performing random reads and writes.</td>
<td width="333">Column store indexes tend to be better for performing sequential reads and writes.</td>
</tr>
<tr>
<td width="307">Row store data is logically organized by rows and columns, and is physically stored in row-oriented data pages.</td>
<td width="333">The column store index is also logically organized as a table with rows and columns, but the data is physically stored in a column-wise data format.</td>
</tr>
</tbody>
</table>
<h3>Limitations of Column Store Index</h3>
<ul>
	<li>Cannot be clustered, cannot be created against a view</li>
	<li>Cannot act as a PK or FK, cannot include sparse columns</li>
	<li>Can’t work on tables with Change Data Capture/Change Tracking or FileStream, can’t participate in replication, nor when page/row compression exists</li>
	<li>Cannot be used with certain data types, such as binary, text/image, rowversion/timestamp, CLR data types (hierarchyID/spatial), nor with data types created with MAX keyword…e.g. varchar(max)</li>
	<li>Cannot be used with Unique Identifier</li>
	<li>Cannot be used with decimal &gt; 18</li>
	<li>Cannot be modified with ALTER – must be dropped and recreated</li>
	<li>Not optimized for certain statements (OUTER JOIN, UNION, NOT IN &lt;subquery&gt;)</li>
	<li>Not optimized for certain scenarios (high selectivity, queries lacking any aggregations)</li>
	<li>Issue OUTER JOIN: can’t use directly against table</li>
	<li>Will “work”, but will use slower row execution mode</li>
	<li>Must pre-aggregate separately and then do OUTER JOIN</li>
</ul>
&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;
