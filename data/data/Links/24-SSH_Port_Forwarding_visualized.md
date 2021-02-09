---
date: 02-08-21
id: 24
path: Links
tags:
- ssh
title: 'SSH Port Forwarding visualized'
type: bookmark
url: http://dirk-loss.de/ssh-port-forwarding.htm
---

[Home](http://dirk-loss.de/)

# SSH port forwarding visualized

The command line syntax for SSH port forwarding always seemed awkward and confusing to me:


    $ ssh -L 22222:localhost:33333 dirk@192.168.100.1
    $ ssh -R 1234:localhost:4321 test@10.10.10.10


I guess everyone using SSH is familiar with host:port pairs -- but how did the SSH developers come up with that unusual port:host:port syntax? And which host does localhost actually refer to in these examples? Hmm...

Well, local port forwarding _alone_ was quite understandable to me. But as soon as remote port forwarding was added to the discussion, I always got very confused. Because now I had to deal with local and remote hosts simultaneously, with clients and servers, and lots of ports. Moreover, it seemed the "servers" were not always on the remote side. I felt like a child being taught the meaning of "left" and "right" -- both at once...

Several weeks ago I finally decided to re-read the (very good) SSH [man pages](http://www.openbsd.org/cgi-bin/man.cgi?query=ssh&sektion=1) and draw a diagram that would explain that syntax to me. I have reproduced it below (click on the diagram to download a [PDF](ssh-port-forwarding.pdf) version).

Now I realized that port:host:hostport is actually a simplified version of bind_addr:port:host:hostport, i.e. two host:port specifications concatenated together using a colon. Aha! Moreover, I came to the conclusion that it's instructive to start the learning process with the (seemingly most complicated) situation where the host is _not_ localhost but another (third) system, and the application using the forwarding resides on yet another (fourth) system. I thought if I understood that scenario, I would understand the simpler (and more common) scenarios as well. Turned out to be true.

Each diagram below shows an application "APP" (e.g. an SMTP client) opening a TCP connection, that being forwarded over the encrypted SSH tunnel and on the outgoing side being connected to the application server APPSRV (e.g. an SMTP server).

The diagram shows why remote port forwarding is so difficult to understand: it is unusual because the application (APP) is on the remote side and the host is on the local side. So the application connection is established in the _opposite_ direction of the initial connection made by the SSH client to the SSH server.

Now we can simplify again: Typically you will not need the bind_addr parameter. Just leave it out. Then your local port forwarding can only be accessed from the same system where the SSH client runs on. That's probably what you want. If you _really_ want to your locally forwarded port to be accessed from other systems, you have specify the bind_addr and set the GatewayPorts parameter to yes. That is a security feature. In the remote port forwarding scenario, the forwarded port _can_ be accessed from the outside by default (bind_addr defaults to *). So you need bind_addr only if you want to limit that access to a particular interface.

Please keep in mind that in both cases only the SSH tunnel is encrypted (and authenticated) -- everything else is transmitted as clear text.

Feedback or corrections are always welcome.

[Home](http://dirk-loss.de/)

[CC-BY (de)](http://creativecommons.org/licenses/by/3.0/de/) 2012 Dirk Loss - - [Last Changed](http://validator.w3.org/check/referer): [ 2012-03-11](http://www.cl.cam.ac.uk/%7Emgk25/iso-time.html)
