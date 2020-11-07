---
layout: post
title: Installing the SSIS Package on Ubuntu 17.04
date: 2017-05-22 17:28
author: techforumugm
comments: true
categories: [Business Intelligence, MS SQLServer]
---
It's Great news that now SQL Server Integration service's  Linux version SSIS is available in CTP 2.1.It's initial step to toward platform independence .According to the Microsoft Program Manager Leo Li  "SQL Server Integration Services CTP 2.1 and try it out on your Linux machines or VM by command lines.Run SSIS packages on Linux OS, monitor and track execution status and result;
<ul>
	<li>Extract data from and to most common used sources and destinations on Linux platform;</li>
	<li>Do common used transformation on Linux platform"</li>
</ul>
First of all we need to install SQL Server 2017 on Ubuntu that I've write in my blog
<a href="https://techforumugm.wordpress.com/2017/05/13/install-microsoft-sql-server-on-ubuntu-linux/">Install Microsoft SQL Server On Ubuntu 17.04</a>

I've tried to install SSIS on Ubuntu according to the Leo Li's blog :
<ol>
	<li>First we have to import the public repository GPG keys:</li>
</ol>
<pre><strong><em>   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add</em></strong></pre>
<ol start="2">
	<li>It's important to aligned repository to install SSIS .Register the Microsoft SQL Server Ubuntu repository:</li>
</ol>
<pre><strong>   <em>curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server.list | sudo tee /etc/apt/sources.list.d/mssql-server.list</em></strong></pre>
<ol start="3">
	<li>Now we're going to install by SSIS using commands below commands:</li>
</ol>
<pre><strong><em>   sudo apt-get update</em></strong></pre>
<img class="alignnone size-full wp-image-537" src="https://techforumugm.files.wordpress.com/2017/05/51.png" alt="5" width="796" height="428" />
<pre><strong><em>       </em></strong><strong><em>sudo apt-get install -y mssql-server-is</em></strong><strong><em> 

<img class="alignnone size-full wp-image-556" src="https://techforumugm.files.wordpress.com/2017/05/8.png" alt="8" width="901" height="691" />
</em></strong></pre>
<ol start="4">
	<li>In this step we need to configure <strong>ssis-conf</strong>:</li>
</ol>
<pre><strong><em>       sudo /opt/ssis/bin/ssis-conf</em></strong></pre>
<ol start="5">
	<li>Once the configuration is done, set path:</li>
</ol>
<pre><b><i>      </i></b><strong><em>export PATH=/opt/ssis/bin:$PATH</em></strong></pre>
<strong><em>    6.</em></strong> if you user is not in <strong>ssis</strong> group, add current user to ssis group:
<pre><b><i>        </i></b><strong><em>sudo gpasswd -a ahsan ssis

<img class="alignnone size-full wp-image-573" src="https://techforumugm.files.wordpress.com/2017/05/101.png" alt="101" width="556" height="319" />
</em></strong></pre>
<pre><strong><em> 7. </em></strong>Copy your SSIS package to your Linux machine and run:

<b><i>       </i></b><strong><em>dtexec /F “your package” /DE “protection password”</em></strong></pre>
&nbsp;
