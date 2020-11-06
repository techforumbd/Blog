---
layout: post
title: Application on Windows Azure
date: 2012-05-10 18:00
author: techforumugm
comments: true
categories: [Microsoft Azure]
---
<div class="MsoNormal" style="margin:0 0 10pt;"><span>At the top level Windows Azure provide two services platform to run application and storage facility to store data. In Windows Azure environment application’s each instance running on an individual virtual machine .Virtual machine runs 64 bit Windows 2008 and they provided by Hypervisor which is specially designed for Cloud .But it is not possible deploy own virtual machine image to Azure. Windows Azure provide service platform, using that .NET 3.5 Application using different roles.</span></div><div class="MsoNormal" style="margin:0 0 10pt;"><span>Web role instance work on HTTP request and implemented through ASP.NET, WCF or another .NET framework and IIS 7 .There is a load balancer which work on all instance to make efficient request handling. Worker role responsible for batch processing taking input from web roles.Each role is run on different Virtual machine .Each Virtual machine contain Windows Azure agent which allow to connect it with Windows Azure Fabric.</span></div><div class="MsoNormal" style="margin:0 0 10pt;"><span>An application may require much computing resource at specific time whenever any batch process is<span>  </span>going to run or specific time whenever much user going to access the application .Windows Azure provide the scale up facility .When ever scale up facility require then Windows Azure fabric will then spin up new VMs and start running new instance .</span></div>
