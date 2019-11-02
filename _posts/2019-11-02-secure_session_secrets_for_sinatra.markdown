---
layout: post
title:      "Secure Session Secrets for Sinatra"
date:       2019-11-02 17:14:14 +0000
permalink:  secure_session_secrets_for_sinatra
---

This blog post is about three months late... but while I had a free Saturday morning, I figured it was time to polish up on my old Sinatra app!



## The problem 

From the Sinatra "Setting up a Sesson" lesson -

```
configure do
  enable :sessions
  set :session_secret, "secret"
end
```

And it comes with this following quote:
> IMPORTANT: You should never set your session_secret by hand, and especially not to something so trivially simple as "secret"! We are doing this for the sake of demonstration this one time. You are advised to learn more about how to secure your sessions by following the [Using Sessions][secsin] documentation at the Sinatra home.
> 

However, I had a hard time following the documentation on the link they provided. 

Also,[ this article](https://martinfowler.com/articles/session-secret.html) spooked me out.

This left me wondering... then how do I make it secure for my users?!

![http://giphygifs.s3.amazonaws.com/media/rAm0u2k17rM3e/giphy.gif](http://giphygifs.s3.amazonaws.com/media/rAm0u2k17rM3e/giphy.gif)



## The solution

[Dot-env.](https://github.com/bkeepers/dotenv)

It's the same solution as for my Rails app later. Since Sinatra is rack-based, it works just as well!

### Generating a secret code 


### Using DotENV

1. Use gem install dotenv to get it installed locally
2. Add `gem 'dotenv'` to your Gemfile (I put it high up in my Gemfile as to not cause environment loading conflicts - right after sinatra)
3.  Install your bundle (& update for good measure)
4.  Require the file to be loaded in your config/environment file as seen below (important line is the dotenv/load line)
> require 'bundler/setup'
Bundler.require(:default, ENV['SINATRA_ENV'])
require 'dotenv/load'
require_all 'app'
5.  In the root of your project, create a file called .env (this is best done from the command line - for MacOs users, type `touch .env `when you are in your root directory)
6.  Use `open .env` to open it up. 
7.  Here, you can add some secret code! I set `SESSION_SECRET=stringofrandomlygeneratedcodegoeshere` (for example, generated with SecureRandom.hex(64) in an irb or rails console session - make sure it's at least 64 bytes to not compromise security - see [Choosing Key Length](https://github.com/sinatra/sinatra/issues/1187))
8.  Now, my Application Controller has a configure_do block that looks like this:
```
  configure do
    set :root, File.dirname(__FILE__)
    set :public_folder, 'public'
    set :views, 'app/views'
    enable :sessions
    use Rack::Flash
    set :session_secret, ENV['SESSION_SECRET']
  end
```

Perfect - it's unguessable, and it's not showing up on my github!

## In production?
Depends on how you're hosting it. In heroku, you can set environmental variables.

Hope this helps. 

![https://media.giphy.com/media/KEYEpIngcmXlHetDqz/giphy.gif](https://media.giphy.com/media/KEYEpIngcmXlHetDqz/giphy.gif)
