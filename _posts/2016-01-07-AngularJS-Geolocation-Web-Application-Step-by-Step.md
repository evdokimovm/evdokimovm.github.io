---
layout: post
title: AngularJS Geolocation Application Step by Step
categories: [JavaScript, AngularJS, Leaflet, Geolocation, API]
tags: [JavaScript, AngularJS, Leaflet, Geolocation, API]
fullview: false
comments: true
description: AngularJS Geolocation Application Step by Step. In this article I will tell you about how to create an AngularJS Geolocation Single Page Application.
---

In this article I will tell you about how to create AngularJS Geolocation Single Page Application.

I'll start with the fact that tell what technologies we will use.

As the name implies the fundamental technology on which to build the entire application is AngularJS.

Also, we need ```Geolocation JSON API``` that will give us the ```latitude```, ```longitude```, ```city name``` and other location data. 
For these purposes, I decided to use ```JSON IP API``` [http://ip-api.com/json](http://ip-api.com/json)

As a card I'll use ```Leaflet```. ```Leaflet``` is an open-source JavaScript library
for mobile-friendly interactive maps. For more information about this JavaScript library, you can find the official website
[http://leafletjs.com/](http://leafletjs.com/)

It is also useful to us ```Leaflet.awesome-markers plugin``` and ```Wikipedia API```. But everything has its time.

#### So we can start

Start by creating a simple HTML page. At this stage, your ```index.html``` should look as follows:

{% highlight html %}
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>AngularJS Geolocation App</title>
	<link rel="stylesheet" type="text/css" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">
	<link rel="stylesheet" type="text/css" href="css/main.css">
</head>
	<body>
		<div class="header">
			<div class="container-fluid">
				<h1 class="pull-left">AngularJS Geolocation App</h1>
			</div>
		</div>
	</body>
</html>
{% endhighlight %}

and ```main.css``` should be:

{% highlight css %}
html, body {
	margin: 0;
	padding: 0;
	height: 100%;
	width: 100%;
}

.header {
	background-color: rgba(0, 0, 0, 0.3);
	position: absolute;
	width: 100%;
	z-index: 10;
	line-height: 37px;
}

.header .container-fluid {
	position: relative;
}

.header h1 {
	color: #fff;
	font-size: 16px;
	letter-spacing: 1.2px;
	margin: 0;
	padding: 10px 0 10px 55px;
}
{% endhighlight %}

Now let's create our application's About page. 

But first we should create a starting point for application, configuring routes as well as the controller for About page.

Now let's create ```js/app.js``` file and configuring our routes

{% highlight js %}
var app = angular.module('geoApp', ['ngRoute']);

app.config(function($routeProvider) {
	$routeProvider
	.when('/about', {
		controller: 'AboutController',
		templateUrl: 'views/about.html'
	})
});
{% endhighlight %}

```$routeProvider``` is AngularJS directive used for configuring routes.
We also connected the router as a dependency ```['ngRoute']``` for work with ```$routeProvider```

In addition, we must connect AngularJS framework for out page and angular-route module.
To do this, add this to our page within the tag ```<head>```

{% highlight html %}
<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.5/angular.min.js"></script>
<script type="text/javascript" src="https://code.angularjs.org/1.2.28/angular-route.min.js"></script>
{% endhighlight %}

now our ```body``` tag should look like this

{% highlight html %}
<body ng-app="geoApp">
{% endhighlight %}

This will be the starting point for our application.

Now we need to create a link to a page about, template and the about controller.

In your ```index.html``` file immediately after the line
{% highlight html %}
<h1 class="pull-left">AngularJS Geolocation App</h1>
{% endhighlight %}
you must to write link to page About
{% highlight html %}
<a class="pull-right" href="#/about">About</a>
{% endhighlight %}
Now add some style for this link in your ```main.css```
{% highlight css %}
.header a {
	padding-right: 20px;
	color: #fff;
}
{% endhighlight %}

Create a file ```js/controllers/AboutController.js```
This will our controller for About page:

{% highlight js %}
app.controller('AboutController', function() {

});
{% endhighlight %}
We must also not forget to connect our file on the page before the closing ```body``` tag

{% highlight html %}
<script type="text/javascript" src="js/app.js"></script>
<script type="text/javascript" src="js/controllers/AboutController.js"></script>
{% endhighlight %}

Now let's create the template for our About page to be displayed when we go to ```/#/about```. 
To do this, create a file ```views/about.html``` and paste the following code:

{% highlight html %}
<div class="about">
	<div class="container-fluid">
		<h1>AngularJS Geolocation App</h1>
		<h2>This AngularJS app shows your current location via the lat and lon coordinates and displays on the map and also shows some of the sights</h2>
		<a class="btn btn-primary" href="#/">Start exploring</a>
	</div>
</div>
{% endhighlight %}

And again, let's write some styles for this page:

{% highlight css %}
.about {
	text-align: center;
	position: relative;
	top: 100px;
}

.about h1 {
	font-size: 48px;
	font-weight: 300;
	margin: 42px 0;
}

.about h2 {
	font-size: 26px;
	margin: 42px 0;
}

.about .btn {
	font-weight: 600;
	border-radius: 0;
	padding: 16px 26px;
	font-size: 16px;
}
{% endhighlight %}

Since we are using ```ngRoute``` directive, for the rendering our pages we must to use ```ngView``` directive.
Go to the file ```index.html``` and paste immediately after

{% highlight html %}
<div class="header">
	<div class="container-fluid">
		<h1 class="pull-left">AngularJS Geolocation App</h1>
		<a class="pull-right" href="#/about">About</a>
	</div>
</div>
{% endhighlight %}

that code

{% highlight html %}
<div ng-view></div>
{% endhighlight %}

your code should look like this

{% highlight html %}
		...
	</div>
</div>

	<div ng-view></div>

<script type="text/javascript" src="js/app.js"></script>
<script type="text/javascript" src="js/controllers/AboutController.js"></script>
{% endhighlight %}

Then you can go to the application About page which should look like this:

![About](https://i.imgur.com/l7vLYLP.png)

#### After create the template, we can begin to create the map and display some of sights points and your location point

Start by creating a ```factory``` service in which we will receive information about the geolocation from ```JSON API```.
Create a file ```js/location.js``` and write:

{% highlight js %}
app.factory('myCoordinates', ['$q', '$http', function myCoordinates($q, $http) {

	var deferred = $q.defer();

	$http.get('http://ip-api.com/json')
		.success(function(coordinates) {
			var myCoordinates = {};
			myCoordinates.lat = coordinates.lat;
			myCoordinates.lng = coordinates.lon;
			myCoordinates.city = coordinates.city;
			deferred.resolve(myCoordinates);
	})

	return deferred.promise;

}])
{% endhighlight %}

```$q``` is a service that helps you run functions asynchronously, 
and use their return values (or exceptions) when they are done processing.

Next, with ```$http.get``` request we get ```latitude```, ```longitude```, and the name of ```city``` from JSON Geo API [http://ip-api.com/json](http://ip-api.com/json)

The data returned by this JSON API
{% highlight json %}
{
	"as": "as",
	"city": "city",
	"country": "country",
	"countryCode": "countryCode",
	"isp": "isp",
	"lat": "latitude",
	"lon": "longitude",
	"org": "org",
	"query": "query",
	"region": "region",
	"regionName": "regionName",
	"status": "success",
	"timezone": "timezone",
	"zip": "zip"
}
{% endhighlight %}

For convenient viewing of JSON in your browser, you can use [JSONView Chrome Extension](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc)

Now we need create a controller for ```Main``` page, template and configure routes in our ```js/app.js```.

I'll start with the configuration routes. Open your ```js/app.js``` file and replace it with following code:

{% highlight js %}
var app = angular.module('geoApp', ['ngRoute', 'leaflet-directive']);

app.config(function($routeProvider) {
	$routeProvider
	.when('/', {
		controller: 'MainController',
		templateUrl: 'views/main.html',
		resolve: {
			coordinates: function (myCoordinates) {
				return myCoordinates;
			}
		}
	})
	.when('/about', {
		controller: 'AboutController',
		templateUrl: 'views/about.html'
	})
	.otherwise({
		redirectTo: '/'
	});
	
});
{% endhighlight %}

You may have noticed that I have connected a new dependency ```[..., 'leaflet-directive'].```

This directive allows you to embed a map on your AngularJS application and interact 
bi-directionally with it via the AngularJS scope and the leaflet map library API

Official website of leaflet-directive [http://tombatossals.github.io/angular-leaflet-directive/](http://tombatossals.github.io/angular-leaflet-directive/)

```resolve:``` - An optional map of dependencies which should be injected into the controller. If any of these dependencies are promises, 
the router will wait for them all to be resolved or one to be rejected before the controller is instantiated. 
If all the promises are resolved successfully, the values of the resolved promises are 
injected and $routeChangeSuccess event is fired. [Cut from the official documentation AngularJS [https://docs.angularjs.org/api/ngRoute/provider/$routeProvider](https://docs.angularjs.org/api/ngRoute/provider/$routeProvider)]

Now, after we have finished the configuration of routes we can create MainController.
Create a ```js/controllers/MainController.js``` file and write:

{% highlight js %}
app.controller('MainController', ['$scope', 'coordinates', 'myCoordinates', function($scope, coordinates, myCoordinates) {
	
	$scope.mapCenter = {
		lat: coordinates.lat,
		lng: coordinates.lng,
		zoom: 14
	}

}])
{% endhighlight %}

About ```$scope.mapCenter``` you will understand when we create a templale for main page.

Create a ```views/main.html``` file and paste:

{% highlight html %}
<div class="main">
	<div class="container-fluid" id="map-canvas">
		<leaflet center="mapCenter"></leaflet>
	</div>
</div>
{% endhighlight %}

So, in our ```MainController```, we have created an ```$scope.mapCenter``` object in which the recorded data from the ```Geo JSON API```. 
That ```latlilude```, ```longitude``` and ```zoom``` (zoom not from API).
With that object we Initial geographical center of the map and "push" it to ```leaflet center="mapCenter"``` for visualize on map. 
In this case, is your approximate location. In the future, we will put marker on your location.

Let's add some style to ```css/main.css```:

{% highlight css %}
.angular-leaflet-map {
	position: absolute;
	top: 0;
	bottom: 0;
	right: 0;
	left: 0;
}

.container-fluid {
	padding-left: 0;
	padding-right: 0;
}

#map_canvas {
	position: relative;
}

#map_canvas {
	margin: 0;
	padding: 0;
	height: 100%;
	width: 100%;
}
{% endhighlight %}

Now, before you run and see application, we must connect the needed files

Connect our files on the page before the closing body tag:
{% highlight html %}
<script type="text/javascript" src="js/location.js"></script>
<script type="text/javascript" src="js/controllers/MainController.js"></script>
{% endhighlight %}
and add this libraries to our page within the tags head:
{% highlight html %}
<link rel="stylesheet" type="text/css" href="http://cdn.leafletjs.com/leaflet/v0.7.7/leaflet.css">
<script type="text/javascript" src="http://cdn.leafletjs.com/leaflet/v0.7.7/leaflet.js"></script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/angular-leaflet-directive/0.10.0/angular-leaflet-directive.min.js"></script>
{% endhighlight %}

Now you can run your application with command ```ws -p 8000``` [https://www.npmjs.com/package/local-web-server](https://www.npmjs.com/package/local-web-server)
and see it on your ```localhost:8000```

#### Wikipedia API and Sights Points Markers

Now, after we display a map in our application and showed on it 
your approximate location is the time to display some markers
of sights in your city. From ```Wikipedia API```.

This is no big deal.

For display coordinates of the markers is strictly on the map, we need a simple algorithm. 
Create new ```js/helper.js``` file and paste this code:

{% highlight js %}
var geodataToMarkers = function(geodata) {
	var places = geodata.query.geosearch;
	var markers = [];
	for(var i=0; i<places.length; i++) {
		place = {
			lat: places[i].lat,
			lng: places[i].lon,
			message: getMessage(places[i].title)
		}
		markers.push(place);
	}

	return markers;
}

var getMessage = function(title) {
	var url = "http://en.wikipedia.org/wiki/" + title;
	return "<a target='_blank' href='" + url + "'>" + title + "</a>";
}
{% endhighlight %}

This algorithm will continue to receive data from the ```Wikipedia API```
and display them on the map.

Just attach this file before the closing body tag

{% highlight html %}
<script type="text/javascript" src="js/helper.js"></script>
{% endhighlight %}

Open ```js/controllers/MainController.js``` and paste code after ```$scope.mapCenter``` object:

{% highlight js %}
var getSightsPoints = $http.jsonp('https://en.wikipedia.org/w/api.php?action=query&list=geosearch&gsradius=10000&gscoord=' + coordinates.lat + '%7C' + coordinates.lng + '&gslimit=30&format=json&callback=JSON_CALLBACK')
	.success(function(data) {
		return data;
	})

getSightsPoints.success(function(data){
	$scope.geodata = data;
	$scope.mapMarkers = geodataToMarkers($scope.geodata);
})
{% endhighlight %}

and add ```$http``` dependency. For that replace
{% highlight js %}
app.controller('MainController', ['$scope', 'coordinates', 'myCoordinates', function($scope, coordinates, myCoordinates) {
{% endhighlight %}
with
{% highlight js %}
app.controller('MainController', ['$scope', '$http', 'coordinates', 'myCoordinates', function($scope, $http, coordinates, myCoordinates) {
{% endhighlight %}

With ```var getSightsPoints = $http.jsonp('https ...``` we get data about some sights within a radius of 10000 meters
from your approximate location. For more information about the parameters specified in the request URL you can read here [https://www.mediawiki.org/wiki/Extension:GeoData](https://www.mediawiki.org/wiki/Extension:GeoData)

And this code

{% highlight js %}
...

getSightsPoints.success(function(data){
    $scope.geodata = data;
    $scope.mapMarkers = geodataToMarkers($scope.geodata);
})
{% endhighlight %}

push gets from Wikipedia API data to our ```js/helper.js``` from which our objects will be visible on the map.

But you need replace

{% highlight html %}
<leaflet center="mapCenter"></leaflet>
{% endhighlight %}
to
{% highlight html %}
<leaflet center="mapCenter" markers="mapMarkers"></leaflet>
{% endhighlight %}
in ```views/main.html``` file. Then you can see your application in action.

#### Add marker for your approximate location

The last thing we need to implement our application is a marker to show your approximate location. So proceed!

For that task we will use ```Leaflet.awesome-markers plugin``` [https://github.com/lvoogdt/Leaflet.awesome-markers](https://github.com/lvoogdt/Leaflet.awesome-markers)

Let's just add needed files for work with this plugin to our page within the tags head
{% highlight html %}
<link rel="stylesheet" type="text/css" href="https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css">
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.js"></script>
{% endhighlight %}

Open ```js/controllers/MainController.js``` and replace
{% highlight js %}
getSightsPoints.success(function(data){
	$scope.geodata = data;
	$scope.mapMarkers = geodataToMarkers($scope.geodata);
})
{% endhighlight %}
with
{% highlight js %}
getSightsPoints.success(function(data){
	$scope.geodata = data;
	var currentPositionPoint = {
		lat: coordinates.lat,
		lng: coordinates.lng,
		city: coordinates.city,
		focus: true,
		message:'Your approximate location in ' + coordinates.city + ' is lat: ' + coordinates.lat + ' and lon: ' + coordinates.lng + '. You can also see some of the sights within a radius of 10 kilometers.',
		icon: {
			type: 'awesomeMarker',
			icon: 'user',
			markerColor: 'blue',
			iconColor: 'white'
		}
	}
	$scope.mapMarkers = geodataToMarkers($scope.geodata);
	$scope.mapMarkers.push(currentPositionPoint);
})
{% endhighlight %}

Hooray! We ended up creating this application.
Now if you run this application, you should see something like this

![Final](https://i.imgur.com/wMo0Gfi.jpg)

#### In a separate article I will tell how to use the HTML5 Geolocation API
