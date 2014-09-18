---
layout: post
title: "Node.js Quickstart Guide, Now With 50% More Pro Tips"
date: 2014-09-18 01:36:19 -0400
comments: true
categories: node.js, express, framework, apps, nodemon, underscore
---


Those of us who just graduated from the Flatiron School have plenty of experience setting up Rails applications. Maybe we'll choose Sinatra if we're feeling daring and minimal. But the internet is a big place, and there are tons of other frameworks to explore. One of the most popular (and trendy) choices today is Node, running on the Express framework.

To be fair, I built [my final project](www.tweetworld.me) for the Flatiron School in Node. But I concentrated on the database work and general functionality, allowing one of my teammates to set up the server and routes. I decided to review Node by setting up a [simple "Hello World" application](https://github.com/ilanasufrin/sampleNodeApp) with help from Aaron from Fog Creek/Trello. [Trello](https://trello.com/) actually runs on Node and Express so Aaron had some great tips for me!

The [Node/Express](http://expressjs.com/guide.html) documentation is very good. Once you install both technologies and make a directory for your app, you just have to make the two required files: `package.json` and `app.js`. The former keeps track of all your dependencies and behaves virtually identically to a Gemfile in Ruby. The latter sets up routes and actually starts the app. In any real-world scenario, you'd also need files for views, app logic, styling, etc. But for the purposes of our Hello World app, you can get away with the two files.

Here's what your `package.json` should look like at first:

![image](https://31.media.tumblr.com/b3e681c78cd3b664d92f58c284b063d0/tumblr_inline_nc2crfM68j1si9gc8.png)

Now you can just type:

 `npm install` 

on the command line to install the one listed dependency.

_Pro tip:
 If you want to add dependencies (like templating engines or anything else) in the future but don't feel like manually adding it to `package.json`, just install it on the command line with the `--save` option. For example, `npm install underscore --save`._

Now we have to actually set up the server. The documentation for what goes in the `app.js` file is quite clear:

![image](https://31.media.tumblr.com/f23f2865e691a0fdb5f15b1c70bb5469/tumblr_inline_nc2d3jLV2w1si9gc8.png)

If you've followed these steps exactly you should have about 8 lines of code in `app.js`. Now you should just be able to type:

`node app.js`

to start the server. Go to _localhost:3000/hello.txt_ in your browser and you should see "Hello World"!

You can define any route you'd like here by changing the arguments passed to `app.get()`. (You can also define POST endpoints with `app.post()`.) The sample code sends a string response but you could easily pass in a view here as well.

_Pro tip:_
_You can define a middleware function here that will get called every time the server receives a request. For example, checking the request for certain media types to redirect users if they visit your site on an iPad. [Just pass a function to `app.use()`](http://expressjs.com/api#app.use)._

Congratulations! You've officially set up a basic Node app that knows how to handle routes and display text passed in as strings! The rest of this guide will focus on setting up apps with more complicated functionality. Remember that every time you make a change to your code, you must restart the server on the command line if you want to see the changes reflected in your browser.

**OPTIONAL: Getting Started with a Templating Library ([Consolidate.js](https://github.com/visionmedia/consolidate.js))
**

_Pro tip: 
At this point you might want to make a `.gitignore` file and add `node_modules` to it. Everybody developing on the codebase needs to `npm install` dependencies on their machines anyway, so there's no reason to check in these potentially huge files._

If you're using this guide to set up an app with multiple views, you might want to consider using a templating library. [Consolidate.js](https://github.com/visionmedia/consolidate.js) is a great tool that plugs into Express and allows you to select almost any templating utility. I arbitrarily chose to use Consolidate to set up [Underscore templating](http://documentcloud.github.io/underscore/), so my code samples will use that syntax. 
Make sure to:

 `npm install consolidate --save`

if you're following along. You also have to require consolidate at the top of `app.js`:

`var cons = require('consolidate');`

Once this is done, here's some sample code to get started using underscore with consolidate:

![image](https://31.media.tumblr.com/abcb1d2cb5732eb22a1eef8c041e7f3b/tumblr_inline_nc2y90Z04f1si9gc8.png)

The templating library is now expecting all of your views to live in a `views` directory, so make this folder.

The sample route we've defined is the index, so now create an `index.html` in your views directory. To make sure you've set up underscore templating correctly, let's evaluate the variable it gave us. Type this in your `index.html`:

`<%- title %> ` 

Now if you start the server with `node app.js`, you should be able to go to _localhost:3000_ and see this:

![image](https://31.media.tumblr.com/1a1b433ec4301aa8d8b4ee2d58edd210/tumblr_inline_nc2yktWzrW1si9gc8.png)

Hooray! Underscore has interpolated the title variable! Of course usually you'd pass in a model here instead of cluttering up your `app.js` file with random variables.

**OPTIONAL: Getting Started with [Nodemon](http://nodemon.io/)**

At this point you're probably tired of restarting your server EVERY time you make a change in your code. Luckily, there's a better way. [Nodemon](http://nodemon.io/) will watch for changes in your source code and and automatically restart the server for you, enabling lazy/efficient developers everywhere. It's similar to Shotgun in Ruby projects. Just run:

`npm install -g nodemon`

to install it globally. This means you can use it on future projects as well. Now you can just run:

`nodemon app.js` 

once instead of 

`node app.js` 

many times. Note that you'll still have to refresh your browser to see changes, but your server will handle reloading for you. You won't see nodemon listed as a dependency for this project since you've installed it globally.

_Pro tip:_
_In case you've lost track of our file structure, here's a screenshot of what it should look like at this point:_

![image](https://31.media.tumblr.com/d6716e83c02caac3b9505dce47f8beac/tumblr_inline_nc2zp822qq1si9gc8.png)

And there you have it, folks! You're now well on your way to setting up an interesting, complicated Node.js project using the Express framework. The documentation is pretty good but the error messages are AWFUL (think something like "generalized error"!) so if you have any problems, feel free to message me at ilana.sufrin@flatironschool.com.