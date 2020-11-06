---
layout: post
title: Explore Dynamic Data Masking
date: 2020-01-27 01:50
author: techforumugm
comments: true
categories: [Business Intelligence]
---
DDM is a very simple security feature that can be fully built using T-SQL commands which we are familiar with, easy to use and also flexible to design.

Masking masks the sensitive data “on the fly” to protect sensitive data from non-privileged users using built-in or customized masking functions, without preventing them from retrieving the unmasked data.

<img class="alignnone size-full wp-image-1272" src="https://techforumugm.files.wordpress.com/2020/01/ddm01.jpg" alt="DDM01" width="286" height="176" />

Masking functions :-

Default function that masks the data according to the field data type:-

Binary, varbinary or image, a single byte of binary &gt;&gt; 0

Date and time data types &gt;&gt; 01.01.1900 00:00:00.0000000

Numeric data types &gt;&gt; zero value will be used to mask field

Email function &gt;&gt; First character of the email address and mask the rest of the email

Random &gt;&gt; masking function is used to mask any numeric data type by replacing the original value with a random value within the range specified in that function

Custom &gt;&gt; allows you to define mask for the specified field by exposing the first and last letters defined by the prefix and suffix and add a padding i.e. prefix, [padding value], suffix

<img class="alignnone size-full wp-image-1271" src="https://techforumugm.files.wordpress.com/2020/01/ddm02.png" alt="DDM02" width="264" height="191" />

&nbsp;
<h2>Why is It Required ?</h2>
<ul>
	<li>Prevent unauthorized access</li>
	<li>Non-privileged application users</li>
	<li>Minimal impact on the application layer</li>
	<li>Without modifying existing queries of Application</li>
	<li>Limit exposure of sensitive data</li>
	<li>Designated database fields</li>
	<li>Policy-based security feature</li>
	<li>Data in the database are unchanged</li>
</ul>
<h2>How is it works ?</h2>
Pre-defined functions for masking and the ability to define your masks
<ol>
	<li>Default: That replaces characters with ‚XXXX‘ and Numbers with 0</li>
	<li>Email: That replaces a part before the @ and puts a ‚@XXXX.com‘ at the end</li>
	<li>Random: That replaces numbers with random values</li>
	<li>Custom String: In this function you can define your own padding string</li>
</ol>
<h2>LAB Work :</h2>
Step 1:  To explore the feature we need to have a table
<pre>CREATE TABLE Employee(
EmpID INT PRIMARY KEY,
FirstName NVARCHAR(50) NOT NULL,
LastName NVARCHAR(50) ,
Birthdate DATE ,
CurrentFlag BIT ,
Salary MONEY ,
EmailAddress NVARCHAR(50),
SickLeave INT,
SalesInsentive MONEY,
NationalID NVARCHAR(15),
PhoneNumber NVARCHAR(25));

</pre>
Step 2: Create a user who is not authorized to view all data
<pre>CREATE USER hasan WITHOUT LOGIN; 
GRANT SELECT ON Employee TO hasan; 
Go
EXECUTE AS USER = 'hasan'; 
SELECT * FROM Employee; 
REVERT;</pre>
Step 3: Now we're going to mask a column
<pre>ALTER TABLE Employee ALTER COLUMN firstName varchar(10) 
MASKED WITH (FUNCTION = 'default()'); 

--After Apply Masking user will get masked value 
---------------------------------------------------------
EXECUTE AS USER = 'hasan'; 
SELECT * FROM Employee; 
REVERT;</pre>
Step 4 : Check the if admin user want to view the data then what will happen

Select * from Employee
<pre>Step 4 : Now we're going to mask different other columns having 
different data type and execute Select statement to view the change

--Mask the Email address
-----------------------------
ALTER TABLE Employee 
ALTER COLUMN EmailAddress nvarchar(50) MASKED WITH (FUNCTION = 'Email()'); 
--After Apply Masking to Employee table
EXECUTE AS USER = 'hasan'; 
SELECT * FROM Employee; 
REVERT; 
--Mask to Random value 
---------------------------------------------------------
ALTER TABLE Employee 
ALTER COLUMN Salary int MASKED WITH (FUNCTION='random(1,9)');

EXECUTE AS USER = 'hasan'; 
SELECT * FROM Employee; 
REVERT; 
-- Mask to Random value 
-------------------------
ALTER TABLE Employee 
ALTER COLUMN PhoneNumber nvarchar(25) MASKED WITH (FUNCTION= 'partial(3,"XXXX",3)');

EXECUTE AS USER = 'hasan'; 
SELECT * FROM Employee; 
REVERT;</pre>
<pre>Step 5: Now we execute the unmask feature to view the change 

GRANT UNMASK TO hasan
GO
EXECUTE AS USER = 'hasan'; 
SELECT * FROM Employee; 
REVERT; 
GO
REVOKE UNMASK TO hasan
EXECUTE AS USER = 'hasan'; 
SELECT * FROM Employee; 
REVERT;</pre>
Step 6: Now we drop the mask feature to view the change
<pre>ALTER TABLE Employee ALTER COLUMN LastName nvarchar(50) MASKED 
WITH (FUNCTION = 'default()');EXECUTE AS USER = 'hasan'; 
SELECT * FROM Employee; 
REVERT;
Go
ALTER TABLE Employee ALTER COLUMN Lastname DROP MASKED;
-- After Removing Masking 
EXECUTE AS USER = 'hasan'; 
SELECT * FROM Employee; 
REVERT;
-- Recycle 
-------------
ALTER TABLE Employee ALTER COLUMN FirstName DROP MASKED;
ALTER TABLE Employee ALTER COLUMN EmailAddress DROP MASKED;
ALTER TABLE Employee ALTER COLUMN PhoneNumber DROP MASKED;</pre>
