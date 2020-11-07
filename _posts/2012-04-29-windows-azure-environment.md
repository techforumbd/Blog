---
layout: post
title: Windows Azure Environment
date: 2012-04-29 19:17
author: techforumugm
comments: true
categories: [Cloud Computing, Microsoft Azure]
---
<div class="MsoNormal" style="line-height:18pt;margin:4.8pt 0 12pt;text-align:justify;"><span><span style="font-size:14pt;">Application running on cloud is quite like that it is running on a virtual machine that running on Windows. Windows Azure assist for making a deployment environment so that <span> </span><span> </span>applications can be more reliable, more scalable, and require less administration cost. But Windows Azure application environment isn’t exactly the same as building an on-premises application.</span><span> </span><span style="font-size:14pt;"><span> </span></span></span></div><div class="MsoNormal" style="line-height:18pt;margin:4.8pt 0 12pt;text-align:justify;"><span style="font-size:14pt;"><span></span></span><span>Whenever application deployed in Azure it is implemented as roles. That can be web role or worker role or both. Web role directly interact with browser or HTTP depending on IIS. In an ASP.net application simple web role is used to interact with users. On the other site worker role is designed to run code. Like data processing in parallel .User interaction through web role can be relay to worker role to execute user request.</span></div>
