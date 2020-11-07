---
layout: post
title: Performance Consideration of joining TABLES in Microsoft SQL Server
date: 2014-08-10 07:38
author: techforumugm
comments: true
categories: [MS SQLServer, Performance Tuning]
---
<br /><br /><ol><li>Performance is to limit how many rows need to be Joined.</li><li>Each of the joined columns have their own indexes. </li><li>the columns used for the joins are not naturally compact, then considering adding surrogate keys to the tables that are compact in order to reduce the size of the keys.</li><li>Plan to join a table to the table with the foreign key, using the foreign key as the linking column, then you should consider adding an index to the foreign key column. </li><li>SQL Server optimizer may not be able to use an existing index in order to speed up the join. Ideally, for best performance, joins should be done on columns that have unique indexes.</li><li>Indexes on the columns to be joined should have the same data type, and ideally, the same width.</li><li>You shouldn’t mix non-Unicode and Unicode data types.</li></ol>
