---
layout: post
title: NPM Errors and Warnings
categories: [Tips, Tricks]
tags: [Tips, Tricks]
fullview: false
comments: true
description: NPM Errors and Warnings. Scary error messages in red may appear during install. The install typically recovers from these errors and finishes successfully. A simple example. During installation Gulp, the following messages may appear in the console npm WARN deprecated graceful-fs@3.0.8 graceful-fs version 3 and before will fail on newer node releases. Please update to graceful-fs@^4.0.0 as soon as possible. npm WARN deprecated lodash@1.0.2 lodash@<3.0.0 is no longer maintained. Upgrade to lodash@^4.0.0. npm WARN deprecated graceful-fs@1.2.3 graceful-fs version 3 and before will fail on newer node releases. Please update to graceful-fs@^4.0.0 as soon as possible.
---

Scary error messages in red may appear during install. The install typically recovers from these errors and finishes successfully.

All is well if there are no console messages starting with npm ERR! at the end of npm install. 
There might be a few npm WARN messages along the way — and that is perfectly fine.

We often see an npm WARN message after a series of gyp ERR! messages. Ignore them. 
A package may try to re-compile itself using node-gyp. 
If the re-compile fails, the package recovers (typically with a pre-built version) and everything works.

Just make sure there are no npm ERR! messages at the end of npm install.

A simple example. During installation Gulp, the following messages may appear in the console:

{% highlight text %}
npm WARN deprecated graceful-fs@3.0.8: graceful-fs version 3 and before will fail on newer node releases. Please update to graceful-fs@^4.0.0 as soon as possible.
npm WARN deprecated lodash@1.0.2: lodash@<3.0.0 is no longer maintained. Upgrade to lodash@^4.0.0.
npm WARN deprecated graceful-fs@1.2.3: graceful-fs version 3 and before will fail on newer node releases. Please update to graceful-fs@^4.0.0 as soon as possible.
{% endhighlight %}

In fact, this is not an error, this is warning that one of the modules depending deprecated. 
If your project does not use these modules with the specified versions directly (through package.json), 
then you are not able to fix anything.

Instead, you can do the following:

1. Update of used dependencies. Perhaps the problem is already solved.
2. If after step 1, the warnings still remains, you can notify the authors modules about this problem.
3. Since this is just a Warning, the last thing you need to - it just doesn't pay attention.

If you are still confused by the output of warnings you can run ```npm install``` with flag ```--loglevel=error```.
