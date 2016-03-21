---
layout: post
title: Work with Gulp. Automate Your Routine Tasks Easily with Gulp.js
categories: [Gulp, JavaScript, CSS, Tutorial]
tags: [Gulp, JavaScript, CSS, Tutorial]
fullview: false
comments: true
description: Work with Gulp. Automate and enhance your workflow. Automate Your Routine Tasks Easily with Gulp.js. In this post I'll talk about a few Gulp plugins. By the end of this post you will be able to use a gulp and gulp plugins. Learn how to configure a gulp with gulpfile.js and do your job easier and more enjoyable because you no longer need to manually perform routine tasks. Automate it!
---

Work with Gulp. Automate and enhance your workflow. Automate Your Routine Tasks Easily with Gulp.js. 
In this post I'll talk about a few Gulp plugins. By the end of this post you will be able to use a gulp and gulp plugins. 
Learn how to configure a gulp with gulpfile.js and do your job easier and more enjoyable 
because you no longer need to manually perform routine tasks. Automate it!

#### What is Gulp

Gulp it's a build system or task runner like Grunt, and so on, except much much better. A few key points:

- Based on Node.js streams.
- Aims to avoid writing to disk unnecessarily for performance.
- The API is really simple. There are four methods.
- Code is favoured over configuration.
- It's really intuitive. You take some files, you do this, that, and that other thing to them, and then store them somewhere.
- Plugins are really simple and easy to use.
- Takes advantage of existing modules on npm (which has now reached over 200,000 modules by the way), but more about that later.
- Automation - gulp is a toolkit that helps you automate painful or time-consuming tasks in your development workflow.
- Platform-agnostic - Integrations are built into all major IDEs and people are using gulp with PHP, .NET, Node.js, Java, and other platforms.
- Strong Ecosystem - Use npm modules to do anything you want + over 2000 curated plugins for streaming file transformations
- Simple - By providing only a minimal API surface, gulp is easy to learn and simple to use

#### Get Gulp

