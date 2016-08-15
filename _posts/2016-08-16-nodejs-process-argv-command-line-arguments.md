---
layout: post
title: NodeJS process.argv command line arguments
categories: [JavaScript, NodeJS]
tags: [JavaScript, NodeJS]
fullview: false
comments: true
description: NodeJS process.argv command line arguments. process.argv is an array containing the command line arguments. The first element will be node, the second element will be the name of the JavaScript file. The next elements will be any additional command line arguments.
---

[process.argv](https://nodejs.org/docs/latest/api/process.html#process_process_argv) is an array containing the command line arguments. The first element will be `node`, the second element will be the name of the JavaScript file. The next elements will be any additional command line arguments.

**Code Example:**

Output sum of all command line arguments

`index.js`

{% highlight js %}
var sum = 0;
for (i = 2; i < process.argv.length; i++) {
	sum += Number(process.argv[i]);
}

console.log(sum);
{% endhighlight %}

**Usage Exaple:**

{% highlight text %}
node index.js 8 4 7 6
{% endhighlight %}

Output will be `25`

**A brief explanation of the code:**

Here in for loop `for (i = 2; i < process.argv.length; i++)` loop begins with 2 because first two elements in process.argv array **always** is `['path/to/node.exe', 'path/to/js/file', ...]`

Converting to number `Number(process.argv[i])` because elements in process.argv array **always** is string
