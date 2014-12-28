---
title: 'Ruby on Rails et OSx 10.8.2 [FR]'
layout: post
permalink: /2013/03/24/ruby-on-rails-osx-10-8-2/
tags: [TechNotes, ruby on rails]
---
Avec l&#8217;installation des cadeaux de noël (SSD + 8 Go) dans le macbook pro, une nouvelle installation de Mountain Lion m&#8217;a paru un choix judicieux. Qui dit nouvel OS, dit nouvelle installation des outils et environnements. Voici mes notes d&#8217;installation pour RoR (largement) inspiré du poste de [dean.io][1].

* Installer [XCode][2] (recommandé, pratique de l&#8217;avoir mais pas obligatoire)
* Installer les &#8220;Command Line Tools&#8221; pour disposer de &#8216;`make`&#8216; (et de `GIT` d&#8217;après [Burke][3]) entres autres outils. Deux possibilités. Soit à partir de XCode dans le menu &#8221; Préférences > Téléchargements > Composants &#8221; puis &#8220;installer&#8221;. Soit à partir de [developer.apple.com][4] (attention à prendre la version pour OSx 10.8 à côté de XCode 4.4).
* Installer [XQuartz][5] à partir du DMG.
* Installer HomeBrew :
{% highlight bash %}
$ ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"
{% endhighlight %}
* Vérifier que HomeBrew ne renvoie pas d&#8217;erreur (corriger les Warnings éventuels) :
{% highlight bash %}
    $ brew doctor
{% endhighlight %}
* Installer GCC 4.2 car Mountain Lion ne l&#8217;inclut pas par défaut :
{% highlight bash %}
$ brew tap homebrew/dupes
$ brew install apple-gcc42
$ sudo ln -s /usr/local/bin/gcc-4.2 /usr/bin/gcc-4.2
{% endhighlight %}
* Installer [RVM][6] :
{% highlight bash %}
$ curl -L https://get.rvm.io | bash -s stable
{% endhighlight %}
* Installer Ruby :
{% highlight bash %}
$ rvm install 1.9.3
No binary rubies available for: downloads/ruby-1.9.3-p362.
[...]
Install of ruby-1.9.3-p362 - #complete
{% endhighlight %}
Pour une version différente, voir [ce post][7].
* Mettre la version 1.9.3 par défaut
{% highlight bash %}
    $ rvm use 1.9.3 --default
{% endhighlight %}
* Installer les gems de base :
{% highlight bash %}
    $ gem install rails bundler
{% endhighlight %}
* Installer [Pow][8] qui est server Ruby pour le développement très pratique et léger. Il évite d&#8217;utiliser `rails server` tout le temps :
{% highlight bash %}
$ curl get.pow.cx | sh
{% endhighlight %}

 [1]: http://dean.io/posts/setting-up-a-ruby-on-rails-development-environment-on-mountain-lion
 [2]: http://itunes.apple.com/gb/app/xcode/id497799835?mt=12
 [3]: http://a.shinynew.me/post/28127780463/git-is-gone-in-os-x-mountain-lion
 [4]: http://developer.apple.com/
 [5]: http://xquartz.macosforge.org/landing/
 [6]: https://github.com/wayneeseguin/rvm#installation
 [7]: http://www.thiagogabriel.com/how-to-install-ruby-1-9-3-p194-on-rvm/
 [8]: http://pow.cx/