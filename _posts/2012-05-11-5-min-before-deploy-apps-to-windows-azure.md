---
layout: post
title: 5 min Before Deploy Apps to Windows Azure
date: 2012-05-11 17:44
author: techforumugm
comments: true
categories: [Microsoft Azure]
---
<div class="MsoNormal" style="margin:0 0 10pt;"><span style="font-family:Calibri;">Thinks need to be aware before deploy application on Windows Azure:</span></div><ol style="margin-top:0;" type="1"><li class="MsoNormal" style="margin:0 0 10pt;"><span style="font-family:Calibri;">Application develop in house can’t be deployed on Azure.</span></li><li class="MsoNormal" style="margin:0 0 10pt;"><span style="font-family:Calibri;">Storage structure in Azure is different ,Windows Azure storage can keep non relational data </span></li></ol><div class="MsoNormal" style="margin:0 0 10pt .5in;"><span style="font-family:Calibri;">SQL Azure work on relational data but it is quite differing from SQL Server.</span></div><ol start="3" style="margin-top:0;" type="1"><li class="MsoNormal" style="margin:0 0 10pt;"><span style="font-family:Calibri;">Web role instance is stateless; state is written to Azure Storage or passed back to client as cookie.</span></li><li class="MsoNormal" style="margin:0 0 10pt;"><span style="font-family:Calibri;">Specific user’s requests are not supplied to same web role instance. Web role and worker role are implemented through .NET technology.</span></li></ol>
