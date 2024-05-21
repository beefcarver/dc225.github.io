---
title: "How to setup a tor exit node on Debian"
layout: blog_article
author: dc225
version: 1.0.1
---

<img src="https://i.imgur.com/4FCZI9l.png" width="25%"> & <img src="https://i.imgur.com/1uQ4nLQ.jpg" width="25%">
<h1>How to setup a tor exit node on Debian and create a responsible torrc config file</h1>

<h2>Log into box as root and do the following to setup tor</h2>
(the box this was installed on was running Debian 8.5 x64)<br>
1. `apt-get install -y tor ntp`<br>
2. `nano /etc/tor/torrc` to edit your configuration file<br>
3. uncomment the "ORPort" setting line<br>
4. change the "ExitPolicy" lines as required to be relative<br>
5. uncomment and set the "ContactInfo" line to whatever you want your TorRelay to be named (publicly viewable)<br>
6. save file and exit<br>
7. `service tor reload` to restart the service and get the new edits working<br>

<a href="http://46.101.98.208/torrc" target="_blank">Copy of our current torrc file</a>

We highly encourage using an advanced or a more modified version of the torrc configuration file. This helps out the Tor community by preventing known malicious botnet traffic (ransomware, crimeware and malware) from using your tor relay or exit node. We recommend using the ‘Crimeware and Ransomware Prevention - ExitPolicy’ reject list from <a href="https://tornull.org/" target="_blank">tornull</a> + using the <a href="https://trac.torproject.org/projects/tor/wiki/doc/ReducedExitPolicy" target="_blank">Reduced Exit Policy</a> from Tor project. Configuring an advanced Exit Policy will help cut down on abuse complaints from your ISP, server terminations, and prevent a decent amount of malicious activity from using your server.

<h2>Where to host a tor exit node</h2>
We're using DigitalOcean for this. A $10/m droplet using Debian 8 x64, 1 GB Memory / 30 GB Disk / 2 TB transfer and hosted at the DO Frankfurt, Germany location. DO is sorta weird when it comes to bandwidth usage. I submitted a support ticket asking about how to monitor overall BW usage at the DO backend level and they replied with basically "you cant and we wont charge you for exceeding the 2TB bw limit"..hrm. I certainly used way over 2TB of bw in October but my bill was only $10. So, ya, that's really good. A lot of others recommend using OVH as well for tor exit nodes. If you want to consider all of the best hosting options, <a href="https://metrics.torproject.org/bubbles.html#as-exits-only" target="_blank">here is a neat list of current Tor Exit Nodes listed by ISP</a> and <a href="https://trac.torproject.org/projects/tor/wiki/doc/GoodBadISPs" target="_blank">here is a list of good/bad Tor hosting providers</a>.

<img src="https://i.imgur.com/2cZZC4C.png" width="75%">

<h2>What to do if you get an abuse complaint</h2>
If you run the default and stock ExitPolicy while running an exit node, you most likely get abuse complaints within ~72 hours.

Luckily for us the Tor Project provides some base templates to use depending upon the type of abuse complaint to come in. <a href="https://trac.torproject.org/projects/tor/wiki/doc/TorAbuseTemplates" target="_blank">You can view them all here</a>. The best way to handle abuse complaints is to set up your exit node so that they are less likely to be sent in the first place.<br>

Within ~24 hours of setting up our exit node that used the default stock ExitPolicy, two abuse complaints rolled in.<br>

The first abuse complaint was an auto generated complaint from a box with fail2ban on it. Someone used the tor exit node to attempt and bruce for logins on another server.<br>

The second abuse complaint was a claimed copyright infringement notice from company, IP-Echelon, an anti-piracy firm who works with copyright holders to protect their data online. Looks like this law firm has a script setup that scans torrent links they own and then subpoena/contact every single IP that downloads movies belonging to their client (Paramount Pictures Corporation). In this case it looks like it was a Shrek the Third bluray torrent. <a href="https://www.torproject.org/eff/tor-dmca-response.html.en" target="_blank">Here</a> is a good template to use for DMCA complaints like this.<br>

>Evidentiary Information: <br>
>Protocol: BITTORRENT <br>
>Infringed Work: Shrek the Third <br>
>Infringing FileName: Shrek the Third (BDrip 1080p ENG-ITA-GER-SPA-TUR) x264 bluray (2007) <br>
>Infringing FileSize: 4736643065 <br>
>Infringer's IP Address: 46.101.98.208 <br>
>Infringer's Port: 45697 <br>
>Initial Infringement Timestamp: 2016-09-01T09:24:12Z
<br>
<img src="http://i.imgur.com/sAl4pBJ.png" width="25%">

IMO, the best way to deal with abuse complaints generated from your Tor exit node is to respond and say you will add their IP ranges to you ExitPolicy reject rules.  This let’s your ISP know you’re down to be pro-active about abuse as well as let’s the complainer know you want to help stop the abuse from happening.<br>

So as you can see, you will have a lot less headaches and worry if you setup an advanced tor ExitPolicy to avoid a lot of these drama llamas. You can also <a href="https://www.upcloud.com/support/installing-fail2ban-on-debian-8-0/" target="_blank">setup fail2ban</a> to harden your server and prevent any hacking/brute force ssh attempts on it.<br>

To make people hella sure our exit node IP is not trying to be malicous, had to throw up a clear message, <a href="http://46.101.98.208/" target="_blank">http://46.101.98.208/</a>. All you have to do is setup apache and then edit the index.html file located in /var/www/html.

Thanks for wanting to learn more about setting up a tor exit node and hopefully this was at least 1% helpful for you.

<h3>Additional reading</h3>

Stats on our current exit node<br>
<a href="https://atlas.torproject.org/#details/D33E1E8F1B9FF03FD2683CE75AA760F75CA30363" target="_blank">https://atlas.torproject.org/#details/D33E1E8F1B9FF03FD2683CE75AA760F75CA30363</a><br>

Running a Tor Exit Node for fun and e-mails<br>
<a href="https://blog.daknob.net/running-a-tor-exit-node-for-fun-and-e-mails/" target="_blank">https://blog.daknob.net/running-a-tor-exit-node-for-fun-and-e-mails/</a><br>

Fail2ban commands and reporting<br>
<a href="http://www.the-art-of-web.com/system/fail2ban-log/" target="_blank">http://www.the-art-of-web.com/system/fail2ban-log/</a><br>

TorWorld<br>
<a  href="http://torworld.org" target="_blank">TorWorld FastExit and FastRelay</a><br> Easy setup of a Tor Exit node or Tor Relay with responibly ExitPolicy pre-made to cut down on abuse.

Tor Null<br>
<a href="https://tornull.org/" target="_blank">Tor Null Advisory BL</a>

Tor subreddit<br>
<a href="https://www.reddit.com/r/tor" target="_blank">/r/tor</a>

Donate Bitcoin to keep our <a href="https://defcon225.org" target="_blank">DC225</a> tor exit node alive and maintaned : 1Knbz4isVBZiCQxGHnYii26HkXcGwJTeYP<br> <3

<img src="http://i.imgur.com/flndXR0.png" width="50%">
