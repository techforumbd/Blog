---
layout: post
title: SQL Server: WHAT-WHY-HOW: "Natively Compiled Stored Procedures"
date: 2019-09-14 01:03
author: techforumugm
comments: true
categories: [MS SQLServer]
---
&nbsp;
<h2 class="MsoNormal" style="margin:0 0 8pt;"><span style="font-family:Calibri;">WHAT </span></h2>
&nbsp;
<div class="MsoNormal" style="margin:0 0 8pt;"><span style="font-family:Calibri;">SQL Server 2014 introduced native compiled stored procedure. In this process code are converted to <b>machine code</b> that stored into DLL files stored in a specific folder of SQL Server. Memory optimized Machine codes can be directly executed by processor without further compilation or interpretation. So it is faster than T-SQL stored procedure that we are using. Memory optimized tables are only accessed by natively compiled stored procedure not disk based tables.</span></div>
&nbsp;
<div class="MsoNormal" style="margin:0 0 8pt;"><a href="http://blogs.msdn.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-01-60-33/8371.3.png"><span style="color:blue;text-decoration:none;"><!-- [if gte vml 1]&gt;                              &lt;![endif]--><!-- [if !vml]--><!--[endif]--></span></a></div>
<div class="separator" style="clear:both;text-align:center;"><a style="margin-left:1em;margin-right:1em;" href="https://techforumugm.files.wordpress.com/2014/07/15949-in-memorystoreadprocedure.jpg"><img src="https://techforumugm.files.wordpress.com/2014/07/15949-in-memorystoreadprocedure.jpg" width="320" height="295" border="0" /></a></div>
<span style="font-family:Calibri;"> </span>
<h2 class="MsoNormal" style="margin:0 0 8pt;"><span style="font-family:Calibri;">Why </span></h2>
&nbsp;
<div class="MsoListParagraphCxSpFirst" style="margin:0 0 0 .5in;text-indent:-.25in;"><!-- [if !supportLists]--><span style="font-family:Calibri;">1.</span>       <!--[endif]--><span style="font-family:Calibri;">“Natively compiled stored procedure” is compiled when it is created. </span></div>
&nbsp;
<div class="MsoListParagraphCxSpMiddle" style="margin:0 0 0 .5in;text-indent:-.25in;"><!-- [if !supportLists]--><span style="font-family:Calibri;">2.</span>       <!--[endif]-->Easy to identify error like arithmetic overflow, type conversion, and divide-by-zero conditions when they are created.</div>
&nbsp;
<div class="MsoListParagraphCxSpMiddle" style="margin:0 0 0 .5in;text-indent:-.25in;"><!-- [if !supportLists]--><span style="font-family:Calibri;">3.</span>       <!--[endif]-->Faster and more efficient data access is occurred because of native compilation.</div>
&nbsp;
<div class="MsoListParagraphCxSpMiddle" style="margin:0 0 0 .5in;text-indent:-.25in;"><!-- [if !supportLists]--><span style="font-family:Calibri;">4.</span>       <!--[endif]-->‘Natively Compiled Stored Procedure’ body must be consisting of exactly one atomic block.</div>
&nbsp;
<div class="MsoListParagraphCxSpLast" style="margin:0 0 8pt .5in;text-indent:-.25in;">

<!-- [if !supportLists]--><span style="font-family:Calibri;">5.</span>       <!--[endif]-->‘Natively Compiled Stored Procedure’ <span lang="EN">can interact very efficiently with the In-Memory storage engine.</span>
<h2>

