---
title: 'Debian : Package version'
layout: post
permalink: /2012/12/10/debian-package-version/
tags: [debian, technotes]
---
An easy way to find out which software version `apt-get` will install without running the installation is using the `-s` option :

{% highlight bash %}
apt-get install -s <package>
{% endhighlight %}