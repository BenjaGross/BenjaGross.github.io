---
layout: post
title: "About Using Jasmine"
date: 2015-12-19 13:24:25 -0500
comments: true
categories: testing, TDD, Gems
---

I have been working on a Backbone.js project for a few days now and while it has been great I came accross one very annoying issue that seemed worth writing about. On the third day after getting my environment set up and getting quick hello world out of the way I notice I hadn't run any of my tests yet. I opened up the console and after typing `rspec` out of habbit I realized I didn't know how to actually run Jasmine tests outside of Rails.

After searching the internet for the better part of an afternoon several solutions surfaced.
    
    * Use the [Yeoman Jasmine generator](https://github.com/yeoman/generator-jasmine) 
    * Use the [Jasmine Node Package](https://www.npmjs.com/package/jasmine)
    * Use the [Jasmine Standalone release](https://github.com/jasmine/jasmine/releases)

I opted for the standalone bundle since this is a learning project and I'm not the most experienced with Yeoman or Node. I download the bundle and was able to pull the necessary files into my project directory, set up an `index.html` in my spec directory but when I began to require all the relevant scripts I got a little fed up around the tenth. I talked to some friends and was told that I should just use the [Jasmine Gem](https://github.com/jasmine/jasmine-gem). Since I had only used it with Rails I didn't realize that the command `jasmine init` would add all the necessary files to a non-Rails non-ruby project. Once you add jasmine to your projec you can call `jasmine examples` and the gem will generate some example tests that you can just replace with your own. You can then run the tests in browser with `rake jasmine` which runs on a python server.

Now you are probably how you can customize jasmine, make your own test files, and require the proper dependencies for yourt ests. The Jasmine gem makes it pretty easy by giving you `jasmine.yml` a file full of comments telling you where you can require your specs, your actual program and whatever else you need.  