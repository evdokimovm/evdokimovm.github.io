---
layout: post
title: Write Data to Local JSON File using Node.js
categories: [JavaScript, NodeJS]
tags: [JavaScript, NodeJS]
fullview: false
comments: true
description: Write Data to Local JSON File using Node.js
---

First, we need to have a JSON file with empty array or existing array with objects. Suppose this `users.json`:

{% highlight json %}
{
	"users": []
}
{% endhighlight %}

For append new content to `users.json` file we need to read the file and convert it contents to an JavaScript object using `JSON.parse()` method:

{% highlight js %}
var fs = require('fs')

fs.readFile('./users.json', 'utf-8', function(err, data) {
	if (err) throw err

	var arrayOfObjects = JSON.parse(data)
})
{% endhighlight %}

Now `arrayOfObjects` variable contains a object from `users.json` file:

{% highlight js %}
console.log(arrayOfObjects)
// Output: { users: [] }
{% endhighlight %}

Now we can write the data into an `users` array using the `push()` method is as follows:

{% highlight js %}
var fs = require('fs')

fs.readFile('./users.json', 'utf-8', function(err, data) {
	if (err) throw err

	var arrayOfObjects = JSON.parse(data)
	arrayOfObjects.users.push({
		name: "Mikhail",
		age: 24
	})

	console.log(arrayOfObjects)
})
{% endhighlight %}

Now we have an array with an object:

{% highlight js %}
console.log(arrayOfObjects)
// Output: { users: [ { name: 'Mikhail', age: 24 } ] }
{% endhighlight %}

You can appended any objects to `users` array.

After we have data in variable we need to write this into file:

{% highlight js %}
fs.writeFile('./users.json', JSON.stringify(arrayOfObjects), 'utf-8', function(err) {
	if (err) throw err
	console.log('Done!')
})
{% endhighlight %}

You may have noticed that here uses `JSON.stringify(arrayOfObjects)`. `JSON.stringify()` method convert the contents of `arrayOfObjects` into JSON.

Now `users.json` looks like this:

{% highlight json %}
{
	"users": [
		{
			"name": "Mikhail",
			"age": 24
		}
	]
}
{% endhighlight %}

Full Code:

`index.js`

{% highlight js %}
var fs = require('fs')

fs.readFile('./users.json', 'utf-8', function(err, data) {
	if (err) throw err

	var arrayOfObjects = JSON.parse(data)
	arrayOfObjects.users.push({
		name: "Mikhail",
		age: 24
	})

	console.log(arrayOfObjects)

	fs.writeFile('./users.json', JSON.stringify(arrayOfObjects), 'utf-8', function(err) {
		if (err) throw err
		console.log('Done!')
	})
})
{% endhighlight %}

`users.json`

{% highlight json %}
{
	"users": []
}
{% endhighlight %}
