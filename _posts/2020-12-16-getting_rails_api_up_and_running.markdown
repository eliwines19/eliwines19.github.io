---
layout: post
title:      "Getting Rails API up and running"
date:       2020-12-16 15:18:12 +0000
permalink:  getting_rails_api_up_and_running
---


## Intro

Before we can start fetching data from the backend in our React App, we first have to setup the data that will be presented in the API. In this quick tutorial, I will be explaining how to create tables for the database, migrating files, and setting up our API. If you are following along from my last blog post you should already have set up both the front end and backend of our application on github. This post will only be editing the backend of our app.

## Migrations

Rails makes creating migration files extremely easy. For my application I'm keeping it very simple. The only table I'm going to create for now is a 'Posts' table. The more tables you have, the more complicated a Rails application gets, but having just one table is easy to manage. Remember, a table is meant to represent any data you plan to store in your database for use in your application, so setting up your tables for a more complicated app requires a lot of planning before taking any drastic steps.

For now, I will use `rails g resource` to create everything I will need for my 'Posts' table.
In the command line, type:

```
rails g resource Post
```

This will generate a few files that we will be using later on, but for now go to 'db/migrate' and you will see a file with a ton of numbers, and ends with '...create_posts.rb'. There you will see some code like this:

```
class CreatePosts < ActiveRecord::Migration[6.0]
  def change
    create_table :posts do |t|
      t.timestamps
    end
  end
end
```

This is exactly what we were looking for!

Now, we want to define our tables columns. We have our 'Posts' table, but what exactly are we going to be storing in it? For my application, there are only 3 things I will be storing; Title, Content, and Likes. We can represent that in the above code like:

```
class CreatePosts < ActiveRecord::Migration[6.0]
  def change
    create_table :posts do |t|
      t.string :title
      t.string :content
      t.integer :likes

      t.timestamps
    end
  end
end
```

