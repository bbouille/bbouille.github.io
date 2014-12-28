---
title: 'How to build storm-mesos (0.8.2-0.11.0)'
layout: post
permalink: /2013/06/06/how-to-build-storm-mesos/
tags: [debian, storm, mesos, TechNotes] 
---

<section id="table-of-contents" class="toc">
  <header>
    <h3>Overview</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

## Introduction

On the road of ressource optimisation and cluster efficiency, mesos is good move for distribution like Hadoop or MPI ([see the paper ][1]from Berkeley). However what about other technologies like Storm and real time processing ? It seems that storm can run on top of mesos with a custom storm distribution published by Nathan Martz : [storm-mesos][2]. Let's see how to do it.

**Note** : another framework is under development at Yahoo and the prototype is available on github : [storm-yarn][3].

## Pre-requisites

  * Make sure you have JDK 1.6 (see [these notes][4])
  * A mesos 0.11.0 cluster up and running (see [these notes][5])
  * A quorum of zookeeper 3.4.5 (see [admin notes][6])

## Environnement

  * OS : Debian 6.0.6 x64

## Download

  * storm-0.8.2.zip ([projet site][7]) : distributed and fault-tolerant realtime computation framework
  * storm-mesos ([from github][8]) : the distribution to run Storm on top of Mesos
  * lein ([from github][9]) : a shell script to manage Leiningen, a tool to automate Clojure projets

Quick download to your *storm* home directory :
{% highlight bash %}
$ wget https://dl.dropbox.com/u/133901206/storm-0.8.2.zip
$ git clone git://github.com/isnoopy/storm-mesos.git
$ wget https://raw.github.com/technomancy/leiningen/stable/bin/lein
{% endhighlight %}

## Install lein

  * Place it on your $PATH :
{% highlight bash %}
$ mv ~/lein ~/bin/.
$ echo "export PATH=$PATH:/home/storm/bin" >> ~/.bashrc
$ source ~/.bashrc
{% endhighlight %}

  * Set it to be executable : 
{% highlight bash %}
chmod 755 ~/bin/lein
{% endhighlight %}

  * Install leiningen (v2.2.0) : `lein self-install`

## Prepare storm-mesos

Copy mesos-0.11.0.jar and protobuf-2.4.1.jar from mesos build in the lib/ folder :
{% highlight bash %}
$ cd storm-mesos
$ mkdir lib
$ cp ../mesos-0.11.0/build/protobuf-2.4.1.jar lib/.
$ cp ../mesos-0.11.0/build/src/mesos-0.11.0.jar lib/.
{% endhighlight %}

* Copy the storm distribution in the lib/ folder:
{% highlight bash %}
$ cd ../storm-0.8.2.zip lib/.
{% endhighlight %}

* Update the description (version number etc.) of storm-mesos in `project.clj`

* Update the storm configuration file : `storm.yaml`. For this post, every daemons are running on localhost and the configuration is the following :

{% highlight bash %}
## Default configuration for standalone mode (every daemons on one node with default settings)

# Path to mesos distribution built properly
java.library.path: "native:/usr/local/mesos-0.11.0/build/src/.libs"

# hostname:port of the mesos master node
mesos.master.url: "localhost:5050"

# in cluster ENV, change it to a globally accessible directory (HDFS or NFS etc.)
mesos.executor.uri: "/usr/local/storm-mesos-0.8.2-SNAPSHOT.tgz"

# hostname:port of zookeeper nodes (default port 2181)
storm.zookeeper.servers:
- "localhost"

# hostname of nimbus node
nimbus.host: "localhost"

# full path of storm local working directory
storm.local.dir: "/usr/local/storm-local"
{% endhighlight %}

**Note** : at this moment the `storm-mesos-0.8.2-SNAPSHOT.tgz` is not yet build. However the storm configuration file requires to set its location now in the mesos.executor.uri. The file `storm-mesos-0.8.2-SNAPSHOT.tgz` is the same for every node. In standalone mode, local path is used but in a cluster mode, a path accessible globally (HDFS or NFS for example) is required.

* Update dependencies :
{% highlight bash %}
$ lein deps
{% endhighlight %}

* Create the pom.xml :
{% highlight bash %}
$ lein install
{% endhighlight %}

* Compile with maven :
{% highlight bash %}
$ mvn clean compile install
{% endhighlight %}

* Build storm-mesos :
{% highlight bash %}
$ ./bin/build-release.sh lib/storm-0.8.2.zip
{% endhighlight %}

* Deploy the distribution to the path declared in `mesos.executor.uri` of `storm.yaml` :
{% highlight bash %}
$ sudo cp storm-mesos-0.8.2-SNAPSHOT.tgz /usr/local/.
{% endhighlight %}

* Unpack the distribution to run the Nimbus (this operation is done only on the master node running the Nimbus). For standalone mode, unpack it in the home directory:
{% highlight bash %}
$ cd ~
$ tar xzvf storm-mesos/storm-mesos-0.8.2-SNAPSHOT.tgz
{% endhighlight %}

* Ensure that zookeeper is running on localhost and run the nimbus `as root` :
{% highlight bash %}
$ cd ~/storm-mesos-0.8.2-SNAPSHOT
$ sudo ./bin/storm-mesos nimbus
{% endhighlight %}

* Mesos UI overview (standalone) : [http://localhost:5050]()

![](http://blog.bbouille.eu/wp-content/uploads/2013/06/mesos-0.11.0-UI-local-storm-mesos.png)

* Run the storm ui :
{% highlight bash %}
$ cd ~/storm-mesos-0.8.2-SNAPSHOT
$ ./bin/storm-mesos ui
{% endhighlight %}

* Storm UI overview (standalone) : [http://localhost:8080]()

![](http://blog.bbouille.eu/wp-content/uploads/2013/06/mesos-0.11.0-UI-local-storm-mesos-ui.png)

 [1]: http://static.usenix.org/event/nsdi11/tech/full_papers/Hindman_new.pdf
 [2]: https://github.com/nathanmarz/storm-mesos
 [3]: https://github.com/yahoo/storm-yarn
 [4]: http://codetalk.code-kobold.de/?p=289
 [5]: /how-to-install-mesos-0-11-0-incubating-on-squeeze-6-0-6/
 [6]: http://zookeeper.apache.org/doc/r3.3.3/zookeeperAdmin.html
 [7]: http://storm-project.net/downloads.html
 [8]: https://github.com/isnoopy/storm-mesos
 [9]: https://github.com/technomancy/leiningen