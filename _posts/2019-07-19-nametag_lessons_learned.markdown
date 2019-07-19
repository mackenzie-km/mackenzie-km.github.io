---
layout: post
title:      "Nametag: Lessons Learned"
date:       2019-07-19 23:40:45 +0000
permalink:  nametag_lessons_learned
---


## Nametag: 95% Complete

My [rails project]https://github.com/mackenzie-km/nametag) has taken me a whole week longer than I expected - mostly due to the many requirements & the fact I started part-time work. 

The good news: the end is in sight! My app is almost good to go (just some refactoring left this weekend). And it's something I would actually use for my international organization!

I wanted to document a few resources & lesson learned that really helped during this process.

## Lessons Learned

* For a big project like this, you need to organize your tasks. I wasn't very organized, and I kept getting distracted by different bugs I found. Something more reminiscient of a mini-sprint process or a coding to-do list would have been much better than wandering around my code aimlessly.
* For any project with user or contact data, security is more challenging than expected. How can I make sure that users don't edit people they don't have access to? This was a big theme in my project for me. 
* Front-end is hard. I tried to use Bootstrap in my project, namely because my part-time job has some people who use it, and because I wanted to get my feet wet. WOW - hard! Bootstrap is a whole different beast. In addition to that, I spent many hours aligning stuff, overriding code, and obsessing over exact hex values. Not my cup of tea. When all is said and done, however, it makes for a good end project. It was all worth it, I suppose.  

## Helpful Resources 

### Q: How can I use Google instead of Facebook for OmniAuth?
[Google OAuth for Ruby on Rails](https://medium.com/@amoschoo/google-oauth-for-ruby-on-rails-129ce7196f35) - A helpful tutorial that translates the lessons about Facebook OmniAuth into a Google context

### Q: I tried to add some fancy bootstrap CSS, & it's not working!
[Stack Overflow](https://stackoverflow.com/questions/47329694/bootstrap-4-navbar-collapse-not-working) - This article resolved hours of rage.

### Q: My controllers are too fat! How can I clean up some code?
Before_actions saved the day for me - 
before_action :redirect_if_no_login
before_action :access_contact, only: [:show, :edit, :update, :destroy]
I made helper methods in the application controller to help redirect and determine if the access levels are appropriate. All it took were these two lines of code, and some methods in my application controller.
