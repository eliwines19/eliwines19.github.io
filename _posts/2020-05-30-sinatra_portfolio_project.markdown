---
layout: post
title:      "Sinatra Portfolio Project"
date:       2020-05-30 21:51:38 +0000
permalink:  sinatra_portfolio_project
---


This blog post will explain my experience a knwoledge used while completing the Sinatra Portfolio Project fot Flatiron School!

The main goal of this project was to create a MVC (model, controller, and views) web application using multiple models(user and superheroes in my case), having user accounts, be able to perform CRUD actions (Create, Read, Update, and Delete), and ensure that users only are able to edit their own date, not others.

The Web app I decided to create was a Create a Superhero application that allowed users to customize their own superhero attributes, post them, and edit them whenever they please. the attributes every hero has is a name, superpower, motto, suit color, city, and age. The user is able to edit this information as they please, and can even delete a hero they have created if they please. 

While creating this app, I struggled mostly with the controllers. Understanding the controller actions and how they interact with the web page is something I still have trouble understanding, but I have understood it enough to where I could complete this web application. While creating the Application Controller, I understood that i have to enable sessions, which basically allows a user to create a session every time they log in, and the session is exited once the user logs out. The application controller also allowed me to create some helpers like 'redirect_if_not_logged_in', which would tell the user they have to be logged in to complete that specific action they attempted, 'logged_in?' which basically checked if the session and the user_id corresponded, if they didn't correspond, the method would return false(meaning the user isn't logged in), and 'current_user' which sets the session id to the users id so the application knows that this session belongs to this specific user.

The SuperheroController class created routes for the website, displaying specific information on certain pages and allowed pages to be updated with new information. the get action reads information, the post action will create new or update old information, the delete action deletes previously create info, all of these actions were implemented in the SuperheroController to be able to create, read, update, and delete information.

The UserController class created routes with 'get' and 'post' the get actions got information on the user and the post action created new users when a new user was signing up to use the website.

The Models I created for this app were User and Superhero models. The user model has_many superheroes and also contains the method 'has_secure_password', this allowed for the Bcrypt gem to come into use so that a users password can be kept a secret, even if two users have the same password as one another. The Superhero model belonged_to the user and I also created a method 'valid_params?' that was meant to be used to ensure that the Superhero params were not empty.

The views I created were meant to display all of this information. there is an index.erb that is meant to welcome the user apon arrival to the website, and a layout.erb which contains the main layout of the website while using it. there are also superhero and user views. The superhero views contained a edit.erb which displayed a webpage where a user can edit their already created superhero, a show.erb that shows their newly created hero, and new.erb which displays a page that allows a user to create a brand new superhero, and a index.erb which displays all of a users created superheroes. The users views contained a new.erb, which displayed a sign up page where a user can create a new account, a show.erb which shows a user their superheroes, and a login.erb which displayed a login page where a user could log into an account that belonged to them!

This basically summarizes the information that I put into this web application. My goal for this project was to better undertsand how to create a website and how to manpulate user information. I believe that I achieved this goal and now have a great understanding for how a website like twitter and instagram is created!
