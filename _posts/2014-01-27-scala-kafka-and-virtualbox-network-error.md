---
title: scala-kafka and virtualbox network error
layout: post
permalink: /2014/01/27/scala-kafka-and-virtualbox-network-error/
tags: [kafka, vagrant, osx]
---

Playing with [scala-kafka][1] ends up with this error on OSx 10.9.1 :

{% highlight bash %}
$ vagrant up
Bringing machine 'zookeeper' up with 'virtualbox' provider...
Bringing machine 'brokerOne' up with 'virtualbox' provider...
[zookeeper] Box 'precise64' was not found. Fetching box from specified URL for
the provider 'virtualbox'. Note that if the URL does not have
a box for this provider, you should interrupt Vagrant now and add
the box yourself. Otherwise Vagrant will attempt to download the
full box prior to discovering this error.
Downloading box from URL: http://files.vagrantup.com/precise64.box
Extracting box...te: 5181k/s, Estimated time remaining: --:--:--)
Successfully added box 'precise64' with provider 'virtualbox'!
[zookeeper] Importing base box 'precise64'...
[zookeeper] Matching MAC address for NAT networking...
[zookeeper] Setting the name of the VM...
[zookeeper] Clearing any previously set forwarded ports...
[zookeeper] Clearing any previously set network interfaces...
There was an error while executing `VBoxManage`, a CLI used by Vagrant
for controlling VirtualBox. The command and stderr is shown below.

Command: ["hostonlyif", "create"]

Stderr: VBoxManage: error: Unable to create a host network interface
VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component Host, interface IHost, callee nsISupports
Context: "CreateHostOnlyNetworkInterface (hif.asOutParam(), progress.asOutParam())" at line 64 of file VBoxManageHostonly.cpp
{% endhighlight %}

###Cause:
The virtual box service is not running properly.

###Solution:
Restart virtual box with :

{% highlight bash %}
sudo /Library/StartupItems/VirtualBox/VirtualBox restart
{% endhighlight %}

Thanks to [Thibaut][2].

 [1]: https://github.com/stealthly/scala-kafka
 [2]: https://coderwall.com/p/ydma0q