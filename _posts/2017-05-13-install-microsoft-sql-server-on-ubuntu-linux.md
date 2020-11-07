---
layout: post
title: Install Microsoft SQL Server On Ubuntu 17.04
date: 2017-05-13 12:13
author: techforumugm
comments: true
categories: [MS SQLServer]
---
<span style="color:#000000;font-family:Calibri;">Exciting mater is that we’re going to install Microsoft SQL Server on Ubuntu Linux. I’ve been working on SQL Server from last fourteen years. Many times, I’m to face many common words “Is your Application database can be installed on Linux platform?”, need additional licensing cost to buy OS for database server and Database server installation on Linux is more secure, … etc. Some time I feel people are not so well-informed about Linux OS and commands. It’s treated as mysterious island and fearful then Windows. Few people’s assumption or mindset is database server installation on Linux is safer. But safety and security depends on many initiative that we should follow. </span><span style="color:#000000;font-family:Calibri;">In the Post "<a href="https://techforumugm.wordpress.com/2017/05/19/ubuntu-17-04-vm-creation-on-azure/">Ubuntu VM creation on Azure </a>" it was described how can we setup an Virtual Machine. Now were going towards the installation of SQL Server 2017 on Ubuntu.</span>

<span style="color:#000000;font-family:Calibri;">1.</span><span style="font:7pt 'Times New Roman';margin:0;font-size-adjust:none;font-stretch:normal;"><span style="color:#000000;">       </span></span><span style="color:#000000;font-family:Calibri;">We’re using use Putty to connect our VM. For on premises, we need to configured and enabled internet connection.</span>

<span style="color:#000000;font-family:Calibri;">Notes: PuTTY is a free and open-source terminal emulator, serial console and network file transfer application. It supports several network protocols, including SCP, SSH, Telnet, rlogin, and raw socket connection.</span>

<span style="color:#000000;font-family:Calibri;"> </span><span style="color:#000000;font-family:Calibri;">2.  You can install it or run the executable file(exe). Initially we use Putty to access our Ubuntu VM. After that we can use xrdp as remote desktop tools. Now copy your VM IP address and put the IP address in the Host Name for IP address and press ‘Open’ button:</span>
<div></div>
<div><em><img class="alignnone size-full wp-image-444" src="https://techforumugm.files.wordpress.com/2017/05/2-2.jpg" alt="2.2" width="481" height="472" /></em></div>
<div></div>
<div>3. Login option will be appeared: Input user name and password</div>
<div></div>
<div> <img class="alignnone size-full wp-image-443" src="https://techforumugm.files.wordpress.com/2017/05/2-2-1-putty-terminal.png" alt="2.2.1 Putty terminal" width="663" height="423" /></div>
<div></div>
<div>4. Install SQL Server
we have to run ‘apt-get update’ in order to sync the package index files with the new source that we’ve just added. Import the public repository GPG keys</div>
<pre>curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -</pre>
<div>5. Register the Microsoft SQL Server Ubuntu repository:</div>
<div>
<pre>curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server.list | sudo tee /etc/apt/sources.list.d/mssql-server.list</pre>
</div>
<div> 6. Run the following commands to install SQL Server:</div>
<div>
<pre>sudo apt-get update
sudo apt-get install -y mssql-server</pre>
7. When the package installation will be finished, we have to run ‘mssql-conf’ setup and ensure we agree to accept the license term. Then Enter the SQL Server system administrator password. Make sure to specify a strong password for the SA account (Minimum length 8 characters, including uppercase and lowercase letters, base 10 digits and/or non-alphanumeric symbols).

</div>
<pre>sudo /opt/mssql/bin/mssql-conf setup</pre>
<img class="alignnone size-full wp-image-445" src="https://techforumugm.files.wordpress.com/2017/05/2-6.png" alt="2.6" width="663" height="421" />

8. Once the configuration is done, verify that the service is running:
<pre>systemctl status mssql-server</pre>
<img class="alignnone size-full wp-image-446" src="https://techforumugm.files.wordpress.com/2017/05/2-7.png" alt="2.7" width="663" height="423" />

9. Connect to the server from Linux we need to install the ‘mssql-tools’ package, which comes from a different repository than the one that we just set up. It can be found here:
<pre>sudo apt-get update</pre>
<div>10. Re-run the installation command, this will upgrade the specific mssql-server package:
sudo apt-get install mssql-server .Now need install sql server tools.</div>
<div></div>
<img class="alignnone size-full wp-image-447" src="https://techforumugm.files.wordpress.com/2017/05/2-10.png" alt="2.10" width="663" height="425" />

11. Install the mssql-tools on Ubuntu.First Step Import the public repository GPG keys
<pre>curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add –</pre>
<div>12 Register the Microsoft Ubuntu repository.</div>
<pre>curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list</pre>
<div>13 Update the sources list and run the installation command with the unixODBC developer package.</div>
<div></div>
<pre>sudo apt-get update 
sudo apt-get install mssql-tools unixodbc-dev</pre>
System will ask you to do continue ? Press YES

<img class="alignnone size-full wp-image-448" src="https://techforumugm.files.wordpress.com/2017/05/2-13-1.png" alt="2.13.1" width="663" height="421" />

14. At the time of configuration mssql-tools ,License Acceptance term will be appear Press Yes

<img class="alignnone size-full wp-image-449" src="https://techforumugm.files.wordpress.com/2017/05/2-13-2.png" alt="2.13.2" width="659" height="421" />

15. Configuration msodbcsql option completion require your authentication of YES.

<img class="alignnone size-full wp-image-450" src="https://techforumugm.files.wordpress.com/2017/05/2-13-3.png" alt="2.13.3" width="663" height="417" />
<div>16. Add /opt/mssql-tools/bin/ to your PATH environment variable in a bash shell.
To make sqlcmd/bcp accessible from the bash shell for login sessions, modify your PATH in the ~/.bash_profile file with the following command:</div>
<pre>echo 'export PATH="$PATH:/opt/mssql-tools/bin"' &gt;&gt; ~/.bash_profile</pre>
<div>To make sqlcmd/bcp accessible from the bash shell for interactive/non-login sessions, modify the PATH in the ~/.bashrc file with the following command:</div>
<pre>echo 'export PATH="$PATH:/opt/mssql-tools/bin"' &gt;&gt; ~/.bash_profile</pre>
17. First of all, let’s see how this tool is called on your system. It might be called something like sqlcmd-13.0.1.0 or simply sqlcmd but it should be in /opt/mssql-tools/bin/ directory.
Let’s see what we have there:
<pre>ls /opt/mssql-tools/bin/sqlcmd*</pre>
<div>18. After you get the name of the tool you can create a symlink :</div>
<pre>ln -sfn /opt/mssql-tools/bin/sqlcmd /usr/bin/sqlcmd

</pre>
<div>19. To allow remote connections, you may need to open the SQL Server TCP port on your firewall
The default SQL Server port is 1433.</div>
<pre>sudo ufw allow 1433/tcp</pre>
<div>To (if need ) network service restart</div>
<pre>sudo service network-manager restart</pre>
20. Now we login to the server and execute simple sql
<pre>sqlcmd -S localhost -U SA 

Execute following SQL 

SELECT @@VERSION
2&gt; GO</pre>
&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;
<pre></pre>
