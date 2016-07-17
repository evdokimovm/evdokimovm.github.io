---
layout: post
title: Connect, Save and Find Data in MongoDB with NodeJS and ExpressJS
categories: [JavaScript, NodeJS, MongoDB, ExpressJS]
tags: [JavaScript, NodeJS, MongoDB, ExpressJS]
fullview: false
comments: true
description: Connect, Save and Find Data in MongoDB with NodeJS and ExpressJS. In this article I'll tell you about how to Connect, Save and Find Data in MongoDB using NodeJS with Mongoose and MongoDB drivers
---

In this article I'll tell you about how to connect to the MongoDB using NodeJS with [Mongoose](https://npmjs.com/package/mongoose) and [MongoDB](https://npmjs.com/package/mongodb) drivers. As well as Save data and Find data in MongoDB.

For work on this post You need to install NodeJS and MongoDB

NodeJS — [https://nodejs.org](https://nodejs.org)

MongoDB — [https://www.mongodb.com](https://www.mongodb.com)

> If You do not want to install MongoDB You can use [mLab.com](https://mLab.com). This service provides 500 Mb of MongoDB for Free

First, we need to install all necessary packages for work. Create a `package.json` file

{% highlight json %}
{
	"name": "name",
	"main": "server.js",
	"dependencies" : {
		"express": "4.13.4",
		"mongoose": "4.5.0",
		"cors": "2.7.1"
	}
}
{% endhighlight %}

and run `npm install` to install packages.

Learn more about these packages you can on [NPM Website](https://www.npmjs.com)

Express — Fast, unopinionated, minimalist web framework — [https://www.npmjs.com/package/express](https://www.npmjs.com/package/express)

Mongoose — Mongoose is a MongoDB object modeling tool designed to work in an asynchronous environment — [https://www.npmjs.com/package/mongoose](https://www.npmjs.com/package/mongoose)

Cors — Middleware for dynamically or statically enabling CORS in express/connect applications — [https://www.npmjs.com/package/cors](https://www.npmjs.com/package/cors)

> There is also a [MongoDB](https://www.npmjs.com/package/mongodb) driver, I'll tell you about it in this article, but I prefer to use the [Mongoose](https://www.npmjs.com/package/mongoose) because it provides the opportunity to create the database schema

After successful install packages, in project folder create `server.js` file and in your terminal or command prompt run MongoDB with `mongod` command.

#### Use Mongoose Driver

`server.js`

{% highlight js %}
var express = require('express');
var cors = require('cors');
var mongoose = require('mongoose');
var Schema = mongoose.Schema;

var app = express();

var schemaName = new Schema({
	request: String,
	time: Number
}, {
	collection: 'collectionName'
});

var Model = mongoose.model('Model', schemaName);
mongoose.connect('mongodb://localhost:27017/dbName');

app.get('/save/:query', cors(), function(req, res) {
	var query = req.params.query;

	var savedata = new Model({
		'request': query,
		'time': Math.floor(Date.now() / 1000) // Time of save the data in unix timestamp format
	}).save(function(err, result) {
		if (err) throw err;

		if(result) {
			res.json(result)
		}
	})
})

app.get('/find/:query', cors(), function(req, res) {
	var query = req.params.query;

	Model.find({
		'request': query
	}, function(err, result) {
		if (err) throw err;
		if (result) {
			res.json(result)
		} else {
			res.send(JSON.stringify({
				error : 'Error'
			}))
		}
	})
})

var port = process.env.PORT || 8080;
app.listen(port, function() {
	console.log('Node.js listening on port ' + port);
});
{% endhighlight %}

#### Let's analyze this code for details

On lines 1 — 6 we include all the necessary dependencies

{% highlight js %}
var express = require('express');
var cors = require('cors');
var mongoose = require('mongoose');
var Schema = mongoose.Schema;

var app = express();
{% endhighlight %}

On lines 7 — 12 create database schema

> At the end of this article I will show how to use mongodb command line to access the data

{% highlight js %}
var schemaName = new Schema({
	request: String,
	time: Number
}, {
	collection: 'collectionName'
})
{% endhighlight %}

On lines 14 and 15 create model and connect to MongoDB

{% highlight js %}
var Model = mongoose.model('Model', schemaName);
mongoose.connect('mongodb://localhost:27017/dbName');
{% endhighlight %}

If you decide to use [mLab](https://mLab.com) just replace `mongodb://localhost:27017/dbName` with database URL. For example:

{% highlight js %}
mongoose.connect('mongodb://dbuser:dbpassword@ds055555.mlab.com:55555/dbName');
{% endhighlight %}

On lines 18 — 31 is implemented a route which we will use to insert data to the database

{% highlight js %}
app.get('/save/:query', cors(), function(req, res) {
	var query = req.params.query;

	var savedata = new Model({
		'request': query,
		'time': Math.floor(Date.now() / 1000)
	}).save(function(err, result) {
		if (err) throw err;

		if(result) {
			res.json(result)
		}
	})
})
{% endhighlight %}

Here, to variable `query` input request `/:query` which then stored to a database into `request` key

{% highlight js %}
var savedata = new Model({
	'request': query,
	...
{% endhighlight %}

Then, if you save the data error has occurred, you will receive an error on the console. If all goes successful you will see the stored data in JSON format on the page.

{% highlight js %}
...
}).save(function(err, result) {
	if (err) throw err;

	if(result) {
		res.json(result)
	}
})
{% endhighlight %}

On lines 33 — 48 has a function find for search data in database. In `request` key

{% highlight js %}
app.get('/find/:query', cors(), function(req, res) {
	var query = req.params.query;
 
	Model.find({
		'request': query
	}, function(err, result) {
		if (err) throw err;
		if (result) {
			res.json(result)
		} else {
			res.send(JSON.stringify({
				error : 'Error'
			}))
		}
	})
})
{% endhighlight %}

As in a `save` function, request is recorded to `query` variable `/:query` then passed to the function `find` to search

{% highlight js %}
Model.find({
	'request': query
{% endhighlight %}

If an error occurs when you search you will see it in the console. If all goes correctly you will get all the documents in JSON format containing the current query in `request` key

{% highlight js %}
}, function(err, result) {
	if (err) throw err;
	if (result) {
		res.json(result)
	} else {
		res.send(JSON.stringify({
			error : 'Error'
		}))
	}
})
{% endhighlight %}

Finally we create a server

{% highlight js %}
var port = process.env.PORT || 8080;
app.listen(port, function() {
	console.log('Node.js listening on port ' + port);
})
{% endhighlight %}

#### Usage

To save you can just go in your browser to the following URL

{% highlight text %}
http://localhost:8080/save/:query
{% endhighlight %}

where `:query` it's your request for save. Example:

{% highlight text %}
http://localhost:8080/save/JavaScript is Awesome
{% endhighlight %}

Output

![SAVE](http://i.imgur.com/Vqn0VjQ.png)

> For convenient viewing of JSON in your browser, you can use [JSONView Chrome Extension](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?utm_source=chrome-ntp-icon)

For use find you can just go to the browser to the following URL

{% highlight text %}
http://localhost:8080/find/:query
{% endhighlight %}

where `:query` it’s your request for find. Example:

{% highlight text %}
http://localhost:8080/find/JavaScript is Awesome
{% endhighlight %}

Output

![FIND](http://i.imgur.com/V3FxGI6.png)

#### Use MongoDB Driver

First, Install mongodb driver with `npm install mongodb`. And replace code in your `server.js` file with:

{% highlight js %}
var express = require('express');
var cors = require('cors');
var mongo = require('mongodb');

var app = express();

mongo.connect('mongodb://localhost:27017/dbName', function(err, db) {
	db.createCollection('collectionName');
	var collectionName = db.collection('collectionName');

	var saveObject = {};

	app.get('/save/:query', cors(), function(req, res) {
		var query = req.params.query;

		saveObject = {
			"request": query,
			"time": Math.floor(Date.now() / 1000)
		}

		res.send(saveObject)

		collectionName.save(saveObject, function(err) {
			if (err) throw err;
		})
	})

	app.get('/find/:query', cors(), function(req, res) {
		var query = req.params.query;

		collectionName.find({'request': query}).toArray(function (err, result) {
			if (err) throw err;

			res.send(result);
		})

	})

})

var port = process.env.PORT || 8080;
app.listen(port, function() {
	console.log('Node.js listening on port ' + port);
})
{% endhighlight %}

Quickly analyze that code

MongoDB Driver

{% highlight js %}
var mongo = require('mongodb');
{% endhighlight %}

Connect and create collection

{% highlight js %}
mongo.connect('mongodb://localhost:27017/dbName', function(err, db) {
	db.createCollection('collectionName');
	var collectionName = db.collection('collectionName');

	...

	...
})
{% endhighlight %}

Save

{% highlight js %}
saveObject = {
	"request": query,
	"time": Math.floor(Date.now() / 1000)
}

res.send(saveObject)

collectionName.save(saveObject, function(err) {
	if (err) throw err;
})
{% endhighlight %}

Find

{% highlight js %}
collectionName.find({'request': query}).toArray(function (err, result) {
	if (err) throw err;

	res.send(result);
})
{% endhighlight %}

#### MongoDB Command Line Interface

Open another terminal and run the command `mongo` to open MongoDB CLI

{% highlight text %}
\>mongo
MongoDB shell version: 3.2.0
connecting to: test
\>_
{% endhighlight %}

To see a list of existing databases, use the command `show dbs`

{% highlight text %}
\>mongo
MongoDB shell version: 3.2.0
connecting to: test
\> show dbs
dbName  0.000GB
local   0.000GB
\>_
{% endhighlight %}

In this article, as the database name I used a `dbName`. To use this database, run the command `use dbName`

{% highlight text %}
\>mongo
MongoDB shell version: 3.2.0
connecting to: test
\> show dbs
dbName  0.000GB
local   0.000GB
\> use dbName
switched to db dbName
\>_
{% endhighlight %}

Show collections with `show collections`

{% highlight text %}
\>mongo
MongoDB shell version: 3.2.0
connecting to: test
\> show dbs
dbName  0.000GB
local   0.000GB
\> use dbName
switched to db dbName
\> show collections
collectionName
\>_
{% endhighlight %}

We see that in the database `dbName` have a collection named` collectionName` which contains our data. Let's read this data using the command `db.collectionName.find().pretty()`

{% highlight text %}
\>mongo
MongoDB shell version: 3.2.0
connecting to: test
\> show dbs
dbName  0.000GB
local   0.000GB
\> use dbName
switched to db dbName
\> show collections
collectionName
\> db.collectionName.find().pretty()
{
		"_id" : ObjectId("578abe97522ad414b8eeb55a"),
		"request" : "JavaScript is Awesome",
		"time" : 1468710551
}
{
		"_id" : ObjectId("578abe9b522ad414b8eeb55b"),
		"request" : "JavaScript is Awesome",
		"time" : 1468710555
}
{
		"_id" : ObjectId("578abea0522ad414b8eeb55c"),
		"request" : "JavaScript is Awesome",
		"time" : 1468710560
}
\>_
{% endhighlight %}
