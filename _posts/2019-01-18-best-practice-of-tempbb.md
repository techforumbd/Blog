---
layout: post
title: Best Practice of tempBB
date: 2019-01-18 00:49
author: techforumugm
comments: true
categories: [MS SQLServer]
---
<img class="alignnone  wp-image-1022" src="https://techforumugm.files.wordpress.com/2019/01/td01.png" alt="TD01" width="262" height="253" />

SQL Server instance, use it to hold temporary objects that users, or the database engine, create.SQL Server recreates the tempdb database each time the SQL Server service starts.  tempdb is a temporary store.It is common for tempdb memory requirements to exceed the capacity of the buffer pool in which case, the data will spool to the I/O subsystem. The performance of the I/O subsystem that holds tempdb data files can therefore significantly impact the performance of the system as a whole. If the performance of tempdb is a bottleneck in your system

<b>Pre-Size </b><b>TempDB</b>

There is no absolutely science how big your TempDB should be. However, here is what I tell my customer, size it at least as big as your biggest Index is so when it is rebuilt your TempDB does not have to grow bigger. If you are not sure about it, just go ahead and keep your Temp database as 25% of the largest database, I think it is a good beginning spot.

<b>Faster Drives – SSD</b>

It goes without saying that you should have your Temp database on the fastest possible drive. I have often seen people keeping it on the slower drive or on the same drive as where they have installed the Operating System. This is indeed not a great idea. My personal preference is that you keep your Temp data and log files on the SSD drives.

<b>Separate Data and Log Files for </b><b>TempDB</b>

This is always true for any of the databases including your Temp Database. Take advantage of IO Parallel operations by keeping your data and log to separate drive.

Mu<b>ltiple Data and Single Log Files</b>

A very popular question is how many Temp data files should one have it. Here is the simple answer to it. As many as logical CPUs you have but not more than 8 in any case. If you have 4 logical CPUs you should have 4 Temp data files but if you have 12 logical CPUs you should cap your temp data files at 8.

<b>Set </b><b>FileGrowth</b><b> to a Large Fixed Value</b>

A common practice is to not set FileGrowth value for your Temp Database. I often see users setting this one on their user database but not the system database. For TempDB it is critical that you set a large fixed value for the autogrowth to avoid extra overhead on the CPU to grow every time your Temporary database is filled up.
<h2><b>Temp DB Concern</b></h2>
<u><b>Keep </b></u><u><b>TempDB</b></u><u><b> on Local drive in Cluster</b></u>

With faster drives like SSD and FusionIO cards, there’s been an increased interest in keeping TempDB on those drives in case of cluster also. Microsoft has heard this feedback and allowed SQL Server to keep TempDB files in local drive, in the case of a cluster. One advantage of placing TempDB on a local disk is that it creates separate paths of IO traffic by having other database files on the SAN and TempDB files on the local disk. By using a PCIe SSD or traditional hard drive SSDs, the IO operations performed on TempDB will bypass HBAs. This provides better performance for TempDB operations and avoids contention on a shared storage network or array.

the SAN would be replicated from one location to another, maybe few miles or many miles apart. If the TempDB is kept on SAN, it would also be replicated and as mentioned earlier, it’s a scratchpad kind of database for SQL Server. Keeping files on local drives would mean better bandwidth usage and faster failovers.

<u><b>Configure for multiple DATA Files</b></u>

When there are multiple data files in a database, all the writes to the database are striped across all files based on the proportion of free space that the file has to the total free space across all of the files. Now, each of the data files has its own set of allocation pages (called PFS, GAM, and SGAM pages) so as the writes move from file to file the page allocations occur from different allocation bitmap pages, spreading the work out across the files and reducing the contention on any individual page.

The general recommendation is that it should be equal to logical processors, if less than 8 else configure it to 8 files. For example, if we have a dual-core processor, then set the number of TempDB data files equal to two.  If we have more than 8 cores, start with 8 files and add four at a time as needed. We also need to ensure that the initial size and auto-growth settings for ALL TempDB data files are configured in the same way.

<u><b>Consider trace flag 1117 and 1118</b></u>

These are two trace flags which are useful to avoid contention in TempDB database. The most common trace flag is 1118 which prevents contention on the SGAM pages by slightly changing the allocation algorithm used. When trace flag 1118 is enabled, the allocation in TempDB are changes from a single page at a time from a mixed extent (8 times) to allocate an extent of 8 pages. So when there are multiple temp tables creation in TempDB database, allocation bitmap contention would be alleviated.

Less well known, trace flag 1117 changes the auto-grow algorithm in SQL Server. It is always recommended to manually grow the data files. This is because when SQL Server performs auto-grow of data files, it is done one data file at a time in a round robin fashion. When this happens, SQL Server auto-grows the first file, writes to it until it is filled and then auto-grows the next data file. If you observed, the proportional fill is broken now. When trace flag 1117 is enabled, then when SQL Server has to perform auto-grow of a data file, it auto-grows all of the files at the same time.

<u><b>SORT_IN_TEMPDB Option For Indexes</b></u>

This option increases the amount of temporary disk space that is used to create an index, the option could reduce the time that is required to create or rebuild an index when tempdb is on a set of disks different from that of the user database.

When you set the SORT_IN_TEMPDB option to ON, you must have sufficient free disk space available in tempdb to hold the intermediate sort runs, and enough free disk space in the destination filegroup to hold the new index. The CREATE INDEX statement fails if there is insufficient free space and there is some reason the databases cannot autogrow to acquire more space, such as no space on the disk or autogrow is set to off.

If SORT_IN_TEMPDB is set to ON, there must be sufficient free space in tempdb to store the sort runs, and sufficient free space in the destination filegroup to store the final index structure. The sort runs contain the leaf rows of the index.

If SORT_IN_TEMPDB is set to OFF, the free space in the destination filegroup must be large enough to store the final index structure. The continuity of the index extends may be improved if more free space is available.
