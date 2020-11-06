---
layout: post
title: Explore Row Level Security
date: 2020-03-07 02:03
author: techforumugm
comments: true
categories: [MS SQLServer]
---
<h2>What is Row Level Security ?</h2>
It is introduced in SQL Server 2016, row-level security feature allows users to have access to a table but restricts them to accessing specific rows within that table. Prior to SQL Server 2016, this required custom stored procedures to be written for the provision of such fine-grained security. However, such stored procedures are prone to SQL injection and other security caveats.
<h2><b>Why Row Level Security required ?</b></h2>
<ul>
	<li>Row-Level Security (RLS) simplifies the design and coding of security in your application.</li>
	<li>The access restriction logic is located in the database tier rather than away from the data in another application tier. This makes your security system more reliable and robust by reducing the surface area of your security system.</li>
	<li>For example ensuring that workers can access only those data rows that are pertinent to their department, or restricting a customer's data access to only the data relevant to their company.</li>
</ul>
<h2>How is it Working  ?</h2>
While selecting the data from the table, rows are filtered based on the execution context of every query. The access restriction logic is located in the database tier rather than application tier. The database system applies the access restrictions every time that data access is attempted from any tier. This makes your security system more reliable and robust. RLS is Implement by using the SECURITY POLICY and Predicate Function .
<h4>Security Predicate :</h4>
Access to row-level data in a table is restricted by a security predicate which contains the logic for filtering the rows defined as an inline table-valued function. The function is then invoked and enforced by a security policy.
<h4>Security Policy:</h4>
This is a new object which can be CREATEed ALTERed and DROPed. You can imagine this as a container of predicates which can be applied to tables. One policy can contain security predicate for many tables. A policy can be in an ON or OFF state.

RLS supports two types of security predicates.:
<ol>
	<li>Filter predicates silently filter the rows available to read operations (SELECT, UPDATE, and DELETE).</li>
	<li>Block predicates explicitly block write operations (AFTER INSERT, AFTER UPDATE, BEFORE UPDATE, BEFORE DELETE) that violate the predicate.</li>
</ol>
Block predicates affect all write operations.
<ul>
	<li>AFTER INSERT and AFTER UPDATE predicates can prevent users from updating rows to values that violate the predicate.</li>
	<li>BEFORE UPDATE predicates can prevent users from updating rows that currently violate the predicate.</li>
	<li>BEFORE DELETE predicates can block delete operations.</li>
</ul>
Row-level security would take care of custom filtering in the form of a user-defined table-valued function, implementing it on the object for everyone. We can add more predicate in the security policy and apply a filter on multiple tables. Since we can turn the policy ON and OFF, it makes it easy to manage all filters at the same time.

<img class="alignnone size-full wp-image-1255" src="https://techforumugm.files.wordpress.com/2020/03/rls02-update2-1.png" alt="RLS02-update2" width="908" height="376" />
<h2>Lab Work :</h2>
Step 1:Create three user accounts that will demonstrate different access capabilities.
<pre>CREATE USER Manager WITHOUT LOGIN;  
CREATE USER SalesTeamDhaka WITHOUT LOGIN;  
CREATE USER SalesTeamKhulna WITHOUT LOGIN;</pre>
Step 2:Make a Sales table for demonstration
<pre>CREATE TABLE Sales  
    (  
    OrderID int,  
    UserRole sysname,  
    Product varchar(10),  
    Sales_Amount Numeric(9,2)  
    );</pre>
Step 3:Populate Sample data to Sales Table
<pre>INSERT INTO Sales VALUES (1, 'SalesTeamDhaka', 'Valve', 500000);
INSERT INTO Sales VALUES (2, 'SalesTeamKhulna', 'Wheel', 20000);
INSERT INTO Sales VALUES (3, 'SalesTeamDhaka', 'Valve', 800000);
INSERT INTO Sales VALUES (4, 'SalesTeamDhaka', 'Bracket', 60000);
INSERT INTO Sales VALUES (5, 'SalesTeamKhulna', 'Wheel', 90000);
INSERT INTO Sales VALUES (6, 'SalesTeamKhulna', 'Seat', 50000);
  
SELECT * FROM Sales;</pre>
Step 4:Grant access on Sales table
<pre>GRANT SELECT ON Sales TO Manager;  
GRANT SELECT ON Sales TO SalesTeamDhaka;  
GRANT SELECT ON Sales TO SalesTeamKhulna;  
GO</pre>
<b>Step 5: </b>New schema has created along with an inline table-valued function which returns 1 when a row in the UserRole column is the same as the user executing the query.If the user executing the query is the Manager user (@UserRole = USER_NAME()) or (USER_NAME() = 'Manager').
<pre>CREATE SCHEMA Security;  
GO  
CREATE FUNCTION Security.fn_securitypredicate(@UserRole AS sysname)  
    RETURNS TABLE  
WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result
WHERE @UserRole = USER_NAME() OR USER_NAME() = 'Manager';</pre>
<b>Step 6: </b>Now a security policy adding the function as a filter predicate and it state must be set to ON to enable the policy.
<pre>Create SECURITY POLICY SalesFilter  
ADD FILTER PREDICATE Security.fn_securitypredicate(UserRole)
ON dbo.Sales  WITH (STATE = ON);
Go
SELECT * FROM sys.security_policies
GO
SELECT * FROM sys.security_predicates
GO</pre>
<b>Step 7: </b>Allow SELECT permissions to the fn_securitypredicate function

GRANT SELECT ON security.fn_securitypredicate TO Manager;
GRANT SELECT ON security.fn_securitypredicate TO SalesTeamDhaka;
GRANT SELECT ON security.fn_securitypredicate TO SalesTeamKhulna;

<b>Step 8:</b>It's time to test the filtering predicate
<pre>EXECUTE AS USER = 'SalesTeamDhaka'; 
SELECT * FROM Sales;
REVERT; 
GO 
EXECUTE AS USER = 'SalesTeamKhulna'; 
SELECT * FROM Sales;
REVERT; 
GO
EXECUTE AS USER = 'Manager'; 
SELECT * FROM Sales;
REVERT;</pre>
<h2>Limitations :</h2>
<ul>
	<li>It doesn’t allow you to generate an indexed view of a table that has a defined security policy.</li>
	<li>It is not compatible with Polybase and FileStream.</li>
	<li>RLS being a function can fetch instances when the queries are re-written.</li>
	<li>Due to some side channel attacks, some data values for particular rows may be determined even without the right access.</li>
</ul>
As a conclusion, it can be stated that by implanting RLS you can easily put on record level security rules within your database design. This smart feature of SQL Server 2016 can help build a solution to different business problems of small to medium sized businesses by creating a security policy with utmost ease.
