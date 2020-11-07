---
layout: post
title: SQL Server 2017 Launch with Great features
date: 2017-10-21 23:53
author: techforumugm
comments: true
categories: [MS SQLServer]
---
Microsoft's shift towards being a more open source-oriented company. The Linux distributions supported include RedHat Enterprise Linux, SuSE Enterprise Linux and Ubuntu. You can also run SQL Server 2017 in Docker containers, which is an option many development shops may consider for rapid deployment of development databases.The database engine binaries you install on Windows and Linux are literally the same exact files down to the byte.

What's new features arriving with SQL Server 2017 ?

<strong>Python Support :</strong>

Python is the standard for binding to deep learning libraries, like TensorFlow from Google or Cognitive Toolkit f.Those libraries are optimized to run on GPUs. Now it's easy to create native AI apps that work on data in SQL, without moving it out.

<strong>Query Store :</strong>
<p style="text-align:justify;">Though query Store introduced in SQL Server 2016. There a number of optimizations in space in SQL Server 2017, focused on making problematic query operations go faster.Tracking query execution plans and run time statistics identify regressed queries and changed execution plans . If the engine determines that a change in plan has occurred, and the query has regressed in performance, the engine will revert to a previous plan.</p>
<strong>Graph Database:</strong>

Graph query support is new, as is Python in Machine Learning Services, and Adaptive Query Processing and Automatic Tuning for better query optimization.Graph databases are commonly used to track relationships or hierarchies, a place where relational databases have struggled in terms of structure and performance. Graph databases are implemented via nodes (or vertices) and edges (or relationships). For example, you might say John is friends with Jane, and Jane is friends with Becky. While there have been a number of small graph database projects, many have not supported the SQL language, and integration with other systems. By bringing graph into SQL Server 2017, users can take advantage of native SQL, along with the new match operator to perform graph queries.

<strong>Resumable Online Index Rebuild:</strong>

As someone who has been a database administrator for way too long, this feature has tremendous appeal to me. Indexes get fragmented as updates and deletes happen, and need to be reorganized and rebuilt periodically. Performing these operations are very IO intensive, and are commonly run during maintenance windows. However on larger systems some operations may run beyond the window and have to be aborted, or more frequently, simply aren't run. Resumable index rebuild allows you to schedule fixed window for your maintenance operations (for example, allocating three hours a night to index maintenance); or simply pause and resume them manually. This feature will change the ways DBAs perform database maintenance.

<strong>Everything Else:</strong> Some of the other features included in this release are improved performance for backups, more enhancements to the In-Memory OLTP feature and integration of the popular Power Query tool with SQL Server Analysis Services. Microsoft also released a very interesting enhancement to its advanced analytics features by adding a <strong>predict </strong>operator to the T-SQL programming area. This can be a very fast way to perform analysis on a pre-existing R or Python model.

As you install SQL Server 2017, it is important to note that both SQL Server Management Studio and SQL Server Reporting Services are now separate installs from the database engine and other components. Management Studio has a different release cycle from SQL Server and is updated monthly.

<img class="alignnone size-full wp-image-815" src="https://techforumugm.files.wordpress.com/2017/10/sql-server-2017_thumb-600x338.png" alt="SQL-Server-2017_thumb-600x338" width="600" height="338" />

<strong>SQL Server 2017</strong> brings us some new <strong>T-SQL</strong> functions. They are very simple to use, and can also help us to simplify our <strong>T-SQL</strong> code.
<h4><strong>String_AGG</strong></h4>
This new function solves an old and very interesting problem: How can we concatenate the contents of a column from several records in a single string value, in a particular order? There are several points where you migh need this. For example, when some people have several e-mail addresses, or several phone numbers, and we would like to print a report with all these emails and phone numbers listed.This was  difficult to do up to now,  though it was possible to achieve this with some <strong>XML</strong> tricks.Let’s try an example. This script below creates a table and insert some records.
<blockquote>
<div>drop tableif exists namescreate table names
( [name] varchar(50) )

</div>
<div>go</div>
<div></div>
<div>insert into names values (‘joao’),(‘jose’),(‘maria’),(‘joaquim’)</div>
<div>go</div></blockquote>
This query below uses some tricks with <strong>XML</strong> to concatenate the names in a single comma-separated string:
<blockquote>
<div>select <i>stuff</i>((select ‘,’ + [name] as [text()]
from names for xml path(”)),1,1,”)</div></blockquote>
<div></div>
<div>

The new <strong>STRING_AGG</strong> function gives us the same result:
<blockquote>
<div>select <b>string_agg</b>([name],‘,’)
from names</div></blockquote>
The <strong>AdventureWorks</strong> database has another interesting example where this function can be used. The tables <em>‘Person.Person’</em> and <em>‘Person.EmailAddress’</em> are related and each people can have several email addresses. It’s an usual need to list the people with their email addresses in a single record.This query below should achieve this,  but there is a catch:
<blockquote>
<div>select lastname,<b>string_agg</b>(emailaddress,‘, ‘) email
from person.person, person.EmailAddress
where person.BusinessEntityID=EmailAddress.BusinessEntityID
group by lastname</div></blockquote>
The result will be the following error:

