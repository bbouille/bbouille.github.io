---
title: How to install mesos on Debian squeeze
layout: post
permalink: /2013/06/06/how-to-install-mesos-0-11-0-incubating-on-squeeze/
tags: [debian, mesos, TechNotes] 
---
Some notes on installation of mesos daemons on a single machine for testing purpose only.

## Environnement

  * OS : Debian 6.0.6 x64

## Download

{% highlight bash %}
$ wget http://mirrors.linsrv.net/apache/incubator/mesos/mesos-0.11.0-incubating/mesos-0.11.0-incubating.tar.gz
{% endhighlight %}

## Dependencies

Install the Debian dependencies, as `root`:
{% highlight bash %}
apt-get install build-essential python-dev libcunit1 libz-dev libssl-dev libcurl4-openssl-dev
{% endhighlight %}

## Installation
Prepare the mesos installation, as `root`:
{% highlight bash %}
tar xzvf mesos-0.11.0-incubating.tar.gz
mv mesos-0.11.0 /usr/local/mesos
cd /usr/local/mesos 
mkdir build 
cd build 
../configure 
make 
make check
{% endhighlight %}

Then install mesos, as `root` :
{% highlight bash %}
make install
{% endhighlight %}

###Configuration (standalone)

See the official configuration page [here][2] for further details.

* Set the hostname 'localhost' in the file `masters`:
{% highlight bash %}
$ cat /usr/local/var/mesos/deploy/masters
localhost
{% endhighlight %}

* Set the hostname 'localhost' in the file `slaves`:
{% highlight bash %}
$ cat /usr/local/var/mesos/deploy/masters
localhost
{% endhighlight %}

* Edit `mesos.conf` and set the following parameters:
{% highlight bash %}
$ vim /usr/local/var/mesos/conf/mesos.conf
log_dir=/var/log/mesos
master=localhost:5050
{% endhighlight %}

* Create the log folder :
{% highlight bash %}
mkdir /var/log/mesos
{% endhighlight %}

* Create and SSH key for `root` on the master without password:
{% highlight bash %}
ssh-keygen -P ""
{% endhighlight %}

* Distribute the key on localhost (standalone mode):
{% highlight bash %}
ssh-copy-id localhost
{% endhighlight %}

**WARNING : this SSH configuration is for testing environnement ONLY, please ensure a proper key and user management on each node.**

##Run mesos

Start the cluster as `root`:
{% highlight bash %}
cd /usr/local/sbin
./mesos-start-cluster.sh
{% endhighlight %}

Network status, as `root` :
{% highlight bash %}
netstat -lptn
Connexions Internet actives (seulement serveurs)
Proto Recv-Q Send-Q Adresse locale Adresse distante Etat PID/Program name
tcp 0 0 0.0.0.0:22 0.0.0.0:* LISTEN 1474/sshd
tcp 0 0 0.0.0.0:5050 0.0.0.0:* LISTEN 8104/mesos-master
tcp 0 0 0.0.0.0:45350 0.0.0.0:* LISTEN 8119/mesos-slave
{% endhighlight %}

Catch the log of the `master` and `slave` daemons with this command as `root`:
{% highlight bash %}
tail -f /var/log/*.INFO
{% endhighlight %}

###Mesos UI

Mesos listening to port 5050 for job orchestration AND web ui, check the cluster status from web ui on http://localhost:5050. An overview of this standalone configuration in the UI :

![](http://blog.bbouille.eu/wp-content/uploads/2013/06/mesos-0.11.0-UI-local.png)

###Test mesos framework

Several examples are available in the distribution:
{% highlight bash %}
cd /usr/local/mesos/build
{% endhighlight %}

Run one `java` example:
{% highlight bash %}
src/examples/java/test-framework localhost:5050
Registered! ID = 201306061832-16777343-5050-8104-0000
Launching task 0
Status update: task 0 is in state TASK_RUNNING
Status update: task 0 is in state TASK_FINISHED
Finished tasks: 1
[...]
Launching task 4
Status update: task 4 is in state TASK_RUNNING
Status update: task 4 is in state TASK_FINISHED
Finished tasks: 5
{% endhighlight %}

Or one `python` example:
{% highlight bash %}
src/examples/python/test-framework localhost:5050
{% endhighlight %}

##Stop mesos

Stop the cluster as `root`:
{% highlight bash %}
cd /usr/local/sbin
./mesos-stop-cluster.sh
{% endhighlight %}

 [1]: http://mirrors.linsrv.net/apache/incubator/mesos/mesos-0.11.0-incubating/mesos-0.11.0-incubating.tar.gz
 [2]: https://github.com/apache/mesos/blob/trunk/docs/Configuration.textile
 [3]: http://blog.bbouille.eu/wp-content/uploads/2013/06/mesos-0.11.0-UI-local.png