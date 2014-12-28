---
title: 'ganglia 3.6.0 sur squeeze [FR]'
layout: post
permalink: /2013/06/11/ganglia-3-6-0-sur-squeeze-fr/
tags: [debian, ganglia, TechNotes]
---
# Présentation

Deux daemons sont fournis dans la distribution ganglia-3.6.x :

  * **gmond** : collecteur des métriques installé sur chaque noeud, il dispose d&#8217;une capacité d&#8217;agrégation dans une certaine mesure également.
  * **gmetad** : agrégateur des métriques, il stocke les métriques d&#8217;un ou plusieurs cluster dans une base de données.

## **Téléchargement de &#8216;ganglia&#8217;**

A partir de sourceforge.com : [ganglia-3.6.0.tar.gz][1] (1.2 MB)

<pre>$ wget http://sourceforge.net/projects/ganglia/files/ganglia%20monitoring%20core/3.6.0/ganglia-3.6.0.tar.gz/download -O ganglia-3.6.0.tar.gz</pre>

## **Installation sur tous les noeuds du cluster ou de la grille**

### Dépendances

<pre># apt-get install libapr1-dev libconfuse-dev libpcre3-dev libexpat1-dev</pre>

### Installation

<pre>$ tar xzvf ganglia-3.6.0.tar.gz
$ cd ganglia-3.6.0
$ ./configure
$ make
$ sudo make install</pre>

## **Installation du noeud de supervision**

### Dépendances

Installation de RRd tool, base de données pour stocker les métriques.

<pre># apt-get install rrdtool librrd-dev</pre>

Installation de ganglia avec le démon gmetad :

<pre>$ tar xzvf ganglia-3.6.0.tar.gz
$ cd ganglia-3.6.0
$ ./configure --with-gmetad
$ make
$ sudo make install</pre>

**note** : Le noeud de supervision doit également être surveillé, c&#8217;est à dire que &#8216;gmond&#8217; doit être exécuté.

# Configuration de &#8216;gmond&#8217; et &#8216;gmetad&#8217;

La configuration des services se fait suivant l&#8217;architecture choisie :

  * échange de métrique unicast ou multicast, le choix peut être contraint pour des raisons de sécurité (désactivation du multicast) ou de performances ;
  * installation d&#8217;un noeud avec collecteur et superviseur sur la même machine (station de travail), d&#8217;un cluster de plusieurs noeuds ou d&#8217;une grille de plusieurs clusters dont un noeud contient le superviseur.

Créer le fichier de configuration du collecteur &#8216;gmond&#8217; sur chaque noeud :

<pre># gmond -t &gt; /usr/local/etc/gmond.conf</pre>

Editer le fichier de configuration du superviseur &#8216;gmetad&#8217; sur le noeud de superviseur :

<pre># vim /usr/local/etc/gmetad.conf</pre>

Puis créer les répertoires suivants avec les droits requis :

<pre># mkdir -p /var/lib/ganglia/rrds
# chown nobody:nogroup /var/lib/ganglia/rrds</pre>

# Démarrage des services

Vérifier que le collecteur ne rencontre pas de problème en l&#8217;exécutant en mode debug dans un premier temps :

<pre># gmond -d 10</pre>

Le fonctionnement normal est un enchainement de publication des métriques et des valeurs relevées.

<pre>[...]
Processing a metric metadata message from n1.localdomain
***Allocating metadata packet for host--n1.localdomain-- and metric --disk_free-- ****

saving metadata for metric: disk_free host: n1.localdomain
Processing a metric value message from n1.localdomain
***Allocating value packet for host--n1-- and metric --disk_free-- ****
[...]</pre>

Une fois tester, lancer le daemon (en root) :

<pre># gmond</pre>

Vérification du status du daemon :

<pre># gstat
CLUSTER INFORMATION
       Name: home-cluster
      Hosts: 1
Gexec Hosts: 0
 Dead Hosts: 0
  Localtime: Thu Jun  6 09:13:58 2013

There are no hosts running gexec at this time</pre>

Vérification réseau :

