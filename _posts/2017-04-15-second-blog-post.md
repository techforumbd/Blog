---
layout: post
title: What's New in SQL Server 2016
date: 2017-04-15 18:07
author: techforumugm
comments: true
categories: [MS SQLServer]
---
SQL Server 2016 delivers breakthrough mission-critical capabilities with in-memory performance and operational analytics built-in. Comprehensive security features like new Always Encrypted technology helps protect your data at rest and in motion, and a world class high availability and disaster recovery solution adds new enhancements to AlwaysOn technology.

In-Memory OLTP

In-Memory OLTP enable scaling to larger databases and higher throughput in order to support bigger workloads. In addition, a number of limitations concerning tables and stored procedures have been removed to make it easier to migrate your applications to and leverage the benefits of In-Memory OLTP.

Live Query Statistics

Management Studio provides the ability to view the live execution plan of an active query. This live query plan provides real-time insights into the query execution process as the controls flow from one query plan operator to another.

Query Store

Query store is a new feature in that provides DBAs with insight on query plan choice and performance. It simplifies performance troubleshooting by enabling you to quickly find performance differences caused by changes in query plans. The feature automatically captures a history of queries, plans, and runtime statistics, and retains these for your review. It separates data by time windows, allowing you to see database usage patterns and understand when query plan changes happened on the server. The query store presents information by using a Management Studio dialog box, and lets you force the query to one of the selected query plans

Temporal Tables

A temporal table is a new type of table that provides correct information about stored facts at any point in time. Each temporal table consists of two tables actually, one for the current data and one for the historical data. The system automagically ensures that when the data changes in the table with the current data the previous values are stored in the historical table. Querying constructs are provided to hide this complexity from users.

Managed Backup to Microsoft Azure uses the new block blob storage for backup files. There are also several changes and enhancements to Managed Backup.

i.      Support for both automated and custom scheduling of backups.
ii.     Support backups for system databases.
iii.    Support for databases that are using the Simple recovery model.
iv.    Option to store the latest full backup locally before uploading to Microsoft Azure.

Multiple TempDB Databases

Setup adds multiple tempdb data files during the installation of a new instance.Format Query Results as JSON with FOR JSON

Format query results as JSON by adding the FOR JSON clause to a SELECT statement.

Use the FOR JSON clause, for example, to delegate the formatting of JSON output from your client applications to SQL Server.

Always Encrypted

With Always Encrypted, SQL Server can perform operations on encrypted data, and best of all the encryption key resides with the application inside the customer’s trusted environment and not on the server. Always Encrypted secures customer data so DBAs do not have access to plain text data. Encryption and decryption of data happens transparently at the driver level minimizing changes that have to be made to existing applications. For more information, see Always Encrypted (Database Engine).

<span style="font-family:Calibri;font-size:small;">Row Level Security (RLS)</span>

<span style="font-family:Calibri;font-size:small;">You have to ensure that Portfolio managers only can able to view his investors information only. Row Level Security (RLS) has come to assist in this scenario.Row Level Security (RLS) features has introduced in SQL Server 2016 to uniquely manage and protect data at row level.</span>

Stretch Database

Stretch Database is a new feature in SQL Server 2016 leverages resources in Windows Azure to store and query archival data. Stretch Database automatically archives eligible rows from Stretch-enabled tables and uses computational resources in Azure to offload queries over the archived rows

Transact-SQL Enhancements

The TRUNCATE TABLE statement now permits the truncation of specified partitions.

&nbsp;

The first public preview of SQL Server 2016 <span class="category">Evaluations</span> is : <a href="http://cic.ms/sCjdl4" target="_blank" rel="nofollow"><span style="color:#0066cc;">http://cic.ms/sCjdl4</span></a><img class="alignnone size-full wp-image-334" src="https://techforumugm.files.wordpress.com/2017/04/thsd2xm0sl.jpg" alt="thSD2XM0SL" width="259" height="160" /><b></b><i></i><u></u>