For installation and further work with Gulp, you will need NPM package manager. NPM is a part NodeJS.
Download and install NodeJS from the official website [NodeJS](http://nodejs.org/). I'm using version [NodeJS 4.4.0 LTS x64](https://nodejs.org/dist/v4.4.0/node-v4.4.0-x64.msi) (Latest at the time of writing).

After installation is complete, open a command prompt (or terminal) and upgrade to the latest version of the NPM:

{% highlight sh %}
$ npm install -g npm@3.8.2
{% endhighlight %}

Then install Gulp globally:

{% highlight sh %}
$ npm install -g gulp
{% endhighlight %}

Create a project folder or [download from my DropBox](https://www.dropbox.com/s/f2sln8wt2r1arys/Gulp-Tutorial.rar?dl=0)

Open the project folder and create package.json file

{% highlight json %}
{
	"name": "project",
	"version": "1.0.0",
	"devDependencies": {
		
	}
}
{% endhighlight %}

All Project and Gulp dependencies will be written in this file.

Now open this folder in terminal and install Gulp to your project localy, with `--save-dev` key:

{% highlight sh %}
$ npm install --save-dev gulp
{% endhighlight %}

and create a basic `gulpfile.js` in the root of project:

{% highlight js %}
var gulp = require('gulp');

gulp.task('default', function() {

});
{% endhighlight %}

Start Gulp

{% highlight sh %}
$ gulp
{% endhighlight %}

#### Start working with Gulp plugins

In this post I'll talk about a few Gulp plugins

Concatenate files with [gulp-concat](https://www.npmjs.com/package/gulp-concat/)

This plugin is for concatenates multiple files into one.
Install this plugin to your project:

{% highlight sh %}
$ npm install --save-dev gulp-concat
{% endhighlight %}

and add this task to your `gulpfile.js`

{% highlight js %}
var concat = require('gulp-concat');

gulp.task('css', function() {
	return gulp.src('css/*.css')
		.pipe(concat('concat.css'))
		.pipe(gulp.dest('css/dist'));
});

gulp.task('default', ['css']);
{% endhighlight %}

After starting Gulp command, plugin `gulp-concat` will take all the CSS files located in the `css/` directory 
and concatenate them into a one file `css/dist/concat.css`. Path and file names, you can change as you wish.

Now your `gulpfile.js` should be look like this

{% highlight js %}
var gulp = require('gulp');
var concat = require('gulp-concat');

gulp.task('default', function() {

});

gulp.task('css', function() {
	return gulp.src('css/*.css')
		.pipe(concat('concat.css'))
		.pipe(gulp.dest('css/dist'));
});

gulp.task('default', ['css']);
{% endhighlight %}

> Don't delete task default. Never!

Minify CSS files with [gulp-clean-css](https://www.npmjs.com/package/gulp-clean-css) 
(new version of deprecation [gulp-minify-css](https://www.npmjs.com/package/gulp-minify-css))
and [gulp-rename](https://www.npmjs.com/package/gulp-rename)

After concatenate is often a desire to minification CSS file for reduce its size. For minification we will use the plugin [gulp-clean-css](https://www.npmjs.com/package/gulp-clean-css).
Plugin [gulp-rename](https://www.npmjs.com/package/gulp-rename) we will use to rename a file because we want to make our file after minification called as `concat.min.css`.

Install this plugins

{% highlight sh %}
$ npm install --save-dev gulp-clean-css
$ npm install --save-dev gulp-rename
{% endhighlight %}

define this plugins to your `gulpfile.js`

{% highlight js %}
var cleanCSS = require('gulp-clean-css');
var rename = require("gulp-rename");
{% endhighlight %}

and change your `css` task so that it looked as

{% highlight js %}
gulp.task('css', function() {
	return gulp.src('css/*.css')
		.pipe(concat('concat.css'))
		.pipe(gulp.dest('css/dist'))
		.pipe(cleanCSS())
		.pipe(rename("concat.min.css"))
		.pipe(gulp.dest('css/dist'));
});
{% endhighlight %}

There `.pipe(cleanCSS())` executes minification of our CSS file and `.pipe(rename("concat.min.css"))` renames it to `concat.min.css`

Now after run Gulp in `css/dist/` directory you will can see your minificated CSS file `concat.min.css`.

At the moment, to again perform `css` task after making changes to your CSS files you need to run Gulp manually.
But we can solve this problem by using the built-in Gulp function `watch`.

Add the following code at the end of `gulpfile.js` before `gulp.task('default', ['css']);`

{% highlight js %}
gulp.task('watch', function() {
	gulp.watch('css/*.css', ['css'])
});
{% endhighlight %}

and replace `gulp.task('default', ['css']);` with `gulp.task('default', ['css', 'watch']);`

Now every time after you save changes in the CSS files, Gulp will run `css` task automatically.

Add task for support SASS with [gulp-sass](https://www.npmjs.com/package/gulp-sass). To do this, install `gulp-sass` plugin:

{% highlight sh %}
$ npm install --save-dev gulp-sass
{% endhighlight %}

add sass task before css task

{% highlight js %}
var sass = require('gulp-sass');

gulp.task('sass', function () {
	return gulp.src('sass/*.scss')
		.pipe(sass())
		.pipe(gulp.dest('css/'));
});
{% endhighlight %}

replace `gulp.task('default', ['css', 'watch']);` with `gulp.task('default', ['sass', 'css', 'watch']);`

and replace

{% highlight js %}
gulp.task('watch', function() {
	gulp.watch('css/*.css', ['css'])
});
{% endhighlight %}

with

{% highlight js %}
gulp.task('watch', function() {
	gulp.watch('sass/*.scss', ['sass'])
	gulp.watch('css/*.css', ['css'])
});
{% endhighlight %}

Now all SASS `*.scss` files from `sass` folder will be compiled to `*.css` files, save it to `css` folder 
and then will start a task `css` for concat and minify. Task `watch` will also keep track for all changes.

The last thing I would like to show about Gulp work with CSS is a plugin [gulp-autoprefixer](https://www.npmjs.com/package/gulp-autoprefixer)

Autoprefixer is a post-processing step meaning it actually updates already compiled stylesheets to 
add relevant prefixes based on an up-to-date database and a given configuration.
In other words, you tell Autoprefixer which browsers you want to support, 
and it adds only relevant prefixes to the stylesheets.

Install it

{% highlight js %}
npm install --save-dev gulp-autoprefixer
{% endhighlight %}

define it

{% highlight js %}
var autoprefixer = require('gulp-autoprefixer');
{% endhighlight %}

add it for our `css task` before `.pipe(concat('concat.css'))`

{% highlight js %}
.pipe(autoprefixer({browsers: ['last 2 version', 'safari 5', 'ie 9', 'opera 12.1', 'ios 6', 'android 4']}))
{% endhighlight %}

Now your `gulpfile.js` should look something like this

{% highlight js %}
var gulp = require('gulp');
var concat = require('gulp-concat');
var cleanCSS = require('gulp-clean-css');
var rename = require("gulp-rename");
var sass = require('gulp-sass');
var autoprefixer = require('gulp-autoprefixer');

gulp.task('default', function() {

});

gulp.task('sass', function () {
	return gulp.src('sass/*.scss')
		.pipe(sass())
		.pipe(gulp.dest('css/'));
});

gulp.task('css', function() {
	return gulp.src('css/*.css')
		.pipe(autoprefixer({browsers: ['last 2 version', 'safari 5', 'ie 9', 'opera 12.1', 'ios 6', 'android 4']}))
		.pipe(concat('concat.css'))
		.pipe(gulp.dest('css/dist'))
		.pipe(cleanCSS())
		.pipe(rename("concat.min.css"))
		.pipe(gulp.dest('css/dist'));
});

gulp.task('watch', function() {
	gulp.watch('sass/*.scss', ['sass'])
	gulp.watch('css/*.css', ['css'])
});

gulp.task('default', ['sass', 'css', 'watch']);
{% endhighlight %}

In this post I described only about how to work Gulp with CSS.
But knowledge that you have get from this post should be enough for you to do be able 
to continue to write your gulpfile.js by himself, 
for example, to work with JavaScript.
Or for any other purposes.

#### Good luck!
