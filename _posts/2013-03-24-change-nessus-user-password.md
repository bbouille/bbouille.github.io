---
title: Change nessus user password
layout: post
permalink: /2013/03/24/change-nessus-user-password/
tags: [TechNotes, nmap]
---
Execute `nessus-chpasswd <username>` command from the /sbin folder as `root` (example with the default installation directory on OSx 10.8) :

{% highlight bash %}
cd /Library/Nessus/run/sbin
./nessus-chpasswd alice
Authentication (pass/cert) : [pass]
Login password :
Login password (again) :
{% endhighlight %}