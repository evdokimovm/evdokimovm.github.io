---
layout: post
title: Send SMS with NodeJS and Twilio API
categories: [JavaScript, NodeJS, Twilio, API]
tags: [JavaScript, NodeJS, Twilio, API]
fullview: false
comments: true
description: Send SMS with NodeJS and Twilio API. Twilio allows software developers to programmatically make and receive phone calls and send and receive text messages using its web service APIs
---

[Twilio](https://www.twilio.com/) allows software developers to programmatically make and receive phone calls and send and receive text messages using its web service APIs

To get started you need to Sing Up. Visit the official site Twilio — [https://www.twilio.com/](https://www.twilio.com/) and click to [Get a Free API Key](https://www.twilio.com/try-twilio)

![TWILIO_SIGN_UP](https://i.imgur.com/DD9Brg8.png)

complete registration form

![TWILIO_SIGN_UP_FORM](https://i.imgur.com/MZyemT9.png)

confirm your phone number

![TWILIO_CONFIRM_PHONE_NUMBER_1](https://i.imgur.com/bAcVk5i.png)

![TWILIO_CONFIRM_PHONE_NUMBER_2](https://i.imgur.com/TYa69Sb.png)

and get your first twilio phone number

![TWILIO_FIRST_PHONE_NUMBER_1](https://i.imgur.com/4yXLTuK.png)

![TWILIO_FIRST_PHONE_NUMBER_2](https://i.imgur.com/RdOLwgC.png)

![TWILIO_FIRST_PHONE_NUMBER_3](https://i.imgur.com/Wy3DAQT.png)

Now you can go to the page [Get Started with SMS](https://www.twilio.com/console/sms/basics) and to practice.

> If before sending SMS you will get a [Error - 21408](https://www.twilio.com/docs/api/errors/21408) "Permission to send an SMS has not been enabled for the region indicated by the 'To' number". Just click on the following link [https://www.twilio.com/user/account/settings/international/sms](https://www.twilio.com/user/account/settings/international/sms) and apply the appropriate for you Geographic Permissions

Once you have finished with the Get Started with SMS and [got your phone number](https://i.imgur.com/gf0Ujid.png), You can go to the console where you will get your Twilio Credentials [https://www.twilio.com/console](https://www.twilio.com/console)

![TWILIO_CREDENTIALS](https://i.imgur.com/2mTgSWJ.png)

Now you can use your credentials in the following code replacing the corresponding values as you want

`server.js`

{% highlight js %}
// Twilio Credentials 
var accountSid = 'accountSid';
var authToken = 'authToken';

// Require the Twilio module and create a REST client
var client = require('twilio')(accountSid, authToken);

client.messages.create({
	to: 'ToNumber', // You can only send messages to verified numbers in trial
	from: 'FromNumber', // https://i.imgur.com/gf0Ujid.png
	body: 'Text'
}, function(err, message) {
	console.log(message.sid);
});
{% endhighlight %}

install [twilio package](https://www.npmjs.com/package/twilio) with `npm install twilio` and run this code with `node server.js`
