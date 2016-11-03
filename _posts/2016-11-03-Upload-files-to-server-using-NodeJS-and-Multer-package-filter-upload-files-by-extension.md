---
layout: post
title: Upload files to server using Node.js and Multer package
categories: [JavaScript, NodeJS, ExpressJS, Multer]
tags: [JavaScript, NodeJS, ExpressJS, Multer]
fullview: false
comments: true
description: How to upload files to server using Node.js and multer package from npm and filter upload files by extension
---

In this article I will tell about how to upload files to server using Node.js and [multer](https://npmjs.org/package/multer) package from npm and filter upload files by extension

First, install all dependencies

{% highlight text %}
npm install --save express ejs multer
{% endhighlight %}

and create simple express server with render `index.ejs`

`server.js`

{% highlight js %}
var express = require("express")
var ejs = require('ejs')
var app = express()

app.set('view engine', 'ejs')

app.get('/api/file', function(req, res) {
	res.render('index')
})

var port = process.env.PORT || 8080
app.listen(port, function() {
	console.log('Node.js listening on port ' + port)
})
{% endhighlight %}

`views/index.ejs`

{% highlight html %}
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title></title>
</head>
<body>
	<form id="uploadForm" enctype="multipart/form-data" method="post">
		<input type="file" name="userFile" />
		<input type="submit" value="Upload File" name="submit">
	</form>
</body>
</html>
{% endhighlight %}

Now you can run server, open url `http://localhost:8080/api/file` in your browser and see form to choose uploaded file.

Add to `server.js` file new dependencies: [path](https://nodejs.org/api/path.html) and multer

{% highlight js %}
var path = require('path')
var multer = require('multer')
{% endhighlight %}

and add config object

{% highlight js %}
var storage = multer.diskStorage({
	destination: function(req, file, callback) {
		callback(null, './uploads')
	},
	filename: function(req, file, callback) {
		console.log(file)
		callback(null, file.fieldname + '-' + Date.now() + path.extname(file.originalname))
	}
})
{% endhighlight %}

There are two options available, `destination` and `filename`.

`destination` is to specify the path to the directory where uploaded files will be stored.

`filename` is used to determine what the file should be named inside the folder. In this case, we upload the file using a return number value from `Date.now()` instead of the original name and add a original file extension using the built-in [path](https://nodejs.org/api/path.html) Node.js library.

{% highlight js %}
path.extname(file.originalname)
{% endhighlight %}

Thus, the file will be uploaded to the server with original extension.

Now we need to create a `POST` route that will send the file to the server. Add following code snippet after `GET` route.

{% highlight js %}
app.post('/api/file', function(req, res) {
	var upload = multer({
		storage: storage
	}).single('userFile')
	upload(req, res, function(err) {
		res.end('File is uploaded')
	})
})
{% endhighlight %}

Here multer used settings from `storage` object we was written above and send the file to the server. If all goes well, you will see the message `File is uploaded`. You can try it.

#### Filter upload by file extension

At the moment, we can upload the file to the server with any extension. But what if we want to allow to upload only image files?

To control which files should be uploaded and which should be skipped in multer module provides `fileFilter` function.

Let's add this feature.

Add this code snippet

{% highlight text %}
fileFilter: function(req, file, callback) {
	var ext = path.extname(file.originalname)
	if (ext !== '.png' && ext !== '.jpg' && ext !== '.gif' && ext !== '.jpeg') {
		return callback(res.end('Only images are allowed'), null)
	}
	callback(null, true)
}
{% endhighlight %}

to `POST` route after `storage` property like this

{% highlight js %}
app.post('/api/file', function(req, res) {
	var upload = multer({
		storage: storage,
		fileFilter: function(req, file, callback) {
			var ext = path.extname(file.originalname)
			if (ext !== '.png' && ext !== '.jpg' && ext !== '.gif' && ext !== '.jpeg') {
				return callback(res.end('Only images are allowed'), null)
			}
			callback(null, true)
		}
	}).single('userFile')
	upload(req, res, function(err) {
		res.end('File is uploaded')
	})
})
{% endhighlight %}

Now you can upload only image files only with `png`, `jpg`, `gif` and `jpeg` extensions.

Full code

`server.js`

{% highlight js %}
var express = require("express")
var multer = require('multer')
var app = express()
var path = require('path')

var ejs = require('ejs')
app.set('view engine', 'ejs')

app.get('/api/file', function(req, res) {
	res.render('index')
})

var storage = multer.diskStorage({
	destination: function(req, file, callback) {
		callback(null, './uploads')
	},
	filename: function(req, file, callback) {
		callback(null, file.fieldname + '-' + Date.now() + path.extname(file.originalname))
	}
})

app.post('/api/file', function(req, res) {
	var upload = multer({
		storage: storage,
		fileFilter: function(req, file, callback) {
			var ext = path.extname(file.originalname)
			if (ext !== '.png' && ext !== '.jpg' && ext !== '.gif' && ext !== '.jpeg') {
				return callback(res.end('Only images are allowed'), null)
			}
			callback(null, true)
		}
	}).single('userFile');
	upload(req, res, function(err) {
		res.end('File is uploaded')
	})
})

var port = process.env.PORT || 8080
app.listen(port, function() {
	console.log('Node.js listening on port ' + port)
})
{% endhighlight %}

`views/index.ejs`

{% highlight html %}
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title></title>
</head>
<body>
	<form id="uploadForm" enctype="multipart/form-data" method="post">
		<input type="file" name="userFile" />
		<input type="submit" value="Upload File" name="submit">
	</form>
</body>
</html>
{% endhighlight %}
