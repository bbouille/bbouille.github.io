---
title: Resume SSH transfer
layout: post
permalink: /2013/08/04/resume-ssh-transfer/
tags: [rsync, ssh]
---
Use rsync to be able to resume a transfer over SSH :

{% highlight bash %}
rsync --partial --progress --rsh=ssh user@host:remote_file local_file
{% endhighlight %}

Thanks to [tigroferoce][1].

 [1]: http://yyab.wordpress.com/2006/12/18/resume-a-large-scp-transfer/