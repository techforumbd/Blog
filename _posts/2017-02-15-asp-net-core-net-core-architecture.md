---
layout: post
title: ASP NET CORE | .NET Core Architecture
date: 2017-02-15 15:57
author: techforumugm
comments: true
categories: [ASP.NET]
---
What is ASP.NET Core?
<ol>
	<li>.NET Core is a new cross-platform (Windows, Mac and Linux) and open source version of .NET</li>
	<li>Targeting Cloud and Mobile scenarios. It is the basis of ASP.NET 5 and .NET UWP apps.</li>
	<li>Deployable on the cloud or run on-premises</li>
</ol>
Why ASP.NET Core?
<ol>
	<li>Modular components with minimal overhead and retain flexibility</li>
	<li>cloud-ready environment-based <a href="https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration">configuration system</a></li>
	<li>New light-weight and modular HTTP request pipeline</li>
	<li>tighter security, reduced servicing, improved performance, and decreased costs</li>
	<li>Built-in <a href="https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection">dependency injection</a></li>
	<li>no longer based on System.Web.dll</li>
	<li>Open source and community focused</li>
	<li>Ability to host on IIS or self-host in your own process</li>
	<li>Ships entirely as <a href="https://nuget.org/">NuGet</a> packages</li>
	<li>Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files</li>
	<li>You can create HTTP services with full support for content negotiation using custom or built-in formatters (JSON, XML)</li>
	<li>Model Binding automatically maps data from HTTP requests to action method parameters</li>
	<li>Model Validation automatically performs client and server side validation</li>
</ol>
ASP.NET Core flavors?

that ASP.NET Core is an application framework that can run on either the full .NET framework or on .NET Core. Selecting a .NET framework flavor is one of your first decisions when making a move to ASP.NET Core. Do you want to run on the full framework, or .NET Core, or support both.

<img class="alignnone size-full wp-image-383" src="https://techforumugm.files.wordpress.com/2017/04/frameworks_3.png" alt="frameworks_3" width="600" height="386" />

You have option to select between .NET Framework and .NET Core.NET framework is the mature .NET framework that has been around since the beginning, is pre-installed with Windows, and includes application level frameworks like windows forms, web forms, WCF, and WPF. The most recent version at this time is 4.6.2.  And .NET Core, a new and modular version of .NET that runs on .NET Core will run on Windows, on the Mac, and on various flavors of Linux or Docker Container.
