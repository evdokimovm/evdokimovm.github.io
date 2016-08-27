---
layout: post
title: React.js Markdown Editor Step by Step
categories: [JavaScript, ReactJS, localStorage, WebStorage]
tags: [JavaScript, ReactJS, localStorage, WebStorage]
fullview: false
comments: true
description: React.js Markdown Editor Step by Step. In this article I will tell you about how to create Markdown Editor with React.js and Web Storage
---

In this article I will tell you about how to create Markdown Editor with React.js and Web Storage

Purpose of this article is not to teach you React.js from scratch, I'm only just going to show you how to create a specific project using React.js and explain the code

If you want to know more about React.js and explore this technology, I recommend that you go through the following courses on Codecademy

[Introduction to React.js: Part I](https://www.codecademy.com/learn/react-101)

![Introduction to React.js: Part I](https://i.imgur.com/jRhTSOU.png)

[Introduction to React.js: Part II](https://www.codecademy.com/learn/react-102)

![Introduction to React.js: Part II](https://i.imgur.com/jcryJs7.png)

In this project, we will use the following libraries and technologies: React.js, Babel, jQuery, Bootstap, Web Storage, Marked.js and Highlight.js

1) [React.js](https://facebook.github.io/react/) — JavaScript Library for Building User Interfaces

2) [Babel](https://babeljs.io/) — Babel is a JavaScript compiler. We will use it for Setting up React for ES6

3) [Marked.js](https://github.com/chjj/marked) — A markdown parser and compiler

4) [Highlight.js](https://highlightjs.org/) — Syntax highlighting for the Web

**5 Min Quickstart**

Get started by creating a new folder for your project, and name it anything you like. Then, inside that folder, create additional folders and files to match the following structure

{% highlight text %}
.
├── css
|    ├── style.css
├── js
|    ├── index.babel
|    └── storage.js
└── index.html
{% endhighlight %}

Start by creating a index.html file with the connected necessary libraries

{% highlight html %}
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>ReactJS Markdown Editor</title>
	<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.5.0/styles/default.min.css">
	<link rel="stylesheet" href='https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css'>
	<link rel="stylesheet" href="css/style.css">
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/react/15.1.0/react.js"></script>
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/react/15.1.0/react-dom.js"></script>
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.23/browser.min.js"></script>
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/marked/0.3.5/marked.js"></script>
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.5.0/highlight.min.js"></script>
	<script type="text/babel" src="js/index.babel"></script>
</head>
<body>
	<div id="app"></div>
</body>
</html>
{% endhighlight %}

Open the `js/index.babel` in your favorite code editor (for example [Sublime Text 3](https://www.sublimetext.com/3) or [Atom](https://atom.io/)) and create a new component that simply displays Hello World on page

{% highlight text %}
var MarkdownEditor = React.createClass({
	render: function(){
		return (
			<h1>Hello, world!</h1>
		)
	}
})
ReactDOM.render(
	<MarkdownEditor />,
	document.getElementById('app')
)
{% endhighlight %}

Now, run your website on a web server. If you see Hello World on page, it means that you have done everything correctly

> If you do not have installed [MAMP](https://www.mamp.info/en/) or [WAMP](http://www.wampserver.com/en/) server, for this project you can use Node.js [local-web-server](https://www.npmjs.com/package/local-web-server) package

**Now we can get started**

First we need to create a text box and markdown preview box where you can see the result of the work

To do this, replace the `render` function in your component with

{% highlight text %}
render: function() {
	return (
		<div className="container-fluid">
			<div className="row">
				<h1 className="text-center">
					ReactJS Markdown Editor
				</h1>
				<div className="col-xs-12 col-sm-6">
					<h3>Markdown</h3>
					<textarea id="markdown" className="markdown"></textarea>
				</div>
				<div className="col-xs-12 col-sm-6">
					<h3>Preview</h3>
					<div id="preview"></div>
				</div>
			</div>
		</div>
	)
}
{% endhighlight %}

and add the following styles to your `css/style.css` file

{% highlight css %}
.markdown {
	padding: 10px 5px;
	resize: vertical;
	overflow: auto;
	width: 100%;
	height: 564px;
	margin: 0 auto;
	display: block;
	margin-bottom: 15px;
}
{% endhighlight %}

Now when you run, your page will look like this

[![MARKDOWN_EDITOR](https://i.imgur.com/cHvqMZj.png)](https://i.imgur.com/cHvqMZj.png)

The next step we define a initial default value that the user will see in a textarea. To do this, add `getInitialState` function before `render`

{% highlight text %}
getInitialState: function() {
	return {
		content: '### Type Markdown Here'
	}
}
{% endhighlight %}

and replace 

{% highlight html %}
<textarea id="markdown" className="markdown"></textarea>
{% endhighlight %}

with 

{% highlight html %}
<textarea id="markdown" className="markdown" defaultValue={this.state.content}></textarea>
{% endhighlight %}

Now, every time you run the page, you'll see the following text in a textarea

{% highlight text %}
### Type Markdown Here
{% endhighlight %}

[![INIT_MARKDOWN](https://i.imgur.com/ftCb2CQ.png)](https://i.imgur.com/ftCb2CQ.png)

But you've probably noticed that in the `preview` block nothing. Let's fix it. Add `rawMarkup` function immediately after `getInitialState` that will parse the contents of the text box and output the result in `preview` box

{% highlight text %}
rawMarkup: function() {
	marked.setOptions({
		renderer: new marked.Renderer(),
		gfm: true,
		tables: true,
		breaks: false,
		pedantic: false,
		sanitize: true,
		smartLists: true,
		smartypants: false,
		highlight: function (code) {
			return hljs.highlightAuto(code).value
		}
	})

	var rawMarkup = marked(this.state.content, {sanitize: true})
	return {
		__html: rawMarkup
	}
}
{% endhighlight %}

and replace

{% highlight html %}
<div id="preview"></div>
{% endhighlight %}

with

{% highlight html %}
<div id="preview" dangerouslySetInnerHTML={this.rawMarkup()}></div>
{% endhighlight %}

In this code snippet, I use the standard recommended settings specified in the project repository Marked.js here [https://github.com/chjj/marked#usage](https://github.com/chjj/marked#usage)

{% highlight text %}
marked.setOptions({
	renderer: new marked.Renderer(),
	gfm: true,
	tables: true,
	breaks: false,
	pedantic: false,
	sanitize: true,
	smartLists: true,
	smartypants: false
})
{% endhighlight %}

and added support syntax highlighting using highlight.js library

{% highlight text %}
highlight: function(code) {
	return hljs.highlightAuto(code).value
}
{% endhighlight %}

and the following code snippet

{% highlight text %}
var rawMarkup = marked(this.state.content, {sanitize: true})
return {
	__html: rawMarkup
}
{% endhighlight %}

takes a `this.state.content` as input, in this case, the contents of the` textarea` as a result of the output we get the HTML that you will see in preview box

[![PREVIEW_MARKDOWN](https://i.imgur.com/ElI8XQd.png)](https://i.imgur.com/ElI8XQd.png)

If you try to change the contents of the `textarea` notice that your changes are not displayed in the preview box. To fix this, add the following `handleChange` function after `getInitialState`

{% highlight text %}
handleChange: function(event) {
	this.setState({
		content: event.target.value
	})
}
{% endhighlight %}

and replace

{% highlight html %}
<textarea className="markdown" defaultValue={this.state.content}></textarea>
{% endhighlight %}

with

{% highlight html %}
<textarea className="markdown" defaultValue={this.state.content} onChange={this.handleChange}></textarea>
{% endhighlight %}

Function `handleChange` is attached to the textarea `onChange` event. The `handleChange` function updates the components state with the new value of textarea. This causes the component to re-render with the new value

So when you change the markdown in the textarea field you will see changes in the preview

At this point, your code in `index.babel` file should look like this

`index.babel`

{% highlight text %}
var MarkdownEditor = React.createClass({
	getInitialState: function() {
		return {
			content: '### Type Markdown Here'
		}
	},
	handleChange: function(event) {
		this.setState({
			content: event.target.value
		})
	},
	rawMarkup: function() {
		marked.setOptions({
			renderer: new marked.Renderer(),
			gfm: true,
			tables: true,
			breaks: false,
			pedantic: false,
			sanitize: true,
			smartLists: true,
			smartypants: false,
			highlight: function (code) {
				return hljs.highlightAuto(code).value
			}
		})

		var rawMarkup = marked(this.state.content, {sanitize: true})
		return {
			__html: rawMarkup
		}
	},
	render: function() {
		return (
			<div className="container-fluid">
				<div className="row">
					<h1 className="text-center">
						ReactJS Markdown Editor
					</h1>
					<div className="col-xs-12 col-sm-6">
						<h3>Markdown</h3>
						<textarea className="markdown" defaultValue={this.state.content} onChange={this.handleChange}></textarea>
					</div>
					<div className="col-xs-12 col-sm-6">
						<h3>Preview</h3>
						<div dangerouslySetInnerHTML={this.rawMarkup()}></div>
					</div>
				</div>
			</div>
		)
	}
})

ReactDOM.render(
	<MarkdownEditor />,
	document.getElementById('app')
)
{% endhighlight %}

**Using Web Storage**

Suppose a user has written a large amount of text and reload the page by accident, reset computer or even any of the case for which the user did not have time or are not able to save the typed text. The user will probably be upset

What can we do in this case and how to prevent it?

We will keep all written user's markdown to Local Storage and after each restart page, restarted browser or rebooting the PC, the user will see the text written by him

To do this, add the `componentWillMount` function immediately after `rawMarkup`

{% highlight text %}
componentWillMount: function() {
	const script = document.createElement("script")

	script.src = "./js/storage.js"
	script.async = true

	document.body.appendChild(script)
}
{% endhighlight %}

This code snippet we use to load JavaScript, which is in the file `./js/storage.js` on our page within the script tag and execute it every time our component is rendered

> Note that the path to `storage.js` file must be specified relative to the file `index.html`

Now include the jQuery adding to render

{% highlight html %}
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
{% endhighlight %}

and add the following code in your `js/storage.js` file

{% highlight js %}
$(document).ready(function() {
	if (window.localStorage) {
		$('textarea').on('input', function(e) {
			var markdown = $('textarea').val()
			localStorage.setItem('markdownStorage', markdown)
		})
	}
})
{% endhighlight %}

or without jQuery

{% highlight js %}
var textarea = document.getElementById('markdown')
if (window.localStorage) {
	textarea.addEventListener('input', function() {
		localStorage.setItem('markdownStorage', textarea.value)
	})
}
{% endhighlight %}

In this code snippet, we check the user's browser supports Local Storage (you can check the browser compatibility using the service [Can I Use](http://caniuse.com/#search=web%20storage)) and if Local Storage is support we add an event listener to the textarea. Every time the input event is fired on them, it means something has changed and we save textarea contents with all changes to Local Storage with `markdownStorage` key

[![LOCAL_STORAGE](https://i.imgur.com/sxEkrzl.png)](https://i.imgur.com/sxEkrzl.png)

Finally, replace the initial default value

{% highlight text %}
getInitialState: function() {
	return {
		content: '### Type Markdown Here'
	}
}
{% endhighlight %}

with add text from Local Storage

{% highlight text %}
getInitialState: function() {
	return {
		content: localStorage.getItem('markdownStorage') || '### Type Markdown Here'
	}
}
{% endhighlight %}

Now, if the user has in Local Storage `markdownStorage` key, to the textarea will be inserted its contents. If `markdownStorage` key not found or empty then user will see a default value

{% highlight text %}
### Type Markdown Here
{% endhighlight %}

**Full Code**

`index.html`

{% highlight html %}
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>ReactJS Markdown Editor</title>
	<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.5.0/styles/default.min.css">
	<link rel="stylesheet" href='https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css'>
	<link rel="stylesheet" href="css/style.css">
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/react/15.1.0/react.js"></script>
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/react/15.1.0/react-dom.js"></script>
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.23/browser.min.js"></script>
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/marked/0.3.5/marked.js"></script>
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.5.0/highlight.min.js"></script>
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
	<script type="text/babel" src="js/index.babel"></script>
</head>
<body>
	<div id="app"></div>
</body>
</html>
{% endhighlight %}

`css/style.css`

{% highlight css %}
.markdown {
	padding: 10px 5px;
	resize: vertical;
	overflow: auto;
	width: 100%;
	height: 564px;
	margin: 0 auto;
	display: block;
	margin-bottom: 15px;
}
{% endhighlight %}

`js/index.babel`

{% highlight text %}
var MarkdownEditor = React.createClass({
	getInitialState: function() {
		return {
			content: localStorage.getItem('markdownStorage') || '### Type Markdown Here'
		}
	},
	handleChange: function(event) {
		this.setState({
			content: event.target.value
		})
	},
	rawMarkup: function() {
		marked.setOptions({
			renderer: new marked.Renderer(),
			gfm: true,
			tables: true,
			breaks: false,
			pedantic: false,
			sanitize: true,
			smartLists: true,
			smartypants: false,
			highlight: function (code) {
				return hljs.highlightAuto(code).value
			}
		})

		var rawMarkup = marked(this.state.content, {sanitize: true})
		return {
			__html: rawMarkup
		}
	},
	componentWillMount: function() {
		const script = document.createElement("script")

		script.src = "./js/storage.js"
		script.async = true

		document.body.appendChild(script)
	},
	render: function() {
		return (
			<div className="container-fluid">
				<div className="row">
					<h1 className="text-center">
						ReactJS Markdown Editor
					</h1>
					<div className="col-xs-12 col-sm-6">
						<h3>Markdown</h3>
						<textarea className="markdown" defaultValue={this.state.content} onChange={this.handleChange}></textarea>
					</div>
					<div className="col-xs-12 col-sm-6">
						<h3>Preview</h3>
						<div dangerouslySetInnerHTML={this.rawMarkup()}></div>
					</div>
				</div>
			</div>
		)
	}
})

ReactDOM.render(
	<MarkdownEditor />, 
	document.getElementById('app')
)
{% endhighlight %}

`js/storage.js`

{% highlight js %}
$(document).ready(function() {
	if (window.localStorage) {
		$('textarea').on('input', function(e) {
			var markdown = $('textarea').val()
			localStorage.setItem('markdownStorage', markdown)
		})
	}
})
{% endhighlight %}
