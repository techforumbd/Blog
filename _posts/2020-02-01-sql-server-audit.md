---
layout: post
title: SQL Server Audit
date: 2020-02-01 02:01
author: techforumugm
comments: true
categories: [Business Intelligence]
---
<h2>What is Auditing ?</h2>
Auditing is used for tracking and logging events on a single instance or individual database .Through server audit specifications server level events and database audit specifications for database level events are captured into event logs or audit files.

Components :

audit is the combination of several elements into a single package for a specific group of server actions or database actions. The components of SQL Server audit combine to produce an output that is called an audit, just as a report definition combined with graphics and data elements produces a report.
<table width="488">
<tbody>
<tr>
<td width="133">SQL Server Audit</td>
<td width="148">Server Audit Specification</td>
<td width="207">Database Audit Specification</td>
</tr>
<tr>
<td width="133">SQL Server Audit object collects a single instance of server or database-level actions and groups of actions to monitor. The audit is at the SQL Server instance level. You can have multiple audits per SQL Server instance.</td>
<td width="148">Server Audit Specification object belongs to an audit. You can create one server audit specification per audit, because both are created at the SQL Server instance scope.</td>
<td width="207">The Database Audit Specification object also belongs to a SQL Server audit. You can create one database audit specification per SQL Server database per audit.</td>
</tr>
<tr>
<td width="133"></td>
<td width="148">The server audit specification collects many server-level action groups raised by the Extended Events feature. You can include audit action groups in a server audit specification. Audit action groups are predefined groups of actions, which are atomic events occurring in the Database Engine. These actions are sent to the audit, which records them in the target.</td>
<td width="207">The database audit specification collects database-level audit actions raised by the Extended Events feature. You can add either audit action groups or audit events to a database audit specification. Audit events are the atomic actions that can be audited by the SQL Server engine. Audit action groups are predefined groups of actions.</td>
</tr>
<tr>
<td width="133"></td>
<td width="148"></td>
<td width="207">Both are at the SQL Server database scope. These actions are sent to the audit, which records them in the target.</td>
</tr>
</tbody>
</table>
<h2><b>How is the SQL Server Audit works ?</b></h2>
<ul>
	<li>A SQL Server Audit object can be written to by one Server Audit Speciﬁcation and one Database Audit Speciﬁcation per database.</li>
	<li>A SQL Server Audit can belong to only one SQL Server instance, but there may be several SQL Server Audits within an instance.</li>
	<li>A Server Audit Speciﬁcation deﬁnes which server-level events will be captured and passed to the SQL Audit.</li>
	<li>A Database Audit Speciﬁcation deﬁnes which database-level events are captured and passed to the SQL Audit.</li>
	<li>Both Server Audit Speciﬁcations and Database Audit Speciﬁcations can deﬁ ne sets of events or groups to be captured. Event groups encapsulate a number of related events. Database actions include select, insert, update, and delete, and they capture the user context and the entire DML query.</li>
	<li>The audited data includes user context information.</li>
	<li>The SQL Server Audit sends all the captured events to a single target: a ﬁ le, the Windows Security event log (not in Windows XP), or the Windows Application event log. The Management Studio SQL Audit UI includes a tool for browsing the audit logs.</li>
	<li>SQL Server Audits, Server Audit Speciﬁcations, and Database Audit Speciﬁcations can all be created and managed either with Object Explorer or by using T-SQL.</li>
	<li>SQL Server Audits, Server Audit Speciﬁcations, and Database Audit Speciﬁcations can all be enabled or disabled. They may be modiﬁed only while disabled. All are disabled by default when they are ﬁrst created because that’s how Extended Events works.</li>
	<li>SQL Server Audits, Server Audit Speciﬁcations, and Database Audit Speciﬁcations can all be managed by Policy-Based Management.</li>
</ul>
SQL Audits are serious. The SQL Server Audit object can be conﬁgured to shut down the server if the audit doesn’t function properly
<h3>Configuration Using Management Studio:</h3>
he SQL Server Audit feature can be set up using either T-SQL, or SQL Server Management Studio options.To configure the feature using SQL Server Management Studio:
<ol>
	<li>To create a <b>SQL Server Audit</b> object, expand the <b>Security</b> folder in <b>Object Explorer</b></li>
	<li>Expand the <b>SQL Server Logs</b> folder</li>
	<li>Select <b>New Audit</b></li>
</ol>
<img class="alignnone size-full wp-image-1287" src="https://techforumugm.files.wordpress.com/2020/04/sa01.png" alt="SA01" width="361" height="322" />

4.  In the <b>Create Audit</b> dialog, specify the audit name, audit destination, and path

&nbsp;
