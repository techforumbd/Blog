---
layout: post
title: Explore Always Encrypted
date: 2020-04-03 21:04
author: techforumugm
comments: true
categories: [Business Intelligence]
---
<h2>What is Always Encrypted?</h2>
Always Encrypted is a data encryption technology that helps protect sensitive data at rest on the server, during movement between client and server, and while the data is in use, ensuring that sensitive data never appears as plain text inside the database system.
<h3></h3>
<img class="alignnone size-full wp-image-1277" src="https://techforumugm.files.wordpress.com/2020/04/ae01.png" alt="AE01" width="633" height="256" />
<h3><b>Type of Always Encrypted</b></h3>
<u><b>Deterministic Encryption:</b></u>
<ul>
	<li>Always generates the same encrypted value for any given plain text value</li>
	<li>Allows grouping, filtering by equality, and joining tables based on encrypted values</li>
	<li>Allows unauthorized users to guess information about encrypted values by examining patterns in the encrypted column</li>
	<li>Must use a column collation with a binary2 sort order for character columns</li>
</ul>
<u><b>Random:</b></u>
<ul>
	<li>Encrypts data in a less predictable manner</li>
	<li>Randomized encryption is more secure, but prevents equality searches, grouping, indexing, and joining on encrypted columns</li>
</ul>
<u><b>Column Master Keys</b></u>
<ul>
	<li>Used to encrypt column encryption keys</li>
	<li>Must be stored in a trusted key store</li>
	<li>Information about column master keys, including their location, is stored in the database in system catalog views</li>
</ul>
<u><b>Column Encryption Keys</b></u>
<ul>
	<li>Used to encrypt sensitive data stored in database columns</li>
	<li>Can be encrypted using a single column encryption key</li>
	<li>Encrypted values of column encryption keys are stored in the database in system catalog views</li>
	<li>Store column encryption keys in a secure/trusted location for backup</li>
</ul>
<h2>How is it works ?</h2>
The following illustration depicts the Always Encryption process.
<ol>
	<li>A user tries to execute a SQL statement from an application</li>
	<li>The SSN is encrypted in the database, so it is re-routed to the Enhanced ADO.NET Library</li>
	<li>The SSN the user specified is then encrypted and sent onto SQL Server</li>
	<li>SQL Server creates a SQL statement from the request, plus the SSN replaced with the cipher value</li>
	<li>The database then filters all rows that DO NOT match the cipher value</li>
	<li>The data is re-routed through the Enhanced ADO.NET Library so the cipher value can be decrypted</li>
	<li>The result set is returned to the user with the decrypted value</li>
</ol>
&nbsp;
<h2><b>Limitations and Technologies NOT Supported</b></h2>
<ul>
	<li>Encrypted columns do not allow range-like operations such as ‘&gt;, &lt;‘ , ‘LIKE’, etc.</li>
	<li>Passing encrypted values to functions, user-defined or otherwise, is not allowed(the database doesn’t have access to the unencrypted values)</li>
	<li>Equality comparisons can only be performed on columns that use deterministic encryption</li>
	<li>Indexes can only be applied to deterministic encryption columns</li>
	<li>Need same column encryption key for columns that are joined</li>
	<li>Constant expressions that refer to encrypted columns not allowed ex. WHERE SSN = ‘111-11-1111’, but WHERE SSN = @SSN is because the driver works with the SqlParameter class</li>
	<li>Unsupported data types: xml, rowversion, image, ntext, text, sql_variant, hierarchyid, geography, geometry, and UDFs</li>
	<li>Currently the only driver that supports this feature is .NET 4.6</li>
	<li>This is NOT TDE</li>
	<li>Encrypted columns take significantly more space</li>
	<li>String-based columns that are encrypted with deterministic encryption must use a _BIN2 collation (e.g. Latin1_General_BIN2)</li>
	<li>The following data types are NOT supported as encrypted columns, per the documentation:text/ntext/image ,XML/hierarchyid/geography/geometry,alias types/user-defined data types,SQL_VARIANT,rowversion (timestamp) ,Sparse columnset (sparse columns are okay, provided the table doesn’t contain a columnset),Built-in alias types, e.g. SYSNAME,Identity columns,Computed columns,Temporal tables,Triggers are partially supported,Full-text search,Replication (need more research),In-Memory OLTP,Stretch Database</li>
</ul>
&nbsp;

&nbsp;
