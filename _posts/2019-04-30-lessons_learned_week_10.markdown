---
layout: post
title:      "Lessons Learned: Week 10"
date:       2019-04-30 16:27:24 +0000
permalink:  lessons_learned_week_10
---

This past week, my most frequent errors were all from my sinatra routes. 

Long story short - I thought I was being RESTful, and I wasn't. 

Sometimes I would confuse the new and create action. 
Sometimes I got mixed up between my new page and my edit action.
Sometimes I wouldn't properly set the HTML forms to patch or delete. 

The most helpful thing for me to do was to extract information from the Learn.co lessons, and make my own cheat sheet.
This allowed me to be more familiar with the appropriate code and purpose of each action.

Below is my cheat sheet:

***

**HTTP VERB    ROUTE    Action    Used For
GET    '/articles'    index action   ** index page to display all articles
get '/articles' do
  @articles = Article.all
  erb :index
end

**GET        '/articles/new'       **   new action      displays create article form
get '/articles/new' do
  erb :new
end

**POST    '/articles'      create action  **  creates one article
post '/articles' do
  @article = Article.create(:title => params[:title], :content => params[:content])
  redirect to "/articles/#{@article.id}"
end

**GET        '/articles/:id'    show action    **  displays one article based on ID in the url
get '/articles/:id' do
  @article = Article.find_by_id(params[:id])
  erb :show
end

**GET    '/articles/:id/edit'    edit action   ** displays edit form based on ID in the url
get '/articles/:id/edit' do  #load edit form
    @article = Article.find_by_id(params[:id])
    erb :edit
  end

note for patch or put: make sure to make it so that the params are specific that it is saving. if you just do params,
it will spaz out because there is no _method variable
**PATCH    '/articles/:id'    update action **   modifies an existing article based on ID in the url. like repainting.
patch '/articles/:id' do #edit action
  @article = Article.find_by_id(params[:id])
  @article.title = params[:title]
  @article.content = params[:content]
  @article.save
  redirect to "/articles/#{@article.id}"
end
**<input id="hidden" type="hidden" name="_method" value="patch">**
make sure your form has this & use Rack::MethodOverride in application controller above all controllers

**PUT    '/articles/:id'    update action**    replaces an existing article based on ID in the url. completely starts with a new article.

**DELETE    '/articles/:id'    delete action **   deletes one article based on ID in the url
delete '/articles/:id/delete' do #delete action
  @article = Article.find_by_id(params[:id])
  @article.delete
  redirect to '/articles'
end

remember the hidden input field
**<input id="hidden" type="hidden" name="_method" value="delete">
  <input type="submit" value="delete">**
