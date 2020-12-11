---
layout: post
title:      "Setting up a React Frontend w/ Rails API Backend on Github"
date:       2020-12-11 15:05:44 -0500
permalink:  setting_up_a_react_frontend_w_rails_api_backend_on_github
---


## Intro

I have just graduated from Flatiron School where I learned how to create Full Stack Web Applications using a React JS Frontend and a Ruby on Rails Backend. For my next project I am going to be building a simple Blogging Application using this same languages and frameworks.

This will be a tutorial explaining the beginning stages of this project: Setting up the Frontend and Backend and creating a new Github Repository!

### Disclaimer

I will be giving instructions on how to create two individual Github Repositories: One for the Frontend and another for the Backend. Personally, this is easier for me to manage and set up, but keep in mind that this means you will constantly have two terminals open and two terminals where you will write commit messages. Don't get them mixed up!

## First Steps

First things first, make a folder for your code!

```
mkdir <app-name>
```

Once this is done, cd into the folder:

```
cd <app-name>
```

## Create the Rails API

You'll need to have Ruby and Ruby on Rails installed onto your computer before you can use `rails new`

```
rails new <app-name-backend> --api --database=postgresql
```

cd into your newly created app:

```
cd <app-name-backend>
```

### Notice

* the `backend` part of my rails app name is completely optional, but recommended.

* Using the `--api` flag lets rails know that we are solely creating a Rails API, which limits the amount of functionality our Rails app will be installed with. Only use this flag if you are creating an API.

* I use the `--database=postgresql` so that I can eventually deploy the app on heroku which only supports postgresql. If you have other intentials the default database for Rails will work fine.

delete the already existing .git file so you can manually create a new Repo from Github:

```
rm -rf .git
git init
```

Now go to Github and create a new Repository.

Once the Repo is created type the following into your command line:

```
git add README.md
git commit -m "first commit"
git remote add origin … (your SSH)
git branch -M main
git push -u origin main
```

## Creating the React Frontend

First things first, you'll want to have the `create-react-app` command installed in order to set up a React Application.

```
npm i -g create-react-app
```

cd back into your application folder:

```
cd ..
```

Run the command (Again, the `frontend` in the app name is optional):

```
npx create-react-app <app-name-frontend>
```

cd into the newly created React App:

```
cd <app-name-frontend>
```

delete the already existing .git file:

```
rm -rf .git
git init
```

Now Again, go to Github and create a new Repo and type the following commands:

```
git add README.md
git commit -m "first commit"
git remote add origin … (your SSH)
git branch -M main
git push -u origin main
```

## Thats it!

While these are very basic commands, they are an essential steps in contributing to the Github community. You want to stay active and always keep working on your craft and working on a new project is a great way to do that. Enjoy creating your new app!







