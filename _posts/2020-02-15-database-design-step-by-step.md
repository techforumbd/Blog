---
layout: post
title: Database Design Step by Step
date: 2020-02-15 14:19
author: techforumugm
comments: true
categories: [MS SQLServer]
---
<h2>Database Design Life Cycle</h2>
The database development life cycle has a number of stages that are followed when developing database systems. But on small database systems, the database system development life cycle is usually very simple and does not involve all of the steps.

<img class="alignnone size-full wp-image-1138" src="https://techforumugm.files.wordpress.com/2020/03/dd01.png" alt="DD01" width="1143" height="493" />
<h2>Data Modelling</h2>
Data Model is like architect's building plan. Data modeling is the analysis of data objects and their relationships. Data modeling is the first step in database design. Data modeling involves a progression from conceptual model to logical model to physical schema. Process modeling – a technique used to organize and document a system’s processes. It textually describes how the outputs are generated from the inputs. Flow of data through processes Logic, Policies &amp; Procedures. Data model emphasizes on : <u>what</u> data is needed and <u>how</u> it should be organized instead of what <u>operations need to be performed </u>on the data.

Two types of Data Models techniques are :
<ul>
	<li>Entity Relationship (E-R) Model</li>
	<li>UML (Unified Modelling Language)</li>
</ul>
Data modeling helps in the :
<ul>
	<li>Visual representation of data</li>
	<li>Helpful to identify missing and redundant data.</li>
	<li>Helps design the database at the conceptual, logical levels and physical.</li>
	<li>Enforces business rules, regulatory compliances, and government policies on the data.</li>
	<li>Data models ensure consistency in naming conventions,</li>
	<li>Default values, semantics, security while ensuring quality of the data.</li>
</ul>
&nbsp;

<img class="alignnone size-full wp-image-1141" src="https://techforumugm.files.wordpress.com/2020/03/dd02.png" alt="DD02" width="535" height="410" />
<h2>Conceptual Model</h2>
Conceptual data models known as Domain models create a common vocabulary for all stakeholders by establishing basic concepts and scope. The conceptual model is developed independently of hardware specifications like data storage capacity, location or software specifications like DBMS vendor and technology. Conceptual Modeling is Important
<ul>
	<li>Effective Communication Tool</li>
	<li>User involvement</li>
	<li>Independence from a particular DBMS</li>
	<li>Documentation</li>
</ul>
Step 1 :

Discovering and analyzing organizational and users data requirements

What data is important ?

What data should be maintained ?

Constructing a data model (Entity-Relationship Diagram).

Step 2:

The main aim of this model is to establish the entities, their attributes, and their relationships. The 3 basic tenants of Data Model are

Entity: A real-world thing

Attribute: Characteristics or properties of an entity

Relationship: Dependency or association between two entities

<img class="alignnone size-full wp-image-1143" src="https://techforumugm.files.wordpress.com/2020/03/dd03.png" alt="DD03" width="620" height="259" />
<h2>Logical Data Model</h2>
Logical data models defines the structure of the data elements and set the relationships between them. At this Data modeling level, need to verify and adjust the connector details that were set earlier for relationships. But no primary or secondary key is defined. Logical design adapts the conceptual design to a specific DBMS implementation model. Logical data model is to provide a foundation to form the base for the Physical model.

The logical design is software-dependent.
<ul>
	<li>Logical Models</li>
	<li>Relational Model</li>
	<li>Network Model</li>
	<li>Hierarchical Model</li>
</ul>
Characteristics of a Logical data model
<ul>
	<li>A logical data model will normally be derived conceptual data model.</li>
	<li>Entities and attributes will have definitions. Data attributes will typically have datatypes with precisions and lengths assigned.</li>
	<li>Data attributes will have nullability (optionality) assigned.</li>
	<li>Contains relationships between entities that address cardinality.</li>
	<li>Describes data requirements for a single project or major subject area.</li>
	<li>May be integrated with other logical data models via a repository of shared entities</li>
</ul>
&nbsp;

<img class="alignnone size-full wp-image-1147" src="https://techforumugm.files.wordpress.com/2020/02/dd04.png" alt="DD04" width="538" height="168" />
<h2>Physical Data Model</h2>
A physical data model is a fully-attributed data model that is dependent upon a specific version of a data persistence technology.  The target implementation technology may be a relational DBMS. Physical data model also helps to visualize database structure. It helps to create database columns keys, constraints, indexes, triggers, and other RDBMS features. Characteristics of a physical data model:

Designed and developed are depending on a specific version of a DBMS,

