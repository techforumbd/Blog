---
layout: post
title: Remote Desktop to connect to a Microsoft Azure Ubuntu VM
date: 2017-05-19 07:54
author: techforumugm
comments: true
categories: [MS SQLServer]
---
<div>Xrdp is an open source RDP server, which allows you to connect your Linux server with Remote Desktop from a Windows machine. It performs much nicer than VNC (Virtual Network Computing). VNC has this streak of “JPEG” quality and slow behavior, whereas RDP is fast and crystal clear.</div>
<div></div>
<div>We will use the default endpoint 3389 for Remote Desktop in this doc. So set up 3389 endpoint as Remote Desktop to your Linux VM like below:</div>
<div></div>
<div><img class="alignnone size-full wp-image-489" src="https://techforumugm.files.wordpress.com/2017/05/portconf.png" alt="portconf" width="812" height="664" /></div>
<div></div>
<div>
<div>1. Install XRDP to Connect to your Linux VM through putty, and install Install xrdp</div>
<pre>sudo apt-get update
sudo apt-get install ubuntu-desktop</pre>
<div>2. For Ubuntu, use:</div>
<pre>sudo apt-get install xrdp</pre>
<div>3. Using xfce if you are using Ubuntu version later than Ubuntu 12.04LTS</div>
<pre>sudo apt-get install xubuntu-desktop</pre>
<div>4. Then enable xfce, use:</div>
<pre>echo xfce4-session &gt;~/.xsession</pre>
<div>5. Edit the config file /etc/xrdp/startwm.sh, use:</div>
<pre>sudo vi /etc/xrdp/startwm.sh
Add line xfce4-session before the line /etc/X11/Xsession</pre>
<div>6. Restart xrdp service, use:</div>
</div>
<pre>sudo service xrdp restart</pre>
<div>7. Connect through Remote desktop from Windows PC</div>
<div></div>
<div><img class="alignnone size-full wp-image-477" src="https://techforumugm.files.wordpress.com/2017/05/remote-desktop.png" alt="Remote Desktop" width="407" height="477" /></div>
<div></div>
<div>8.  Now the Ubuntu Login option has appeared</div>
<div></div>
<div><img class="alignnone size-full wp-image-481" src="https://techforumugm.files.wordpress.com/2017/05/login.png" alt="login" width="807" height="705" /></div>
<div></div>
<div>9.  WOW ! Ubuntu Desktop has appeared :</div>
<div></div>
<div><img class="alignnone size-full wp-image-482" src="https://techforumugm.files.wordpress.com/2017/05/ubuntu-desktop.png" alt="Ubuntu Desktop" width="958" height="714" /></div>
<div></div>
