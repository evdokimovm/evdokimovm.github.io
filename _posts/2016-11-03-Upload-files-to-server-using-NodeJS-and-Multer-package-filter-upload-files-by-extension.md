---
layout: post
title: Upload files to server using Node.js and Multer package
categories: [JavaScript, NodeJS, ExpressJS, Multer]
tags: [JavaScript, NodeJS, ExpressJS, Multer]
fullview: false
comments: true
description: How to upload files to server using Node.js and multer package from npm and filter upload files by extension and validate file by check magic numbers.
---

In this article I will tell about how to upload files to server using Node.js and [multer](https://npmjs.org/package/multer) package from npm, filter upload files by extension and validate file by check magic numbers.

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

Now you can upload files only with `png`, `jpg`, `gif` and `jpeg` extensions.

Full code:

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

#### Edited: April 23, 2017. Validate Files by Check Magic Numbers.

**Comment by *Araik Martirosyan:*** *Hey Mikhail, I have a question. How I can validate uploade file, if i change file name and make this myscript.js.jpeg. I dont want this file save on my server side.*

**Answer**

To determine if a file is an valid we can read the first bytes of the stream and compare it with magic numbers [https://en.wikipedia.org/wiki/Magic_number_(programming)](https://en.wikipedia.org/wiki/Magic_number_(programming)). Since multer does not provide file data (we need bitmap in this case) as a solution we can check the magic numbers after uploading the file to the server and if it does not match then delete this file from filesystem.

Since In this article I give an example about the validation image files then the following example I will give about images.

First, declare an object that will contain magic numbers for the file types of interest to us:

{% highlight js %}
var MAGIC_NUMBERS = {
    jpg: 'ffd8ffe0',
    jpg1: 'ffd8ffe1',
    png: '89504e47',
    gif: '47494638'
}
{% endhighlight %}

then write simple function that we will use to check magic numbers:

{% highlight js %}
function checkMagicNumbers(magic) {
    if (magic == MAGIC_NUMBERS.jpg || magic == MAGIC_NUMBERS.jpg1 || magic == MAGIC_NUMBERS.png || magic == MAGIC_NUMBERS.gif) return true
}
{% endhighlight %}

add new [fs](https://nodejs.org/api/fs.html) dependency:

{% highlight js %}
var fs = require('fs')
{% endhighlight %}

and replace:

{% highlight js %}
upload(req, res, function(err) {
	res.end('File is uploaded')
})
{% endhighlight %}

with:

{% highlight js %}
upload(req, res, function(err) {
	var bitmap = fs.readFileSync('./uploads/' + req.file.filename).toString('hex', 0, 4)
	if (!checkMagicNumbers(bitmap)) {
		fs.unlinkSync('./uploads/' + req.file.filename)
		res.end('File is no valid')
	}
	res.end('File is uploaded')
})
{% endhighlight %}

at the moment we do not need `fileFilter` option and you can delete it.

Full code for this example:

`server.js`

{% highlight js %}
var express = require("express")
var multer = require('multer')
var app = express()
var path = require('path')
var fs = require('fs')

var ejs = require('ejs')
app.set('view engine', 'ejs')

var MAGIC_NUMBERS = {
	jpg: 'ffd8ffe0',
	jpg1: 'ffd8ffe1',
	png: '89504e47',
	gif: '47494638'
}

function checkMagicNumbers(magic) {
	if (magic == MAGIC_NUMBERS.jpg || magic == MAGIC_NUMBERS.jpg1 || magic == MAGIC_NUMBERS.png || magic == MAGIC_NUMBERS.gif) return true
}

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
		storage: storage
	}).single('userFile')
	upload(req, res, function(err) {
		var bitmap = fs.readFileSync('./uploads/' + req.file.filename).toString('hex', 0, 4)
		if (!checkMagicNumbers(bitmap)) {
			fs.unlinkSync('./uploads/' + req.file.filename)
			res.end('File is no valid')
		}
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
