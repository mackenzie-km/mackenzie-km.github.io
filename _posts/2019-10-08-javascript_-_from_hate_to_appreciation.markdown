---
layout: post
title:      "JavaScript - From Hate to Appreciation"
date:       2019-10-08 16:09:49 +0000
permalink:  javascript_-_from_hate_to_appreciation
---


*NOTE: The following blog post is for the V7 JS project. I finished the project the day they rolled out V8, so I am retroactively submitting V7.*

A few weeks ago, I told my friend the following:

> Rails is beautiful. It's like reading a college literary thesis, full of elegance and mastery. Javascript... it reminds me of what my writing was like back in high school. It's alright - getting there. But not quite the same. At least it's better than php... that looks like a middle schooler who still doesn't know what they're doing.

This article won't cover PHP (that's a whole different story!). And I still think that Ruby is much more elegant than Javascript..

## BUT here's why I've come to love Javascript:
1. Less pages! In this project, I was able to cut out many additional pages. If a user wanted to add a contact to an event, they didn't have to view multiple pages. It was added or deleted upon one click. No more scrolling through hundreds of checkboxes, hitting save, and redirecting to a new page. It could all be sent and displayed through JS. 

![Image](https://imagizer.imageshack.com/img924/5663/DBTnAR.png)

2. Easier to display info! In this project, users add contacts to events (see above). In the Rails-only version, it was impossible to double check which Sally was being added without viewing a separate show page. Now, duplicate name errors are reduced. With a handy JS info button, the user receives information about the desired contact, such as their email and their last event attended. 


3. Easier to search! In this project, I was able to implement a contacts search page that allows filtering without a refresh. This prevents the page from loading slowly, and improves the user's experience.

![Image](https://imagizer.imageshack.com/img922/3097/A3XBXv.png)


## Some JS lessons learned:
* When working with JS & Rails, **be careful about CSRF errors**. In one part of my code, I had two forms on the same page, and this caused problems with AJAX requests. I found that using `let authenticity_token = $('meta[name="csrf-token"]').attr('content')` to grab my first form's authenticity token and `data.forEach(function(item, i) { if (item.name == "authenticity_token") item.value = authenticity_token; });` to replace the value and `  beforeSend: function(request) {
    request.setRequestHeader("authenticity_token", data[1])` to send it in the AJAX request all helped me to avoid CSRF errors with AJAX and multiple forms on the same page.
* Use `event.preventDefault() `along with the notation `on("click", function(event) `often to intercept clicks when your listener is activated. When I was first going through the material, I didn't realize that I hadn't disabled the default behavior of my buttons/links. This was causing the page to still reload even though I thought it was an AJAX request.
* Use `debugger `- a lot. You just have to type in debugger and have your browser console open to be able to play around with the variables. I used this a lot with Rails' pry to test my code line for line. You just have to type in `debugger` into whatever line of JS code 
* Adding JS files to your rails project: add the file to your app/assets/javascript file, add a javascript_includes_tag to the page, and add `Rails.application.config.assets.precompile += %w( file.js )` to your config/initializers/assets.rb file. Make sure jQuery is also added by including the tag on the view and adding ```
//= require jquery
//= require jquery_ujs
``` to your application.js file
* Browser compatibility - my friend complained to me that datalist doesn't work in old versions of Safari. But this taught me a lesson - if someone complains about a bug, **poke deeper**. See what software they're using, and how old it is. It ends up she had a very old version that wasn't following HTML 5 standards, and she was having a lot of other issues with other websites. It taught me not to doubt myself, and spend hours rethinking a particular feature, but to investigate and educate. 
