---
layout: post
title:      "Rails: SessionsController & has_secure_password"
date:       2019-06-22 18:51:18 -0400
permalink:  rails_sessionscontroller_and_has_secure_password
---

I was really disappointed with the has_secure_password section because I didn’t feel prepared for all the concepts introduced in the lab. So here’s a blog post about some of the steps I took to navigate these concepts.

**What the heck? SessionsController? How does this interact with UsersController?**

1. Get some more background knowledge. I checked out [this wordy](https://www.railstutorial.org/book/basic_login) (but helpful) guide in seciton 8.1.1 to learn more about what SessionsController was intended for. I also read around some other sections on the page as a refresher on sessions and routes.

2. I learned that SessionsController really should be the controller that manages `sessions#new` (login page), `sessions#create `(setting your session_id), and `sessions#destroy` (the logout feature)

3. What does this leave UsersController for, then? `users#new `should be a sign-up page. `users#create` should create a new user if the passwords match. 

**Preparing to use has_secure_password**

1. Add the` bcrypt` gem to your Gemfile

2. Run `bundle install `& update

3. Create your users table (remember NOT to make a password column - this isn’t secure. Make a `password_digest `column so that bcrypt can save only the salted and hashed password digest)

4. Run your migrations 

5. Create your user model & play around with it in console to make sure it’s working as expected

6. Add `has_secure_password `to your user model 

Woop woop - now you have access to methods like `password & password_confirmation `to verify your password!! You also have access to things like authenticate to make sure that the user’s submitted password matches the expected password.

**How do I make new users? (This should be a lot of review!)**

Set up my UsersController
  ```
  class UsersController < ApplicationController
    def new 
    end 
    def create 
    end 
    end 
```

2. Set up my user routes for submitting and creating a user in the database 
* get '/users/new', to: "users#new"
* post '/users', to: "users#create"

3. I made the user/new view with a nice create a user form 
   ```
 <%= form_for :user, url: ‘/users' do |f| %>
    Username: <%= f.text_field :name %>
    Password: <%= f.password_field :password %>
    Password Confirmation: <%= f.password_field :password_confirmation %>
    <%= f.submit "Submit" %>
    <% end %>
```

4. Now, my tests can go to create a new user. The data is getting pushed into the UsersController `users#create` action. So it was time to set up my create action. I wanted to make sure I saved a user to the database, and that it redirected me to try again if it didn’t work. If it did work, I wanted the user to be logged in.

    ```
@user = User.new(user_params)
    if !@user.save
    redirect_to '/users/new'
    else
    session[:user_id] = @user.id
    redirect_to "/"
    end
```

	NOTE: I also added strong params to the bottom of this section: `params.require(:user).permit(:name, :password, :password_confirmation)`…. isn’t that cool how the password_confirmation is a nice given method with you when you do has_secure_password 🙂

I’m in!! Yes!!! But next time I visit this page… I’m gonna need to login & logout. I’m going to need a sessions controller.

**How do I set up my SessionsController for submitting, creating, and destroying a session in the browser?**

1. Set up your sessions controller routes.
```
class SessionsController < ApplicationController
def new 
end 
def create 
end 
def destroy
end 
end 
```

2. I used that guide to set up my routes 
```
get    '/login',   to: 'sessions#new'
post   '/login',   to: 'sessions#create'
delete '/logout',  to: 'sessions#destroy'
```

3. I made the `session/new view` & I copied the same create user form into the `session/new view `
```
<%= form_for :user, url: ‘/login' do |f| %>
Username: <%= f.text_field :name %>
Password: <%= f.password_field :password %>
Password Confirmation: <%= f.password_field :password_confirmation %>
<%= f.submit "Submit" %>
<% end %>
```

4. Now, when the tests ran, there was a `sessions#new form `to view. That form would post to `sessions#create`. Sessions create now could use the data. So I set up the create action. (I have to admit… I had to look at the solution at this part. I was starting to lose my mind. I’m going to leave something out that’s too solution-specific.)

5. Then you would to check and see if the user was authenticated or not. 
* The first step is to find the user so you can later compare credentials - use a `User.find_by`
* Then you need to actually try to authenticate them ([a fun note on try!!](https://apidock.com/rails/v3.2.1/Object/try) - `user = user.try(:authenticate, params[:user][:password])`

6. You will need to redirect_to the `users#new `action (to make a new user) or the login page (try again) if not. 
* If they were authenticated, try to set` session[:user_id]` to the user’s id. Redirect them to your homepage or wherever!

7. To let the user logout, go into `sessions#destroy` to use the .delete method on the `session[:user_id],` and redirect them to your homepage 


Fixing this implementation of my SessionsController made all of my tests turn green. Cross checking the solution, this seems to be mostly accurate. This may not be 100% accurate, but doing all of this really helped me to start building a conceptual foundation!

