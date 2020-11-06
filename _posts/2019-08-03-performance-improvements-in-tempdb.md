---
layout: post
title: Performance improvements in TempDB
date: 2019-08-03 00:51
author: techforumugm
comments: true
categories: [MS SQLServer]
---
<img class="alignnone size-full wp-image-1042" src="https://techforumugm.files.wordpress.com/2019/08/td02.png" alt="TD02" width="257" height="245" />

Starting with SQL Server 2016 (13.x), TempDB performance is further optimized in the following ways:
<ul>
	<li>Temporary tables and table variables are cached. Caching allows operations that drop and create the temporary objects to execute very quickly and reduces page allocation contention.</li>
	<li>Allocation page latching protocol is improved to reduce the number of UP (update) latches that are used.</li>
	<li>Logging overhead for TempDB is reduced to reduce disk I/O bandwidth consumption on the TempDB log file.</li>
	<li>Setup adds multiple TempDB data files during a new instance installation. This task can be accomplished with the new UI input control on the Database Engine Configuration section and a command-line parameter /SQLTEMPDBFILECOUNT. By default, setup adds as many TempDB data files as the logical processor count or eight, whichever is lower.</li>
	<li>When there are multiple TempDB data files, all files autogrow at same time and by the same amount depending on growth settings. Trace flag 1117 is no longer required.</li>
	<li>All allocations in TempDB use uniform extents. Trace flag 1118 is no longer required.</li>
	<li>For the primary filegroup, the AUTOGROW_ALL_FILES property is turned on and the property cannot be modified.</li>
</ul>
