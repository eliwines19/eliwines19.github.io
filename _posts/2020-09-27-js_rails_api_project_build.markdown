---
layout: post
title:      "JS/Rails API Project Build"
date:       2020-09-27 15:39:49 -0400
permalink:  js_rails_api_project_build
---


## The application

To start off this project, my task was to create a JS SPA(single page application) with a rails api backend, using AJAX coding patterns(asynchrony!), with a minimum of 3 fetch requests. The code had to be organized into classes, I had to have at least one has many relationships between models, and at least 2 CRUD actions.

My application I created is a workout planning application. There are two forms at the top of the application, one of which is to create a workout, which is created with a name, and 5 exercises. The other form directs the user to create an exercise, which is created with a name, muscle group the exercise works on, the amount of reps they must do, and instructions on how to complete the exercise. Along with these two forms, the web page will have workout cards, which shows all the workouts that have been created, along with the exercises to be perfomed with that workout.

## Setting up the backend

To start off, I had to create the backend of my application. My backend is a Rails API meant to be referenced when pulling information needed for my application.

My database has three tables: 

* A Workout table, meant to contain the name and exercises for each Workout created
* A Exercise table, meant to store the information on each exercise
* A ExercisesWorkout table, which serves as my join table to hold my Workout and Exercise ID's

After generating the models, controllers, and migrations using `rails g resource <ModelName>` I had to establish my relationships between each model in the models files:

```
class ExercisesWorkout < ApplicationRecord
  belongs_to :workout
  belongs_to :exercise
end
```

```
class Workout < ApplicationRecord
    has_many :exercises_workouts, dependent: :delete_all
    has_many :exercises, through: :exercises_workouts
end
```

These relationships allow me to establish my 'many-to-many' relationship betweens the models. This basically means that workouts and exercises can be created, but they don't rely on one another for creation. I am able to create an Exercise without having to attach it to a Workout, and vice versa.

I then moved on to creating the actual API to be referenced by my front end.
In my WorkoutsController:
* My Index action, showing all created Workouts and all the information for them
* My Show action, showing an individual workout based on the id params sent into the URL
* My Create action, allowing me to create new workouts from the web page

In my ExercisesController:
* My Index action, showing all exercises, along with any workouts that exercise belongs to
* My Show action, showing an indivudual exercise based on the params sent to the URL
* My Create action, allowing me to create new exercises from the web page

I also used the 'fast_jsonapi' gem, which allows me to tell the API which information about each model will be shown on the API in a much quicker way. Rather than typing everything out, I can quickly define what will be show with a few short lines of code.

All controller actions are rendered to the API using json, for example:
```
    def index 
        workouts = Workout.most_recent
        render json: WorkoutSerializer.new(workouts)
    end
```

After the relationships were established, I was able to seed my database with a few example workouts, and then I knew I could begin creating my front end.

## Creating the Front end

With my database created and my seed data stored in that database I knew the first step to creating the application was to fetch the data in my API. A simple fetch request to my '/workouts' URL looks like this:

`fetch("http://localhost:3000/workouts").then(resp => resp.json()).then(json => console.log(json.data))`

With this fetch request, in my console is all the information found in the 'http://localhost:3000/workouts' API. From here I am able to create workout cards for each of my workouts seeded into the database. Finally the app is starting to come together! But now comes the challenge of creating a fetch request, along with another parameter passed in most commonly known as the "configuration object". With the API URL and the configuration object passed in as the parameters I will be able to post an exercise or workout to the API from the web page:

``` 
            const configurationObject = {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "Accept": "application/json"
                },
                body: JSON.stringify({
                    "name": workout.name,
                    "exercises": workout.exercises
                })
            }
						
        return fetch("http://localhost:3000/workouts", configurationObject)
        .then(resp => resp.json())
        .catch(error => console.log(error))
```

With a fetch request such as the one shown above, along with the create action in the WorkoutsController, we are able to POST values passed into the body and add to the already existing API.

I created a method such as the one above so I am able to create exercises as well. With these functionalities added to my project, I am able to post new exercises and workouts to the api and apply them to the already existing DOM to create my fully functioning workout planning application!

Many more details and functions went into creating my application, check out my Github Repo link below to see the full code. You can view my video demo with the link below as well. Enjoy!

* Github Repo: https://github.com/eliwines19/workout-planner
* Video Demo: https://drive.google.com/file/d/1XYu1g8J8jzIiMgEjnFZDQjhYuuZA-BEI/view