This example of a migration is very simple and easy to follow. Things can get much more complicated when you have more data involved. Check out [the rails docs](https://guides.rubyonrails.org/active_record_migrations.html) for a more detailed dive into rails migrations.

Now that we have this file filled out with our columns for the database, we simply run a command to apply it to our database. If you followed along with my recent post, you should have a postgresql database which is supported by heroku. Otherwise, rails has a default database: SQL.

Type the migration command:

```
rails db:migrate
```

If everything works you can now visit 'db/schema.rb' and you can see your table has been created!

## Seeding the database

To make sure your table is set up, run `rails c` on your command line to bring up the rails console. From there you can write ruby code, as well as access your database.

Lets make sure everything is working.

In your command line, type the following:

`p = Post.new`

This should return something like:

```
<Post id: nil, title: nil, content: nil, likes: nil, created_at: nil, updated_at: nil> 
```

This is good. This means you just created a new post that has zero information so far, so lets give it some!

```
p.title = 'post title'
p.content = 'content for this new post'
p.likes = 100
p
```

Now, we've given our new post some information, after entering that last `p` it should return something like this:

```
<Post id: nil, title: "post title", content: "content for this new post", likes: 100, created_at: nil, updated_at: nil>
```

Awesome! Notice how the Post does not yet have an id, created_at or updated_at. That is because this post has not yet been saved to the database! Lets do that now:

```
p.save!
```

This command does two things. You may be wondering, why the exclamation point at the end? I could have very easily just typed `p.save` and that would have saved the post just the same, but adding the exclamation point at the end returns true or false based on whether the post actually saved or not. If the post saved, you should see something like this:

```
   (0.2ms)  BEGIN
  Post Create (12.5ms)  INSERT INTO "posts" ("title", "content", "likes", "created_at", "updated_at") VALUES ($1, $2, $3, $4, $5) RETURNING "id"  [["title", "post title"], ["content", "content for this new post"], ["likes", 100], ["created_at", "2020-12-16 14:28:50.200699"], ["updated_at", "2020-12-16 14:28:50.200699"]]
   (8.7ms)  COMMIT
 => true 
```

This is ruby doing some SQL 'magic' for you. It inserts the new post and all of its data attributes into our SQL database so that it can be referenced for later. Notice that at the end it returns true ( `=> true` ). This is the doing of our exclamation point or 'bang' symbol at the end of our save command. If the post did not save, this would have returned false, as well as an error message explaining why the post could not be saved. Very useful!

Now, type `p` in your command line to reference our newly saved post. It now has an id, created_at, and updated_at filled out. Success!

Okay, so we set up or table, tested our database, now what?

## fast_jsonapi

For starters, we're going to install a ruby gem used for getting API's up and running quickly:

`gem 'fast_jsonapi'`

This is a gem created by netflix that displays our databse information in our API in a clean, readable way. This gem is not necessary, but I do reccomend using it.

Once that is in our gemfile, run `bundle` to get it installed.

Great, now we have a new command line tool to use, lets try it out:

`rails g serializer Post`

this should have created a new folder for you: 'app/serializers'. In the folder there should be a 'post_serializer.rb' file where you will see some code like this:

```
class PostSerializer
  include FastJsonapi::ObjectSerializer
  attributes
end
```

Awesome! Now we're going to define what attributes from our 'Post' table we want displayed in our API:

```
class PostSerializer
  include FastJsonapi::ObjectSerializer
  attributes :title, :content, :likes, :created_at
end
```

Great! Now, lets get this API up and running!

## API up and running

This is where things start to come together. We're going to get our API up and running so its data can be referenced from our front end application.

Lets go to 'app/controllers/posts_controller.rb', this is a file that should've been added automatically when we first ran our `rails g resource Post` command earlier on.

Here you should see an empty ruby class. This is actually where we will define our controller actions.

lets set up a basic index action, which will ultimately gather up all of our posts in our database into one route:

```
class PostsController < ApplicationController

    def index
        posts = Post.all
        render json: PostSerializer.new(posts)
    end
    
end
```

Here we define our index action. Inside the index action we are going to create a variable, posts, that represents all of our posts in our database via Post.all

Next, we render some json based on what is stored inside our 'posts' variable. We are using our PostsSerializer to render the json in a more clean and readable way.

Thanks again to our `rails g resource Post`, because rails is already aware of this controller action! See your 'config/routes.rb'.

Once we have this controller action, we can see it on our server! type `rails s` in your console to get a rails server up and running, then go to http://localhost:3000/posts on your browser and you will see the rails API!

You should even be able to see the post you created earlier from your rails console on the API:

```
{
   id: "7",
   type: "post",
   attributes: {
      title: "post title",
      content: "content for this new post",
      likes: 100,
      created_at: "2020-12-16T14:28:50.200Z"
   }
}
```

You may be thinking "Well great now I can start fetching this info from my frontend" BUT there is actually one more thing you need to do... CORS.

## Configuring CORS for our Rails App

CORS stands for Cross-origin resource sharing. This allows us to gather information from our rails domain from a different domain (AKA our frontend), but we must first enable CORS in our rails app.

install the `rack-cors` gem in your Gemfile. 

#### Note

This should already be in your gemfile, it is just commented out. Uncomment the gem and run `bundle` to install it.

Next, go to 'config/initializers/cors.rb'

There you will see a bunch of commented out code.

Uncomment out this code: 

```
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins 'example.com'

    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

change where it says `origins 'example.com'` to `origins '*'`

This basically means that this rails application will allow any other domain to make HTTP Requests to its server. Later on, if you get your frontend application up and running on a server like netlify, you need to change the `*` to your domain name which will probably look something like `app-name.netlify.app`

## That's it!

I covered a lot of basic Ruby on Rails functionalities in this tutorial, but there are still many ways to diver deeper into Rails. I suggest reading [the rails docs](https://guides.rubyonrails.org/) to get a better understanding of what is going on. I hope you learned a thing or two from this tutorial, have fun creating your application!


