---
layout: post
title: First general availability (GA) release of SSDT for Visual Studio 2017 (15.5.0)
date: 2017-12-19 22:44
author: techforumugm
comments: true
categories: [Business Intelligence, MS SQLServer]
---
First general availability (GA) release of SSDT for Visual Studio 2017 (15.5.0) has published.Now design and deploy SQL Server content type :Database Projects, Schema Compare, Data Compare, SQL Server Object Explorer and more in way like develop an application in Visual Studio.Analysis Services and Reporting Services BI project types is available in the VS2017 Gallery.

Developers can leverage SQL Server Data Tools (SSDT) included in all Visual Studio editions. Additionally on Visual Studio 2017, developers can leverage Redgate Data Tools. SQL Search is available in all editions, and SQL Prompt Core and ReadyRoll Core are available for VS 2017 Enterprise subscribers.
<ul>
	<li><strong>SQL Server Data Tools (SSDT)</strong> turns Visual Studio into a powerful development environment for SQL Server, Azure SQL Database and Azure SQL Data Warehouse. With SSDT, developers can visually design, build, debug, test, maintain, refactor, deploy, source control and enable continuous integration &amp; continuous deployment for their databases with a declarative model that spans all the phases of database development. Developers can work offline with a database project, or directly with a connected database instance in Azure SQL Database, Azure SQL Data Warehouse, and SQL Server running on Windows, Linux, Docker and in Azure or any cloud.</li>
	<li>With <strong>ReadyRoll Core</strong>, users can develop, source control, and safely automate deployments of database changes, alongside application changes. This means the same tools used for application development can be utilized for database development and deployment, and ensures a single source of truth for both application and database changes. ReadyRoll’s migrations-based approach gives developers more control over the end database deployment script and can be easily integrated into DevOps processes, such as continuous integration and continuous delivery.</li>
	<li>With <strong>SQL Prompt Core</strong> users can increase productivity with advanced IntelliSense-style code completion in Visual Studio.</li>
	<li><strong>SQL Search</strong> allows users to quickly search for SQL objects across databases. Together, Redgate Data Tools help to ensure database development is not the bottleneck to continuously delivering value to end users.</li>
</ul>
Benefits of developing database with SSDT is you can easily integrate Application Lifecycle Management (ALM) practices to database development. You can develop and manage your database in source control such as Git, automate build with continuous integration and orchestrate releases with continuous deployment.  <a href="https://blogs.msdn.microsoft.com/ssdt/2016/04/06/sqldb-cicd-intro/">Details</a>.

In this blog, we will walk through a series of end-to-end scenarios of continuous integration and deployment. After playing through these scenarios, you will have a complete build and deployment automation setup for your Azure SQL Database from where you can venture into more advanced techniques. For simplicity we will use Visual Studio Team Services and Azure

The Data Storage and Processing workload is optimized for DB developers, but SSDT is a recommended option in most other workloads including ASP.Net and Web Development. It's a standalone web installation experience for SQL Server Database, Analysis Services, Reporting Services, and Integration Services projects in Visual Studio 2017 15.5 or later.

<strong>System requirements :</strong>

Windows 7 SP1, Windows 8.1 or Windows Server 2012 R2, Windows 10 or Windows Server 2016.

Available Languages - SSDT for VS 2017
<p class="lf-text-block lf-block">This release of <strong>SSDT for VS 2017</strong> can be installed in the following languages:</p>
<p class="lf-text-block lf-block">Chinese (People's Republic of China) | Chinese (Taiwan) | English (United States) | French |German | Italian | Japanese | Korean | Portuguese (Brazil) | Russian | Spanish</p>
<strong>Supported SQL versions</strong>
<table>
<tbody>
<tr>
<td class="">Relational databases</td>
<td>SQL Server 2005* - SQL Server 2017

Azure SQL Database

Azure SQL Data Warehouse (supports queries only; database projects are not yet supported)</td>
</tr>
</tbody>
</table>
You can now download it free. <a href="https://docs.microsoft.com/en-us/sql/ssdt/download-sql-server-data-tools-ssdt">Download Link</a>
<h3>Installation Steps :</h3>
[gallery ids="872,873,874,875,876,877" type="rectangular"]

&nbsp;
<h3>Start with Visual Studio 2017 :</h3>
Step 1 : Create New Project using Visual Studio 2017 .Select SQL Server &gt; SQL Server Database Project.

<img class="alignnone  wp-image-884" src="https://techforumugm.files.wordpress.com/2017/12/v1.png" alt="v1" width="604" height="301" />

Step 2 : Now with the Project you can add database objects tables, View , User defined function,Stored Procedure etc as New items .

<img class="alignnone  wp-image-885" src="https://techforumugm.files.wordpress.com/2017/12/v2.png" alt="v2" width="504" height="477" />

Step 3 : All objects and script files can be addable

<img class="alignnone  wp-image-886" src="https://techforumugm.files.wordpress.com/2017/12/v3.png" alt="v3" width="561" height="345" />

Step 4 : It can be possible Write SQL statement and executable .

<img class="alignnone  wp-image-887" src="https://techforumugm.files.wordpress.com/2017/12/v4.png" alt="v4" width="558" height="450" />

Have a Nice Day !
