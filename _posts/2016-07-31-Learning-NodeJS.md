---
layout: post
title: Learning NodeJS
categories: [JavaScript, NodeJS, ExpressJS]
tags: [JavaScript, NodeJS, ExpressJS]
fullview: false
comments: true
description: Learning NodeJS. Hello World. Sum of Command Line Arguments. Sync and Async Input Output. Filtered LS by the extension of the files. Modules. HTTP GET Request. HTTP Client. Time Server. Unix Timestamp. HTTP JSON API Time Server
---

### Hello World

Write a program that prints the text "HELLO WORLD" to the console

{% highlight js %}
console.log('HELLO WORLD');
{% endhighlight %}

### Baby Steps

Write a program that accepts one or more numbers as command-line arguments
and prints the sum of those numbers to the console (stdout)

{% highlight js %}
var sum = 0;
for (i = 2; i < process.argv.length; i++) {
	sum += Number(process.argv[i]);
}

console.log(sum);
{% endhighlight %}

### My First I/O

Write a program that uses a single synchronous filesystem operation to
read a file and print the number of newlines (\n) it contains to the
console (stdout), similar to running cat file | wc -l

The full path to the file to read will be provided as the first
command-line argument (i.e., process.argv[2]). You do not need to make
your own test file

{% highlight js %}
var fs = require('fs');
lines = fs.readFileSync(process.argv[2]).toString().split('\n').length - 1;

console.log(lines);
{% endhighlight %}

### My First Async I/O

Write a program that uses a single asynchronous filesystem operation to
read a file and print the number of newlines it contains to the console
(stdout), similar to running cat file | wc -l

The full path to the file to read will be provided as the first
command-line argument

{% highlight js %}
var fs = require('fs');
fs.readFile(process.argv[2], 'utf-8', function(err, data) {
	console.log(data.split('\n').length - 1)
})
{% endhighlight %}

### Filtered LS

Create a program that prints a list of files in a given directory,
filtered by the extension of the files

{% highlight js %}
var fs = require('fs');
var path = require('path');
// var extension = RegExp('.' + process.argv[3]);
var extension = '.' + process.argv[3];
fs.readdir(process.argv[2], function(err, files) {
	if (err) throw err;

	for (var i = 0; i < files.length; i++) {
		/*if (extension.test(files[i])) {
			console.log(files[i]);
		}*/
		if (path.extname(files[i]) === extension) {
			console.log(files[i])
		}
	}
})
{% endhighlight %}

### Make it Modular

server.js

{% highlight js %}
var lsmodule = require('./index');

var dirname = process.argv[2];
var ext = process.argv[3];

lsmodule(dirname, ext, function(err, files) {
	for (i = 0; i < files.length; i++) {
		console.log(files[i]);
	}
})
{% endhighlight %}

index.js

{% highlight js %}
var fs = require('fs');
var path = require('path');

module.exports = function(dirname, ext, callback) {
	var extension = '.' + ext;
	fs.readdir(dirname, function(err, files) {
		if (err) {
			callback(err, null);
		} else {
			result = [];
			files.forEach(function(entry) {
				if (path.extname(entry) == extension) {
					result.push(entry);
				}
			})
			callback(null, result);
		}
	})
}
{% endhighlight %}

### HTTP Client

Write a program that performs an HTTP GET request to a URL provided to you
as the first command-line argument. Write the String contents of each
"data" event from the response to a new line on the console (stdout)

{% highlight js %}
var http = require('http');

http.get(process.argv[2], function(request) {
	request.setEncoding('utf8');
	request.on('data', function(data) {
		console.log(data);
	})
})
{% endhighlight %}

### HTTP Collect

Write a program that performs an HTTP GET request to a URL provided to you
as the first command-line argument. Collect all data from the server (not
just the first "data" event) and then write two lines to the console
(stdout)

The first line you write should just be an integer representing the number
of characters received from the server. The second line should contain the
complete String of characters sent by the server

{% highlight js %}
var http = require('http');

http.get(process.argv[2], function(request) {
	var result = '';
	request.setEncoding('utf8');
	request.on('data', function(data) {
		result += data;
	})
	request.on('end', function() {
		console.log(result.length);
		console.log(result);
	})
})
{% endhighlight %}

### Juggling Async

This problem is the same as the previous problem (HTTP COLLECT) in that
you need to use http.get(). However, this time you will be provided with
three URLs as the first three command-line arguments

You must collect the complete content provided to you by each of the URLs
and print it to the console (stdout). You don't need to print out the
length, just the data as a String; one line per URL. The catch is that you
must print them out in the same order as the URLs are provided to you as
command-line arguments

{% highlight js %}
var http = require('http');
var bl = require('bl');
var results = [];
var count = 0;

function printResults() {
	for (var i = 0; i < 3; i++) {
		console.log(results[i])
	}
}

