---
title: VMware Tools on Debian Wheezy
layout: post
permalink: /2012/11/05/vmware-tools-on-debian-squeeze/
tags: [debian, technotes]
---

##Distribution settings

As root, install the following packages to be ready for kernel building :

{% highlight bash %}
apt-get install make gcc build-essential 
apt-get install linux-headers-$(uname -r)
{% endhighlight %}

##VMware Tools installation

From Fusion or Workstation hypervisor, find the menu &#8220;Install/Upgrade VMware tools&#8221; to load the tools archive through the CDROM drive.

{% highlight bash %}
$ cd /tmp
$ tar xfz /media/cdrom/VMwareTools-9.2.2-893683.tar.gz</pre>
{% endhighlight %}

Build and install (default options are fine) the tools as root :

{% highlight bash %}
cd vmware-tools-distrib
./vmware-install.pl</pre>
{% endhighlight %}

Run the tools :

{% highlight bash %}
$ /usr/bin/vmware-user
{% endhighlight %}

Note : no need to restart your session to enable copy/paste from host and screen resizing !

<br />

[source](http://www.mattpson.info/2011/04/16/vmware-tools-on-debian-squeeze-a-short-howto)