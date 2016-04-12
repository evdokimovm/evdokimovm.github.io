---
layout: post
title: Generating Fake Data for your JavaScript App using Faker.js
categories: [JavaScript, NodeJS, Faker]
tags: [JavaScript, NodeJS, Faker]
fullview: false
comments: true
description: When we create a JavaScript web application, we need to have some data to demonstrate the application in action. But what if the database is not available or do not have time to fill it manually? Marak has created a convenient tool that allows us to generate fake data quickly Faker.js
---

When we create a JavaScript web application, we need to have some data to demonstrate the application in action. But what if the database is not available or do not have time to fill it manually?

[Marak](https://github.com/Marak) has created a convenient tool that allows us to generate fake data quickly: [Faker.js](https://github.com/Marak/faker.js).

#### So, let's begin. First, a simple example.

Supposing, we are developing the application "Catalogue of Books". We need to quickly and easily create a sample book from the catalog. We want to have the book was the name of the author picture of the author, the book release date, the image on the cover of the book, price and description.

In this case, we can use Faker.js:

{% highlight js %}
var book = {
    title: faker.lorem.words(),
    author: faker.name.findName(),
    author_image: faker.image.avatar(),
    release_date: faker.date.recent(),
    image: faker.image.abstract(),
    price: faker.commerce.price(),
    short_description: faker.lorem.sentence(),
    long_description: faker.lorem.paragraph()
}
{% endhighlight %}

We now have a book in the form of an object, which has all the desired properties:

{% highlight json %}
{
    "title": "praesentium at et",
    "author": "Brett Osinski",
    "author_image": "https://s3.amazonaws.com/uifaces/faces/twitter/xravil/128.jpg",
    "release_date": "2016-04-10T16:12:46.276Z",
    "image": "http://lorempixel.com/640/480/abstract",
    "price": "8.00",
    "short_description": "Voluptatem eaque velit numquam et esse rerum.",
    "long_description": "Quasi odio eveniet necessitatibus vitae ..."
}
{% endhighlight %}

#### Usage Faker.js

You can use Faker.js in the browser and on the server use Node.js.
Install Faker.js with npm `npm install faker`

* In Browser

1. `npm install faker`
2. Create HTML File and Include Faker:

{% highlight html %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <script type="text/javascript" src="node_modules/faker/build/build/faker.js"></script>
</head>
<body>
    <script type="text/javascript">
        var book = {
            title: faker.lorem.words(),
            author: faker.name.findName(),
            author_image: faker.image.avatar(),
            release_date: faker.date.recent(),
            image: faker.image.abstract(),
            price: faker.commerce.price(),
            short_description: faker.lorem.sentence(),
            long_description: faker.lorem.paragraph()
        }

        console.log(book);
    </script>
</body>
</html>
{% endhighlight %}

* In Node.js

1. `npm install faker`
2. Create your `index.js` file and `require` faker:

{% highlight js %}
var faker = require('faker');

var book = {
    title: faker.lorem.words(),
    author: faker.name.findName(),
    author_image: faker.image.avatar(),
    release_date: faker.date.recent(),
    image: faker.image.abstract(),
    price: faker.commerce.price(),
    short_description: faker.lorem.sentence(),
    long_description: faker.lorem.paragraph()
}

console.log(book);
{% endhighlight %}

#### Faker.js Data

List of data that can generate Faker.js:

* address
* commerce
* company
* date
* finance
* hacker
* helpers
* image
* internet
* lorem
* name
* phone
* random
* system

Each element has a lot of sub-items:

* address
    - zipCode
    - city
    - cityPrefix
    - etc ...
* commerce
    - color
    - department
    - productName
    - etc ...
* etc ...

A full list of the data you can find here [https://github.com/Marak/faker.js#api-methods](https://github.com/Marak/faker.js#api-methods)

#### Faker.js Helpers

Faker.js also has "helpers" who represent a ready-made "templates" with the data. I will give a couple of examples:

* Contextual Card

`faker.helpers.contextualCard()`

{% highlight json %}
{
    "name": "Susie",
    "username": "Susie_Greenfelder2",
    "avatar": "https://s3.amazonaws.com/uifaces/faces/twitter/ffbel/128.jpg",
    "email": "Susie_Greenfelder2_Rice@gmail.com",
    "dob": "1978-02-11T13:42:19.879Z",
    "phone": "1-953-835-6721 x423",
    "address": {
        "street": "Kuhic Parkways",
        "suite": "Apt. 627",
        "city": "Weberchester",
        "zipcode": "53222-1314",
        "geo": {
            "lat": "-69.8378",
            "lng": "-59.7793"
        }
    },
    "website": "joey.info",
    "company": {
        "name": "Hansen - Kuhic",
        "catchPhrase": "Optional intangible ability",
        "bs": "granular monetize networks"
    }
}
{% endhighlight %}

* User Card

`faker.helpers.userCard()`

{% highlight json %}
{
    "name": "Madeline Breitenberg",
    "username": "Santina_Hackett",
    "email": "Eldred3@hotmail.com",
    "address": {
        "street": "Hershel Mills",
        "suite": "Apt. 034",
        "city": "Horaciostad",
        "zipcode": "40886",
        "geo": {
            "lat": "35.5782",
            "lng": "-47.5731"
        }
    },
    "phone": "660.904.9057 x449",
    "website": "mertie.net",
    "company": {
        "name": "Deckow and Sons",
        "catchPhrase": "Visionary multimedia functionalities",
        "bs": "cutting-edge innovate e-services"
    }
}
{% endhighlight %}

* Create Transaction

`faker.helpers.createTransaction()`

{% highlight json %}
{
    "amount": "52.00",
    "date": "2012-02-01T13:00:00.000Z",
    "business": "Auer Group",
    "name": "Money Market Account 4685",
    "type": "withdrawal",
    "account": "13540266"
}
{% endhighlight %}

#### NodeJS JSON API Simple Template

We can quickly create a simple Node API, which returns the fake data.

* Install ExpressJS, CORS and Faker use `npm install express cors faker`

* Create Simple API Server:

{% highlight js %}
var express = require('express'),
    cors = require('cors'),
    app = express(),
    faker = require('faker');

app.set('port', process.env.PORT || 3500);

app.use(cors());

app.get('/api', function(req, res) {
    res.json([
        {
            title: faker.lorem.words(),
            author: faker.name.findName(),
            author_image: faker.image.avatar(),
            release_date: faker.date.recent(),
            image: faker.image.abstract(),
            price: faker.commerce.price(),
            short_description: faker.lorem.sentence(),
            long_description: faker.lorem.paragraph()
        }
    ])
});

var server = app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});
{% endhighlight %}

* Run Server with `node server.js`

{% highlight text %}
Server up: http://localhost:3500
{% endhighlight %}

* You Can Use [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?utm_source=chrome-ntp-icon) Google Chrome Extension to see the result. To do this, open Postman and send GET Request to `http://localhost:3500/api`

![POSTMAN](http://i.imgur.com/6uB5qvU.png)
