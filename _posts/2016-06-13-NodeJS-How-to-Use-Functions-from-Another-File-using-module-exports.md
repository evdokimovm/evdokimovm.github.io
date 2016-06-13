---
layout: post
title: How to use functions from another file in Node.js using module.exports
categories: [JavaScript, NodeJS]
tags: [JavaScript, NodeJS]
fullview: false
comments: true
description: How to use functions from another file in Node.js using module.exports. Understanding Node.js Require and Exports. In Node.js, all variables, functions, classes are visible to each other only within the same file. We are able to call functions from any other file. To do this, we will use require and module.exports.
---

In Node.js, all variables, functions, classes are visible to each other only within the same file. We are able to call functions from any other file. To do this, we will use `require` and `module.exports`.

First, create 2 files with which we work, `server.js` and `index.js`.

In `server.js` we just include `index.js`

{% highlight js %}
var another = require('./index.js');
{% endhighlight %}

And all functions we will write in our `index.js` file

Lets add to `index.js` object which will contain all of our functions

{% highlight js %}
var methods = {};
{% endhighlight %}

And add some functions

{% highlight js %}
var methods = {
	timestamp: function() {
		console.log('Current Time in Unix Timestamp: ' + Math.floor(Date.now() / 1000));
	},
	currentDate: function() {
		console.log('Current Date is: ' + new Date().toISOString().slice(0, 10));
	}
};
{% endhighlight %}

or

{% highlight js %}
methods.timestamp = function() {
	console.log('Current Time in Unix Timestamp: ' + Math.floor(Date.now() / 1000))
};
methods.currentDate = function() {
	console.log('Current Date is: ' + new Date().toISOString().slice(0, 10))
};
{% endhighlight %}

In order that we can use these functions in other files (in our example is `server.js`), we need to add at the end of `index.js` following code

{% highlight js %}
exports.data = methods;
{% endhighlight %}

> module.exports is a Node.js specific feature, it does not work with regular JavaScript

If you type in your `server.js` file

{% highlight js %}
console.log(another)
{% endhighlight %}

you will see a similar message in the console

{% highlight text %}
/user> node server.js
{ data: { timestamp: [Function], currentDate: [Function] } }
{% endhighlight %}

Now let's use this functions. In `server.js` file, write a code that call the functions which we created above

{% highlight js %}
another.data.timestamp();
another.data.currentDate();
{% endhighlight %}

In your terminal you should see the following

{% highlight text %}
Current Time in Unix Timestamp: 1465804362
Current Date is: 2016-06-13
{% endhighlight %}

#### Note:

Instead of `exports.data = methods;` can be used `module.exports = methods;`. In this case, to call the functions, you should use 

{% highlight js %}
another.timestamp();
another.currentDate();
{% endhighlight %}

instead of

{% highlight js %}
another.data.timestamp();
another.data.currentDate();
{% endhighlight %}

#### Consider another example:

{% highlight js %}
var level = function(num) {
	return num > 40 ? "It's over 40!" : num;
};
module.exports = level;
{% endhighlight %}

When you connect the module using the require, in fact it will be the function that will allow you to do the following

{% highlight js %}
require('./index.js')(50);
{% endhighlight %}

What is a simplified recording the following code

{% highlight js %}
var level = require('./index.js')
level(50);
{% endhighlight %}