data storage location or technology.

Data Model contains relationships between tables which addresses

cardinality and nullability of the relationships.

Columns should have exact data types, lengths assigned and default values.

Primary and Foreign keys, views, indexes, access profiles, and authorizations,

etc. are defined.

Developed for a specific version of a DBMS, location, data storage or

technology to be used in the project.

<img class="alignnone size-full wp-image-1149" src="https://techforumugm.files.wordpress.com/2020/02/dd05.png" alt="DD05" width="558" height="190" />
<h2>Summary</h2>
<img class="alignnone size-full wp-image-1151" src="https://techforumugm.files.wordpress.com/2020/02/dd06.png" alt="DD06" width="849" height="465" />
<h3>Diagrammatic comparison:</h3>
Data modeling, <a href="https://www.1keydata.com/datawarehousing/conceptual-data-model.html">conceptual data model</a>, <a href="https://www.1keydata.com/datawarehousing/logical-data-model.html">logical data model</a>, and <a href="https://www.1keydata.com/datawarehousing/physical-data-model.html">physical data model</a>

<img class="alignnone size-full wp-image-1152" src="https://techforumugm.files.wordpress.com/2020/02/dd07.png" alt="DD07" width="1157" height="339" />
<h3>Comparison :</h3>
<table width="571">
<tbody>
<tr>
<td width="184">Feature</td>
<td width="142">Conceptual</td>
<td width="95">Logical</td>
<td width="150">Physical</td>
</tr>
<tr>
<td width="184">Entity Names</td>
<td width="142">✓</td>
<td width="95">✓</td>
<td width="150">&nbsp;</td>
</tr>
<tr>
<td width="184">Entity Relationships</td>
<td width="142">✓</td>
<td width="95">✓</td>
<td width="150">&nbsp;</td>
</tr>
<tr>
<td width="184">Attributes</td>
<td width="142">&nbsp;</td>
<td width="95">✓</td>
<td width="150">&nbsp;</td>
</tr>
<tr>
<td width="184">Primary Keys</td>
<td width="142">&nbsp;</td>
<td width="95">✓</td>
<td width="150">✓</td>
</tr>
<tr>
<td width="184">Foreign Keys</td>
<td width="142">&nbsp;</td>
<td width="95">✓</td>
<td width="150">✓</td>
</tr>
<tr>
<td width="184">Table Names</td>
<td width="142">&nbsp;</td>
<td width="95">&nbsp;</td>
<td width="150">✓</td>
</tr>
<tr>
<td width="184">Column Names</td>
<td width="142">&nbsp;</td>
<td width="95">&nbsp;</td>
<td width="150">✓</td>
</tr>
<tr>
<td width="184">Column Data Types</td>
<td width="142">&nbsp;</td>
<td width="95">&nbsp;</td>
<td width="150">✓</td>
</tr>
</tbody>
</table>
<h2>Best Practice</h2>
<u>Documentation</u>
<ul>
	<li>SQL development standards</li>
	<li>Database source controls.</li>
	<li>Plan for table partition</li>
	<li>Specific number of columns</li>
	<li>Avoid sub queries.</li>
	<li>Not to use ’LIKE’</li>
	<li>Keep passwords as encrypted for security</li>
</ul>
<u>Implementation </u>
<ul>
	<li>Role based authentication</li>
	<li>Keep your stored procedure encrypted.</li>
	<li>Use disaster recovery like failover clustering, auto backups, replication etc.</li>
	<li>Security and user permissions.</li>
	<li>Split over a number of files</li>
	<li>Place data and log files on different drives</li>
	<li>Enable Auto Grow</li>
	<li>Disable Auto Shrink</li>
	<li>Security and user permissions.</li>
</ul>
<h2>Conclusion</h2>
<ul>
	<li>Data modeling is the process of developing data model for the data to be stored in a Database.</li>
	<li>Data Models ensure consistency in naming conventions, default values, semantics, security while ensuring quality of the data.</li>
	<li>Data Model structure helps to define the relational tables, primary and foreign keys and stored procedures.</li>
	<li>There are three types of conceptual, logical, and physical.</li>
	<li>The main aim of conceptual model is to establish the entities, their attributes, and their relationships.</li>
	<li>Logical data model defines the structure of the data elements and set the relationships between them.</li>
	<li>A Physical Data Model describes the database specific implementation of the data model.</li>
	<li>The main goal of a designing data model is to make certain that data objects offered by the functional team are represented accurately.</li>
	<li>The biggest drawback is that even smaller change made in structure require modification in the entire application.</li>
</ul>
