---
layout: post
title: AngularJS Geolocation Application. Part 2 - HTML5 Geolocation API
categories: [JavaScript, AngularJS, HTML5, Geolocation, JSON, API]
tags: [JavaScript, AngularJS, HTML5, Geolocation, JSON, API]
fullview: false
comments: true
description: AngularJS Geolocation Application. Part 2 - HTML5 Geolocation API. This article is an addition to my previous article AngularJS Geolocation Application Step by Step In this article I will tell how to use HTML5 Geolocation API for get geolocation data. Latitude and Longitude.
---

This article is an addition to my previous article [AngularJS Geolocation Application Step by Step](http://evdokimovm.github.io/javascript/angularjs/leaflet/geolocation/api/2016/01/07/AngularJS-Geolocation-Web-Application-Step-by-Step.html)
In this article I will tell how to use HTML5 Geolocation API for get geolocation data. Latitude and Longitude.

Replace code in ```js/location.js``` with

{% highlight js %}
app.factory('myCoordinates', ['$q', function myCoordinates($q) {

	var deferred = $q.defer();

	// Check your browser support HTML5 Geolocation API
	if (window.navigator && window.navigator.geolocation) {
		window.navigator.geolocation.getCurrentPosition(getCoordinates);
	} else {
		deferred.reject({msg: "Browser does not supports HTML5 geolocation"});
	}

	function getCoordinates(coordinates){
		var myCoordinates = {};
		myCoordinates.lat = coordinates.coords.latitude;
		myCoordinates.lng = coordinates.coords.longitude;
		deferred.resolve(myCoordinates);
	}

	return deferred.promise;

}])
{% endhighlight %}
