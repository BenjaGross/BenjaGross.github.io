---
layout: post
title: "About Using Jasmine"
date: 2015-12-19 13:24:25 -0500
comments: true
categories: testing, TDD, Gems
---

I have been working on a Backbone.js project for a few days now and while it has been great I came accross one very annoying issue. After getting my environment set up and a quick hello world out of the way I started to write a few Jasmine tests. I opened up the console and stared blankly for a few minutes and realized I didn't know how to run them. I had the proper source files and dependencies but just didn't know how to actually run my tests. 

After looking on google for a while I decided to refactor to use the [Jasmine Standalone release](https://github.com/jasmine/jasmine/releases) which I probably should have done to begin with but I guess I had on my Ruby colored glasses. 

I download the bundle and was able to pull the necessary files into my project directory, set up an `index.html` in my spec directory but when I began to require all the relevant scripts I decided I wanted a better way. I talked to some friends and was told that I should just use the [Jasmine Gem](https://github.com/jasmine/jasmine-gem). Since I had only used the gem with with Rails and I didn't think to use a gem for a strictly JavaScript project didn't even bother looking at the gem (which I already had). 

Using the Jasmine gem is extremely simple and you can get started with just two commands: 

```bash
jasmine init
jasmine examples
```


 Once you add jasmine to your projec you can call `jasmine examples` and the gem will generate some example tests that you can just replace with your own. You can then run the tests in browser with `rake jasmine` which runs on a python server.

Now you are probably how you can customize jasmine, make your own test files, and require the proper dependencies for yourt ests. The Jasmine gem makes it pretty easy by giving you `jasmine.yml` a file full of comments telling you where you can require your specs, your actual program and whatever else you need. 

In `jasmine.yml` you can use wildecards to require directories like `js/**/*.js` however this will load all your dependencies in alphabetical order so it will still be necessary to require certain files in order manually for example, `underscore.js` will need to be required before `backbone.js`.  