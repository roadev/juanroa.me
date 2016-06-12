---
layout: post
title:  "Swift 2 on Linux"
date:   2016-01-25 15:04:23
categories: programming language
tags: [swift lang, swift2, linux, swift linux, apple]
---

Maybe many of you didn't notice that Apple has open-sourced its Swift 2 code (compiler and core-libraries too) under the MIT license the past december (2015).

I'm going to show you how to install it on your Linux System (Archlinux, Ubuntu and Debian distros) and how to run some
basic examples on the swift shell environment.

### Ubuntu 14.04-3

At first, we're going to install some dependences and libs needed for swift lang.

{% highlight swift %}
sudo apt-get update
{% endhighlight %}
{% highlight swift %}
sudo apt-get install git cmake ninja-build clang uuid-dev libicu-dev icu-devtools libbsd-dev libedit-dev libxml2-dev libsqlite3-dev swig libpython-dev libncurses5-dev pkg-config
{% endhighlight %}

Lets get the swift package for this version of Ubuntu.

{% highlight swift %}
wget https://swift.org/builds/swift-2.2-branch/ubuntu1404/swift-2.2-SNAPSHOT-2016-02-08-a/swift-2.2-SNAPSHOT-2016-02-08-a-ubuntu14.04.tar.gz
{% endhighlight %}

Now, untar (decompress) the package

{% highlight swift %}
tar -zxvf swift-2.2-SNAPSHOT-2016-02-08-a-ubuntu14.04.tar.gz
{% endhighlight %}

### Ubuntu 15.10

This is your package at 15.10

{% highlight swift %}
wget https://swift.org/builds/swift-2.2-branch/ubuntu1510/swift-2.2-SNAPSHOT-2016-02-08-a/swift-2.2-SNAPSHOT-2016-02-08-a-ubuntu15.10.tar.gz
{% endhighlight %}

Decompress the package

{% highlight swift %}
tar -zxvf swift-2.2-SNAPSHOT-2016-02-08-a-ubuntu15.10.tar.gz
{% endhighlight %}


**Export the environment variable**
{% highlight swift %}
export PATH=/path/to/swift-2.2-SNAPSHOT-2015-12-01-b-ubuntu14.04/usr/bin/:"${PATH}"
{% endhighlight %}

Test your swift installation

{% highlight swift %}
swift --version
{% endhighlight %}

Let try something in swift lang !

{% highlight swift %}
print("Hello World!")  

> Hello World!

3*67  

> $R0: Int = 201

{% endhighlight %}

We've set swift 2 on Ubuntu ! Now, lets do it for Archlinux.


Install the binary package from AUR repo

{% highlight swift %}
yaourt -S swift-bin
{% endhighlight %}

I'm sure that you will get some kind of error about GPG keys, like this:

{% highlight swift %}

==> Validating source files with md5sums...

    ncurses-6.0.tar.gz ... Passed

    ncurses-6.0.tar.gz.asc ... Skipped

==> Verifying source file signatures with gpg...

    ncurses-6.0.tar.gz ... FAILED (unknown public key 702353E0F7E48EDB)

==> ERROR: One or more PGP signatures could not be verified!

==> ERROR: Makepkg was unable to build ncurses5-compat-libs.
{% endhighlight %}

Fix: Install the GPG signature for ncurses package

{% highlight swift %}
gpg --keyserver pgp.mit.edu --recv-keys C52048C0C0748FEE227D47A2702353E0F7E48EDB
{% endhighlight %}

Possible error # 2:

{% highlight swift %}
==> Validating source files with sha256sums...

    swift-2.2-SNAPSHOT-2015-12-10-a-ubuntu15.10.tar.gz ... Passed

    swift-2.2-SNAPSHOT-2015-12-10-a-ubuntu15.10.tar.gz.sig ... Skipped

==> Verifying source file signatures with gpg...

    swift-2.2-SNAPSHOT-2015-12-10-a-ubuntu15.10.tar.gz ... FAILED (unknown public key D441C977412B37AD)

==> ERROR: One or more PGP signatures could not be verified!

==> ERROR: Makepkg was unable to build .
{% endhighlight %}

"Another GPG signature missing..."

Fix it with:

{% highlight swift %}
wget -q -O - https://swift.org/keys/all-keys.asc | gpg --import -  
{% endhighlight %}

And that's all!

<div id="noosfeer"></div>
<script src="http://noosfeer.com/javascripts/widget.js" async="" defer="defer" type="text/javascript"></script>
