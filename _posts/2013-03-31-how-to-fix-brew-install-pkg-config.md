---
title: 'how to fix &#8216;brew install pkg-config&#8217;'
layout: post
permalink: /2013/03/31/how-to-fix-brew-install-pkg-config/
tags: [TechNotes, ruby on rails]
---
Trying to install pkg-config :

{% highlight bash %}
$  brew install pkg-config
[...]
Warning: Could not link pkg-config. Unlinking...
Error: The `brew link` step did not complete successfully
The formula built, but is not symlinked into /usr/local
You can try again using `brew link pkg-config'
==> Summary
/usr/local/Cellar/pkg-config/0.28: 10 files, 636K
{% endhighlight %}

Go deeper :

{% highlight bash %}
$ brew link pkg-config
Linking /usr/local/Cellar/pkg-config/0.28... Warning: Could not link pkg-config. Unlinking...
Error: Could not symlink file: /usr/local/Cellar/pkg-config/0.28/share/doc/pkg-config
/usr/local/share/doc/pkg-config may already exist.
/usr/local/share/doc may not be writable.
{% endhighlight %}

The solution :

  1. brew uninstall pkg-config
  2. brew install pkg-config
  3. brew link pkg-config
  4. rm -rf `<offending-directory>`
  5. repeat step 1

[source](http://stackoverflow.com/questions/13483059/how-do-i-fix-brew-install-pkg-config)