for (var i = 0; i < process.argv.length - 2; i++) {
	httpGet(i)
}

function httpGet(index) {
	http.get(process.argv[2 + index], function(response) {
		response.pipe(bl(function(err, data) {
			if (err) throw err;

			results[index] = data.toString()
			count++

			if (count == 3) {
				printResults()
			}
		}))
	})
}
{% endhighlight %}

### Time Server

Write a TCP time server!

Your server should listen to TCP connections on the port provided by the
first argument to your program. For each connection you must write the
current date & 24 hour time in the format:

`"YYYY-MM-DD hh:mm"`

followed by a newline character. Month, day, hour and minute must be
zero-filled to 2 integers. For example:

`"2013-07-06 17:42"`

After sending the string, close the connection

{% highlight js %}
var net = require('net');

function verify(n) {
	return (n < 10 ? '0' + n : n)
}

var server = net.createServer(function(socket) {
	d = new Date();
	s = d.getFullYear() + "-"
		+ verify(d.getMonth() + 1) + "-"
		+ verify(d.getDate()) + " "
		+ verify(d.getHours()) + ":"
		+ verify(d.getMinutes()) + "\n";
	socket.end(s);
})
server.listen(process.argv[2])
{% endhighlight %}

### HTTP File Server

Write an HTTP server that serves the same text file for each request it
receives

Your server should listen on the port provided by the first argument to
your program

You will be provided with the location of the file to serve as the second
command-line argument. You must use the fs.createReadStream() method to
stream the file contents to the response

{% highlight js %}
var fs = require('fs');
var http = require('http');

server = http.createServer(function(req, res) {
	fs.createReadStream(process.argv[3]).pipe(res);
})
server.listen(process.argv[2])
{% endhighlight %}

### HTTP Uppercaserer

Write an HTTP server that receives only POST requests and converts
incoming POST body characters to upper-case and returns it to the client

Your server should listen on the port provided by the first argument to
your program

{% highlight js %}
var http = require('http');

var map = require('through2-map');

server = http.createServer(function(req, res) {
	if (req.method == 'POST') {
		req.pipe(map(function(chunk) {
			return chunk.toString().toUpperCase();
		})).pipe(res);
	}
})
server.listen(process.argv[2])
{% endhighlight %}

### HTTP JSON API Server

Write an HTTP server that serves JSON data when it receives a GET request
to the path '/api/parsetime'. Expect the request to contain a query string
with a key 'iso' and an ISO-format time as the value

For example:

`/api/parsetime?iso=2013-08-10T12:10:15.474Z`

The JSON response should contain only 'hour', 'minute' and 'second'
properties. For example:

{% highlight json %}
{
	"hour": 14,
	"minute": 23,
	"second": 15
}
{% endhighlight %}

Add second endpoint for the path '/api/unixtime' which accepts the same
query string but returns UNIX epoch time in milliseconds (the number of
milliseconds since 1 Jan 1970 00:00:00 UTC) under the property 'unixtime'.
For example:

{% highlight json %}
{
	"unixtime": 1376136615474
}
{% endhighlight %}

Your server should listen on the port provided by the first argument to
your program

Without [ExpressJS](https://www.npmjs.com/package/express)

{% highlight js %}
var http = require('http')
var url = require('url')

function parsetime(time) {
	return {
		hour: time.getHours(),
		minute: time.getMinutes(),
		second: time.getSeconds()
	}
}

function unixtime(time) {
	return { unixtime: time.getTime() }
}

var server = http.createServer(function(req, res) {
	var parsedUrl = url.parse(req.url, true);
	var time = new Date(parsedUrl.query.iso);
	var result;

	if (/^\/api\/parsetime/.test(req.url)) {
		result = parsetime(time)
	} else if (/^\/api\/unixtime/.test(req.url)) {
		result = unixtime(time)
	}

	if (result) {
		res.writeHead(200, { 'Content-Type': 'application/json' })
		res.end(JSON.stringify(result))
	} else {
		res.writeHead(404)
		res.end()
	}
})
server.listen(process.argv[2])
{% endhighlight %}

With use [ExpressJS](https://www.npmjs.com/package/express)

{% highlight js %}
var express = require('express');
var	cors = require('cors');
var	app = express();

app.set('port', process.env.PORT || process.argv[2]);

app.use(cors());

app.get('/api/parsetime', function(req, res) {
	d = new Date(req.query.iso);
	res.json(
		{
			hour: d.getHours(),
			minute: d.getMinutes(),
			second: d.getSeconds()
		}
	)
})

app.get('/api/unixtime', function(req, res) {
	res.json(
		{
			unixtime: Math.floor((new Date(req.query.iso).getTime()))
		}
	)
})

var server = app.listen(app.get('port'), function() {
	console.log('Server up: http://localhost:' + app.get('port'));
})
{% endhighlight %}
