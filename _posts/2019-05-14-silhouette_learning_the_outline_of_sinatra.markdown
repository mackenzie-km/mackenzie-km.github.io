---
layout: post
title:      "Silhouette: Learning the Outline of Sinatra"
date:       2019-05-14 21:28:18 +0000
permalink:  silhouette_learning_the_outline_of_sinatra
---


Today I reached a big milestone - completion of my Sinatra project (Silhouette)!

Of course - there are more features I want to add someday. But, for now, it's ready for feedback.

Looking back, I want to remember lessons learned about the following three topics:


## PLANNING
My most critical tools at the beginning? A blank .txt file, and a white sheet of paper.

On the white sheet of paper, I drew out a process map of how I wanted the models to interact, what columns they would need, and how the models would interact with each other. 

On the blank .txt file, I described the purpose, functionality, goals, app color scheme, and class attributes I desired. 

Because the information was still fresh to me, and I had other projects to model from, it was easy for me to set up the required files. But <a href="https://flatironschool.com/blog/how-to-build-a-sinatra-web-app-in-10-steps/">this website</a> was super helpful in double checking my work. 


## MACROS
ActiveRecord, Sinatra, and Rake are my friends. After spending so much time struggling to create my own methods in previous labs, I was amazed at how much I could do with so little code. 

Halfway through this project, I decided that I wanted to add new functionality: a facts class that could take in a string of data, create new fact instances for me, and assign it/display it with my contact. I was blown away by how quickly this could be implemented with ActiveRecord and Sinatra. And now my app is versatile enough that I would actually use it.

I definitely want to remember <a href="https://guides.rubyonrails.org/association_basics.html">ActiveRecord associations</a>, bcrypt's has_secure_password macro,  Class.where("sql statement").first_or_create, and ActiveRecord's valid? method. These were so critical in validating my CRUD functions concisely and securely.

## GITHUB COMMITS
Ouch - this one hurt towards the end. I would get too excited with troubleshooting, and forget to commit. This is a lesson learned for next time. 

It's improving, but I need to learn how to use branches and commits to better troubleshoot while protecting my master code. This is critical as I move forward into more complicated applications.


Ready for submit!