<span style="color:#424242;font-family:Segoe UI;">How:</span></h2>
<div class="reCodeBlock" style="-ms-overflow-y:auto;border:1px solid #7f9db9;">
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:#008200;">--Step 1:  Database Creation  </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:#008200;">---------------------------------------------------------------  </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:black;">USE [master]  </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:black;">GO  </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:#006699;font-weight:bold;">CREATE</code> <code style="color:#006699;font-weight:bold;">DATABASE</code> <code style="color:black;">[DBInMemoryOLTP] </code></span></div>
<div style="background-color:#f8f8f8;"><code> </code><span style="margin-left:3px !important;"><code style="color:black;">CONTAINMENT = NONE  </code></span></div>
<div style="background-color:white;"><code> </code><span style="margin-left:3px !important;"><code style="color:#006699;font-weight:bold;">ON</code>  <code style="color:#006699;font-weight:bold;">PRIMARY</code> </span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:black;">( </code><code style="color:#006699;font-weight:bold;">NAME</code> <code style="color:black;">=  N</code><code style="color:blue;">'DBInMemoryOLTP_data'</code><code style="color:black;">, FILENAME = N</code><code style="color:blue;">'c:\database\DBInMemoryOLTP_data.mdf'</code>  <code style="color:black;">),  </code></span></div>
<div style="background-color:white;"><code> </code><span style="margin-left:3px !important;"><code style="color:black;">FILEGROUP  [DBInMemoryOLTP_data] </code><code style="color:#006699;font-weight:bold;">CONTAINS</code> <code style="color:black;">MEMORY_OPTIMIZED_DATA  </code><code style="color:#006699;font-weight:bold;">DEFAULT</code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:black;">( </code><code style="color:#006699;font-weight:bold;">NAME</code> <code style="color:black;">=  N</code><code style="color:blue;">'DBInMemoryOLTP_FG1'</code><code style="color:black;">, FILENAME = N</code><code style="color:blue;">'c:\database\DBInMemoryOLTP_FG1'</code> <code style="color:black;">, MAXSIZE = UNLIMITED), </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:black;">( </code><code style="color:#006699;font-weight:bold;">NAME</code> <code style="color:black;">=  N</code><code style="color:blue;">'DBInMemoryOLTP_FG2'</code><code style="color:black;">, FILENAME = N</code><code style="color:blue;">'c:\database\DBInMemoryOLTP_FG2'</code> <code style="color:black;">, MAXSIZE = UNLIMITED) </code></span></div>
<div style="background-color:#f8f8f8;"><code> </code><span style="margin-left:3px !important;"><code style="color:black;">LOG </code><code style="color:#006699;font-weight:bold;">ON</code> </span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:black;">( </code><code style="color:#006699;font-weight:bold;">NAME</code> <code style="color:black;">=  N</code><code style="color:blue;">'DBInMemoryOLTP_log'</code><code style="color:black;">, FILENAME = N</code><code style="color:blue;">'C:\database\DBInMemoryOLTP_log.ldf'</code> <code style="color:black;">) </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:black;">GO  </code></span></div>
<div style="background-color:white;"><code> </code><span style="margin-left:3px !important;"> </span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:#008200;">--Step 2: Now  we are going to create a memory-optimized table: </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:#008200;">---------------------------------------------------------------------  </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:black;">Use  [DBInMemoryOLTP] </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:black;">Go  </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:#006699;font-weight:bold;">CREATE</code> <code style="color:#006699;font-weight:bold;">TABLE</code> <code style="color:black;">[techforum_member_list](  </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:black;">[TfmID]  </code><code style="color:#006699;font-weight:bold;">INT</code> <code style="color:grey;">NOT</code> <code style="color:grey;">NULL</code> <code style="color:#006699;font-weight:bold;">PRIMARY</code> <code style="color:#006699;font-weight:bold;">KEY</code> <code style="color:black;">NONCLUSTERED HASH </code><code style="color:#006699;font-weight:bold;">WITH</code> <code style="color:black;">(BUCKET_COUNT = 500000),  </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:black;">[</code><code style="color:#006699;font-weight:bold;">Name</code><code style="color:black;">]  NVARCHAR(50) </code><code style="color:#006699;font-weight:bold;">COLLATE</code> <code style="color:black;">Latin1_General_100_BIN2 </code><code style="color:grey;">NOT</code> <code style="color:grey;">NULL</code> <code style="color:#006699;font-weight:bold;">INDEX</code> <code style="color:black;">[IName] HASH </code><code style="color:#006699;font-weight:bold;">WITH</code> <code style="color:black;">(BUCKET_COUNT = 500000),  </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:black;">[JoiningDate]  DATETIME </code><code style="color:grey;">NULL</code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:black;">)   </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:#006699;font-weight:bold;">WITH</code> <code style="color:black;">(MEMORY_OPTIMIZED = </code><code style="color:#006699;font-weight:bold;">ON</code><code style="color:black;">,  DURABILITY = SCHEMA_AND_DATA);  </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:#008200;">--Sample data  input </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:#006699;font-weight:bold;">Declare</code> <code style="color:black;">@i </code><code style="color:#006699;font-weight:bold;">as</code> <code style="color:#006699;font-weight:bold;">bigint</code><code style="color:black;">=1   </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:black;">While ( @i &lt;  50)  </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:#006699;font-weight:bold;">Begin</code></span></div>
<div style="background-color:#f8f8f8;"><code>    </code><span style="margin-left:12px !important;"><code style="color:#006699;font-weight:bold;">Insert</code> <code style="color:#006699;font-weight:bold;">into</code> <code style="color:black;">techforum_member_list </code><code style="color:#006699;font-weight:bold;">values</code> <code style="color:black;">(@i,</code><code style="color:blue;">'techforum'</code><code style="color:black;">+</code><code style="color:deeppink;">cast</code><code style="color:black;">(@i </code><code style="color:#006699;font-weight:bold;">as</code> <code style="color:black;">nvarchar(50)),GETDATE() )  </code></span></div>
<div style="background-color:white;"><code>    </code><span style="margin-left:12px !important;"><code style="color:#006699;font-weight:bold;">Set</code> <code style="color:black;">@i+=1  </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:#006699;font-weight:bold;">End</code> </span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:#008200;">--Featch data  from table </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:#006699;font-weight:bold;">Select</code> <code style="color:black;">*  </code><code style="color:#006699;font-weight:bold;">from</code> <code style="color:black;">techforum_member_list</code></span></div>
</div>
&nbsp;
<div class="reCodeBlock" style="-ms-overflow-y:auto;border:1px solid #7f9db9;">
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:black;">--Step 3: Now we  are going to create </code><code style="color:blue;">'Natively Compiled Stored  Procedures'</code><code style="color:black;">: </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:black;">------------------------------------------------------------------------------  </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:black;">USE  [DBInMemoryOLTP] </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:black;">GO  </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:black;">CREATE PROCEDURE  Usp_Member_Profile  </code></span></div>
<div style="background-color:#f8f8f8;"><code> </code><span style="margin-left:3px !important;"><code style="color:black;">(  @TfmID int NOT  NULL) </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:black;">WITH EXECUTE AS  OWNER, SCHEMABINDING, NATIVE_COMPILATION </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:black;">AS  </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:black;">BEGIN ATOMIC WITH  (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N</code><code style="color:blue;">'us_english'</code><code style="color:black;">)  </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:black;">SELECT [TfmID]  </code></span></div>
<div style="background-color:white;"><code>      </code><span style="margin-left:18px !important;"><code style="color:black;">,[Name]  </code></span></div>
<div style="background-color:#f8f8f8;"><code>      </code><span style="margin-left:18px !important;"><code style="color:black;">,[JoiningDate]  </code></span></div>
<div style="background-color:white;"><code>  </code><span style="margin-left:6px !important;"><code style="color:black;">FROM  [dbo].[techforum_member_list] </code></span></div>
<div style="background-color:#f8f8f8;"><code>     </code><span style="margin-left:15px !important;"><code style="color:black;">WHERE TfmID =  @TfmID </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:deeppink;">END</code><code style="color:black;">; </code></span></div>
<div style="background-color:#f8f8f8;"><code> </code><span style="margin-left:3px !important;"> </span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:black;">GO  </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:black;">--Step 4: Execute  </code><code style="color:blue;">'Natively Compiled Stored  Procedures'</code><code style="color:black;">: </code></span></div>
<div style="background-color:white;"><span style="margin-left:0 !important;"><code style="color:black;">------------------------------------------------------------------------------  </code></span></div>
<div style="background-color:#f8f8f8;"><span style="margin-left:0 !important;"><code style="color:deeppink;">Exec</code> <code style="color:black;">Usp_Member_Profile </code><code style="color:blue;">'10'</code></span></div>
</div>
<span style="font-family:Calibri;font-size:14px;">