<pre># netstat -ltpn
Connexions Internet actives (seulement serveurs)
Proto Recv-Q Send-Q Adresse locale          Adresse distante        Etat        PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1498/sshd
tcp        0      0 0.0.0.0:8649            0.0.0.0:*               LISTEN      16419/gmond</pre>

Vérification des métriques disponibles (XML renvoyé sur le port 8649 configuré dans la section &#8216;tcp\_accept\_channel&#8217;) :

<pre class="decode-attributes:false lang:default decode:true"># netcat localhost 8649
&lt;?xml version="1.0" encoding="ISO-8859-1" standalone="yes"?&gt;
&lt;!DOCTYPE GANGLIA_XML [

[...]

&lt;METRIC NAME="mem_total" VAL="372080" TYPE="float" UNITS="KB" TN="126" TMAX="1200" DMAX="0" SLOPE="zero"&gt;
&lt;EXTRA_DATA&gt;
&lt;EXTRA_ELEMENT NAME="GROUP" VAL="memory"/&gt;
&lt;EXTRA_ELEMENT NAME="DESC" VAL="Total amount of memory displayed in KBs"/&gt;
&lt;EXTRA_ELEMENT NAME="TITLE" VAL="Memory Total"/&gt;
&lt;/EXTRA_DATA&gt;
&lt;/METRIC&gt;

[...]</pre>

Ceci montre que les métriques sont bien collectés et mis à disposition par &#8216;gmond&#8217;.

Sur le noeud contenant le superviseur, lancer le daemon &#8216;gmetad&#8217; en mode debug :

<pre># gmetad -d 10
[...]
Writing Root Summary data for metric part_max_used
Created rrd /var/lib/ganglia/rrds/__SummaryInfo__/part_max_used.rrd
[...]</pre>

Ceci montre que les métriques sont bien reçus par &#8216;gmetad&#8217; et enregistré dans la base RRD.

Une fois tester, lancer le daemon (en root) :

<pre># gmetad</pre>

Vérification réseau :

<pre># netstat -ltpn
Connexions Internet actives (seulement serveurs)
Proto Recv-Q Send-Q Adresse locale          Adresse distante        Etat        PID/Program name
tcp        0      0 0.0.0.0:8651            0.0.0.0:*               LISTEN      16501/gmetad
tcp        0      0 0.0.0.0:8652            0.0.0.0:*               LISTEN      16501/gmetad
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1498/sshd
tcp        0      0 0.0.0.0:8649            0.0.0.0:*               LISTEN      16425/gmond</pre>

# Affichage des métriques : ganglia-web

Dépendances :

<pre># apt-get install apache2 libapache2-mod-php5 php5
/etc/init.d/apache2 restart</pre>

**Téléchargement :**

A partir de sourceforge.com : [ganglia-web-3.5.8.tar.gz][2] (1.4 MB)

<pre>$  wget http://sourceforge.net/projects/ganglia/files/ganglia-web/3.5.8/ganglia-web-3.5.8.tar.gz/download -O ganglia-web-3.5.8.tar.gz
</pre>

**Installation :**

Décompresser l&#8217;archive :

<pre>$ tar xzvf ganglia-web-3.5.8.tar.gz</pre>

placer le dossier dans /var/www (en root) :

<pre># mv ganglia-web-3.5.8 /var/www/ganglia</pre>

Configuration des de ganglia-web :

<pre># mkdir -p /var/lib/ganglia-web/conf
# chown -R www-data:www-data /var/lib/ganglia-web/conf

# mkdir -p /var/lib/ganglia-web/dwoo/compiled
# mkdir -p /var/lib/ganglia-web/dwoo/cache
# chown -R www-data:www-data /var/lib/ganglia-web/dwoo/
</pre>

Les métriques sont accessibles sur le portail : http://localhost/ganglia

 [1]: http://sourceforge.net/projects/ganglia/files/ganglia%20monitoring%20core/3.6.0/ganglia-3.6.0.tar.gz/download
 [2]: http://sourceforge.net/projects/ganglia/files/ganglia-web/3.5.8/ganglia-web-3.5.8.tar.gz/download