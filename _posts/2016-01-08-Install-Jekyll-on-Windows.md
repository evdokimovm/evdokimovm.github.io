---
layout: post
title: Install Jekyll on Windows
categories: [Jekyll, Windows, Ruby]
tags: [Jekyll, Windows, Ruby]
fullview: false
comments: true
description: Install Jekyll on Windows. In this article I will talk about how to install Jekyll and all necessary components on Windows.
---

In this article I will talk about how to install Jekyll and all necessary components on Windows.

Let's start with installing ```Ruby```. To do this, you can use ```Ruby Installer``` [http://rubyinstaller.org/downloads/](http://rubyinstaller.org/downloads/)
At the time of this writing, If you don’t know what version to install and you’re getting started with Ruby, we ```recommend``` you use ```Ruby 2.1.X``` installers
[http://dl.bintray.com/oneclick/rubyinstaller/rubyinstaller-2.1.7.exe](http://dl.bintray.com/oneclick/rubyinstaller/rubyinstaller-2.1.7.exe)

Besides also download ```Ruby Development Kit```. For use with Ruby 2.0 and above (32bits version only) [http://dl.bintray.com/oneclick/rubyinstaller/DevKit-mingw64-32-4.7.2-20130224-1151-sfx.exe](http://dl.bintray.com/oneclick/rubyinstaller/DevKit-mingw64-32-4.7.2-20130224-1151-sfx.exe)
and extract in to any folder. I recommend to do the extract it to ```C:\RubyDevKit\```

Once you have installed and extracted Ruby (with ```add Ruby executables to your PATH```) and Ruby Development Kit.
Open a command prompt in the directory where you extract the Ruby Development Kit and do the following in command prompt

{% highlight text %}
> ruby dk.rb init
> ruby dk.rb install
{% endhighlight %}

Now install Python 2.7.11 [https://www.python.org/ftp/python/2.7.11/python-2.7.11.msi](https://www.python.org/ftp/python/2.7.11/python-2.7.11.msi) and do not forget to ```add Python to your PATH```

Download ```get-pip.py``` [https://bootstrap.pypa.io/get-pip.py](https://bootstrap.pypa.io/get-pip.py) and install it with command

{% highlight text %}
> python get-pip.py
{% endhighlight %}

Now open your Command Prompt and use following commands

{% highlight text %}
> python -m pip install Pygments
> gem install wdm
> gem install jekyll
{% endhighlight %}

Now all the necessary components to get started with Jekyll installed.

For a quick start with Jekyll use commands:

{% highlight text %}
> jekyll new myblog
> cd myblog
> jekyll serve
{% endhighlight %}

Now browse to ```localhost:4000```

If as a host for your Jekyll blog you will use GitHub Pages also install Git [https://git-scm.com/](https://git-scm.com/)

Themes for Jekyll you can find here [https://github.com/jekyll/jekyll/wiki/Themes](https://github.com/jekyll/jekyll/wiki/Themes)