The size limit of the <strong>string_agg</strong> function results depends on the datatype that is passed to it. Usually, the data type will be varchar, as in the example above, and because the datatype of the column is 8000 bytes, the size limit for the aggregated column will be 8000 bytes.We saw this error message even though we have no  record over 8000 bytes, but the records combined together exceeded 8000 bytes. The solution is to change the datatype of the field. We can use the <strong>‘Cast’</strong> function for this:
<blockquote>
<div>select lastname,<b>string_agg</b>(<i>cast</i>(emailaddress as <i>varchar</i>(max)),‘, ‘) email
from person.person, person.EmailAddress
where person.BusinessEntityID=EmailAddress.BusinessEntityID
group by lastname</div></blockquote>
<h3><strong>Trim</strong></h3>
This new function has been requested for a lot of SQL Server DBAs for a long time.Removing the empty spaces in a string always demanded the use of two functions, like this:
<blockquote>
<div>SELECT <i>RTRIM</i>(<i>LTRIM</i>( ‘     test    ‘)) AS Result;</div></blockquote>
This new function simplifies this task:
<blockquote>
<div>SELECT <b>TRIM</b>( ‘     test    ‘) AS Result;</div></blockquote>
<h4><strong>Concat_WS</strong></h4>
<strong>Concat_WS</strong> function is similar to the Concat function that exists since SQL Server 2012, with the ‘WS’ as a plus. ‘WS’ in this case means <em>‘With Separator’</em>, meaning this new function is able to add a separator between each string value it concatenates.The NULL value behavior with both functions is the same: NULL values are ignored, not even adding the separator.

This isn’t SQL Server’s default behavior in a concatenation. By default, concatenating a NULL value with a string value yields a null value. Despite what a lot of people believe, NULL doesn’t mean an empty value, NULL means an unknown value. That’s why any value concatenated with NULL yields NULL: the result is also unknown.

SQL Server has a session configuration called <strong>CONCAT_NULL_YIELDS_NULL</strong>, but this configuration is deprecated. You can see more about this <a href="https://docs.microsoft.com/en-us/sql/t-sql/statements/set-concat-null-yields-null-transact-sql">here</a> .Both functions, <strong>CONCAT</strong> and <strong>CONCAT_WS</strong>, ignores the default behavior and the <strong>CONCAT_NULL_YIELDS_NULL</strong> configuration, ignoring NULL values during the concatenation.This is very useful to simplify the queries when we need to concatenate fields that aren’t always filled, such as address fields, that sometimes have all the fields filled and sometimes haven’t.The first example below use a comma as a separator, the 2nd uses a carriage-return (char(13)) :
<blockquote>
<div>SELECT <b>CONCAT_WS</b>(‘,’,‘1 Microsoft Way’, NULL, NULL, ‘Redmond’, ‘WA’, 98052) AS Address;</div>
select <b>Concat_WS</b>(<i>char</i>(13),addressline1,addressline2,city,PostalCode)
as [Address],AddressId
from person.Address</blockquote>
This function can be useful to produce reports, concatenating some fields, however it’s not useful for exporting data, because when we export data we need some kind of separator, such as a semi-colon (“;”) even when a field is NULL, but this function doesn’t add the separator when a field is NULL.
<h2><strong>Translate</strong></h2>
Translate does the work of several replace functions, simplifying some queries.The function is called <strong>‘Translate’</strong> because its main objective: transform one kind of information in another by doing a bunch of replaces.For example: GeoJson and WKT are two different formats for coordinates. In GeoJson a coordinate is represented using the format <em>‘[137.4, 72.3]’</em> while in WKT a point is represented using the format<em>‘(137.4 72.3)’</em>.

We would need several <strong>‘Replace’s</strong> to transform GeoJson format in WKT format and the reverse. The <strong>‘Translate’</strong> function can do this easily.Using <strong>‘Replace’</strong> function the transformation would be like this:
<blockquote>
<div>select <i>replace</i>(<i>replace</i>(<i>replace</i>(‘[137.4, 72.3]’,‘[‘,‘(‘),‘,’,‘ ‘),‘]’,‘)’) as Point</div></blockquote>
Using the <strong>‘Translate’</strong> function the transformations becomes way simpler:
<blockquote>
<div>SELECT <b>TRANSLATE</b>(‘[137.4, 72.3]’ , ‘[,]’, ‘( )’) AS Point,
<b>TRANSLATE</b>(‘(137.4 72.3)’ , ‘( )’, ‘[,]’) AS Coordinates</div></blockquote>
Instead of several <em>‘Replaces’</em>, the <em>‘Translate’</em> syntax allows us to specify all the characters in the source string we would like to replace and all the new characters.

</div>
