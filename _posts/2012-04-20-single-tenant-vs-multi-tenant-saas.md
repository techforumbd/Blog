---
layout: post
title: Single-Tenant Vs Multi-Tenant SaaS
date: 2012-04-20 16:22
author: techforumugm
comments: true
categories: [Cloud Computing]
---
In single-Tenant architecture user use dedicated instance of application software hosting on a server and database not shared. In case where same application software is used by different companies having same functionality but do not share same instance and database. Each company uses it’s own instance and database. SAP is the company which provides service following the single tenant architecture.<br /><br /><img src="http://photos-f.ak.fbcdn.net/hphotos-ak-prn1/524150_10150727498504183_551824182_9154389_922758941_a.jpg" /><br /><br />But in Multi-tenant architecture, multiple companies’ use same instance of application software. It is hosted in a server and data are kept in same server and data are separated by simple partition that prevents data migration. Salesforce.com is the successful SaaS provider having the multi-tenant architecture.<br /><img src="http://photos-e.ak.fbcdn.net/hphotos-ak-ash3/536026_10150727495804183_551824182_9154367_1692091333_a.jpg" /><br /><br />Single tenant system user gets freedom for customization, security and enhancement because by that other client is not impacted. But in multi-tenant environment one client requirement can differ from another that it become difficult to fit.<br />Single tenant environment is more secure then multi-tenant because data are kept differently and may implement customer specific security. Single tenant is more costly then multitenant system.<br />Multi-tenant systems efficiently use each resource and cost shared between clients. <br />Thinking ins and outs of both architecture and requirement and business context decision need to be made.