</span>
<div class="MsoNormal" style="line-height:normal;margin:0 0 8pt;text-indent:-.25in;"><!-- [if !supportLists]--><span style="font-family:Wingdings;font-size:10pt;">§  </span><!--[endif]--><span style="font-size:small;">NATIVE_COMPILATION: stored Procedure Needs to be compiled to Native Code during it’s creation.</span></div>
&nbsp;
<div class="MsoNormal" style="line-height:normal;margin:0 0 8pt;text-indent:-.25in;"><!-- [if !supportLists]--><span style="font-family:Wingdings;font-size:10pt;">§  </span><!--[endif]--><span style="font-size:small;">SCHEMABINDING: This line of code dropping of the Tables referenced by the Stored Procedure</span></div>
&nbsp;
<div class="MsoNormal" style="line-height:normal;margin:0 0 8pt;text-indent:-.25in;"><!-- [if !supportLists]--><span style="font-family:Wingdings;font-size:10pt;">§  </span><!--[endif]--><span style="font-size:small;">EXECUTE AS OWNER: The context in which the Stored Procedure Should Execute. It can be EXECUTE AS OWNER, EXECUTE AS user and EXECUTE AS SELF</span></div>
&nbsp;
<div class="MsoNormal" style="line-height:normal;margin:0 0 8pt;text-indent:-.25in;"><!-- [if !supportLists]--><span style="font-family:Wingdings;font-size:10pt;">§  </span><!--[endif]--><span style="font-size:small;">BEGIN ATOMIC: Guarantees the Atomic Execution (Implicit Transaction) of the Stored Procedure (i.e. Either All Statement’s will succeed or fail together)</span></div>
&nbsp;
<div class="MsoNormal" style="line-height:normal;margin:0 0 8pt;text-indent:-.25in;"><!-- [if !supportLists]--><span style="font-family:Wingdings;font-size:10pt;">§  </span><!--[endif]--><span style="font-size:small;">WITH ( TRANSACTION ISOLATION LEVEL = SNAPSHOT,  LANGUAGE = ‘us_english’ )
TRANSACTION ISOLATION LEVEL: Transaction Isolation Level used for the Atomic Execution of the Stored Procedure.
LANGUAGE: Language used for the Date Time Formats and System Messages in the Stored Procedure</span></div>
&nbsp;

&nbsp;

</div>
