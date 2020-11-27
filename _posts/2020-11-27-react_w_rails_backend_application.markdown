---
layout: post
title:      "React w/ Rails backend Application"
date:       2020-11-27 17:55:09 +0000
permalink:  react_w_rails_backend_application
---

## Intro

This blog post will explain the technical aspects of my React Application with a Ruby on Rails backend. The Application I created is called 'Writers Notebook', and it is basically a story sharing application. The idea was to allow the users to create an account, come up with story ideas, create characters that belong to those stories, and allow the user to share the story with other users. I call it the Writers Notebook because it is meant for writers to jot down their ideas for a new story and get an opinion on it from others.

## The Backend

To set up the backend there was a couple of things I needed to plan out. A major one being my database tables, then setting up my Models, then my controllers, and then a few other little things to make it all accessible to the front end.

### Tables

In my application I have 4 tables: User, StoryIdea, Character, Comment.

Everything first stems from the User. The User account must be created first. From there on a User can create a StoryIdea that belongs to them. Once a StoryIdea is created, You can create Characters and Comments that belong to that StoryIdea.

#### The User
```
has_many :story_ideas
has_many :characters
has_many :comments, through: :story_ideas
```

#### The StoryIdea
```
  belongs_to :user
  has_many :comments
  has_many :characters
```

#### The Character
```
  belongs_to :user
  belongs_to :story_idea
```

#### The Comment
```
  belongs_to :user
  belongs_to :story_idea
```

### The Controllers

Each of my tables has their own controller. Each controller has basic CRUD actions built into them. For example, in the StoryIdeaController I have the following actions : Index, Show, Create, Update, Destroy.

However, since I am building a Frontend Application that fetches data from the Backend and not a purely Rails application, I can't just create instance variables that can be sent to the Views, instead I must render some JSON to make my own API, which is fetchable from the front end.

For example:
```
    def index 
        story_ideas = StoryIdea.all.reverse_order
        render json: StoryIdeaSerializer.new(story_ideas)
    end
```

As you can see, in this index action there are a few things going on:

1. I create a variable 'story_ideas' that is set equal to 'StoryIdea.all.reverse_order', which basically means all the StoryIdeas in the reverse order(I do this so that the JSON API shows the newest StoryIdea first)

2. I render some json which is set to 'StoryIdeaSerializer.new(story_idea)'  which is what renders the json to the page.

You might be wondering, what is that 'StoryIdeaSerializer.new', well it is basically just a gem that renders serialized data, I could have just put 'render json: story_ideas' and that would work too. The 'fast_jsonapi' gem makes this possible.

I also set up some authentication, which means I have both a sessions controller and a users controller. In the sessions controller, I use the BCrypt gem to allow authentication, so when a User logs in, it has to authenticate the user before allowing them any further, this is also made possible in the User controller where I put the method 'has_secure_password', this allows for the 'password_digest' attribute for the user. With this implemented nobody can see the users password. The Bcrypt gem is very useful for basic authentication and I would reccomend using it for any basic application.

read more on it here: https://gist.github.com/thebucknerlife/10090014

### gem 'rack-cors'

In order to allow the Frontend to manipulate data in the Backend, you must install the 'rack-cors' gem. Here you can define what domain name can manipulate data and their optios for manipulating that data.

To start off, install the gem, then create a file in the backend in the config/initalizers folder called 'cors.rb'.

In the config/initializers/cors.rb file, you will put this:

```
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins "http://localhost:3000"
    resource '*', headers: :any, methods: [:get, :post, :patch, :put, :delete],
    credentials: true
  end
end
```

What we are doing is enabling HTTP requests to the backend from the frontend located at "http://localhost:3000",
below that we are enabling the GET, POST, PATCH, PUT, DELETE HTTP methods. Without this enabled. You won't be able to do things such as deleting a story or editing a character. Cors makes this possible.

Before continuing on to the front end there is one more thing we need to configure. In the config/initializers folder we also need to create a file called 'session_store.rb'

In config/initializers/session_store.rb we put this:

`Rails.application.config.session_store :cookie_store, key: '_auth_app'`

The '_auth_app' is customizable, but the rest is what enables sessions in your application. Meaning every time I refresh the page I dont have the log in, the session will allow me to stay logged in to the application until I log out.

Now we are ready to move on to the frontend.

## The Frontend

first things first, we use 'npx create-react-app app-name' to create our React application. Once that loads we can start!

### Setting up redux

run `npm install redux && npm install react-redux`

In our 'src' folder, we want to create a seperate folder where we will keep all of our redux code seperate from out components. First things first we need to set up the redux store.

We need to import a few things in order to begin:

`import { createStore, combineReducers, applyMiddleware, compose } from 'redux'`

createStore is the method we use to create a redux store, combineReducers is necessary because we are going to have more that one reducer in our store, applyMiddleware is necessary because we are also going to be using thunk, and compose is necessary for our redux devtools.

below we will define our rootReducer, which will be set equal to the combineReducers method along with all of the reducers:

```
const rootReducer = combineReducers({
  auth: authReducer,
  stories: storyIdeaReducer,
  characters: characterReducer,
  comments: commentReducer
});
```

Keep in mind, the 'authReducer', 'storyIdeaReducer', 'characterReducer', and 'commentReducer' were all created in another file and imported into this one, this keeps our reducer files seperate from our store file.

After we combine the reducers, we create the store with this rootReducer variable:

```
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const store = createStore(
  rootReducer,
  composeEnhancers(applyMiddleware(thunk))
);

export default store;
```

The variable 'composeEnhancers' is set equal to the function required for the redux dev tools, read more about this here: https://github.com/zalmoxisus/redux-devtools-extension

In our store we add the rootReducer, which is a combination of all our reducers and the redux devtools code. Keep in mind that the redux devtools is not required for this to work, it is a simply a tool that connects to your app to help you keep track of the redux store.

### Actions an Reducers

In order to interact with the redux store, we need both reducers and action. an Action can technically be defined in any component you want to be interacting with redux, but the whole point of react and redux is to keep like things together and things that arent alike seperate so I define the actions and reducers inside that redux folder, which can of course be imported into our component files later on.

Lets take a look at an action and a reducer:

```
export const fetchStoryIdeas = () => {
    return dispatch => {
        dispatch({ type: 'STORY_IDEAS_LOADING' })
        fetch('http://localhost:3001/story_ideas', { credentials: 'include' })
        .then(resp => resp.json())
        .then(json => {
            dispatch({
                type: 'STORY_IDEAS_SUCCESS',
                payload: { storyIdeas: json.data }
            })
        })
    }
}
```

Here in src/redux/actions, I have this code. This might seem slightly confusing, but dont worry, IT IS.

So remember back where we created our store variable, we gave it two arguments. One was our rootReducer, and the other was this method with an argument that was also a method with an argument? Kind of confusing but I will try to break it down.

The inital argument, 'composeEnhancers' is what is required for the redux devtools, this also takes in an argument, which is 'applyMiddleware' which we imported from redux, this allows us to apply some middleware to our application, now this 'applyMiddleware' ALSO takes in an argument, which is the middleware itself. In this case, that middleware is 'thunk'.

As quoted from this documentation here: https://www.digitalocean.com/community/tutorials/redux-redux-thunk#:~:text=Redux%20Thunk%20is%20a%20middleware,asynchronous%20operations%20have%20been%20completed.

Redux thunk is a middleware which allows you to call action creators that return a function instead of an action object, this function returned from the action is a dispatch to our reducer. It is then used to dispatch regular synchronous actions... so what is this all required for?

Well, In our case, thunk is used to make a fetch request synchronous instead of asynchronous. Why? Because our dispatch above (and like many other dispatch actions that communicate with the backend) sends the data from the API to our store, but if the fetch request is asynchronous, the dispatch action will be sent BEFORE the fetch request is complete, causing our reducer to throw an error saying it has no idea what we're talking about.

This is all very confusing, and takes some time to wrap your head around, so I would suggest reading some documentation on redux and thunk in order to make sense of this.

Anyways, back to the action...

So basically what that action above does is sends a get request to our backend so that we can take the data from our backend and store it in our redux store. Then, in our components, we can reference this data by connecting to the store and display it on the page for the user to see.

### Container Components

Now that we have our store, we need to be able to connect to this store so that we show this information in the store, and create event listeners that trigger our actions.

Lets take a look at our CharacterContainer Component.

Lets start off by importing a few things:

```
import React from 'react';
import Characters from '../CharacterComponents/Characters';
import { connect } from 'react-redux';
import { deleteCharacter, updateCharacter } from '../../redux/actions/CharacterActions'
```

1. We always import React when creating a React Component
2. We import Characters, which will be our presentational component to render the characters to the screen
3. we import the 'connect' function that allows us to connect to our redux store
4. we import our deleteCharacter and updateCharacter actions from CharacterActions file

Now at the bottom of our file, we will actually connect to the store where we export the application:

```
const mapStateToProps = state => {
    return {
        currentUser: state.auth.currentUser,
        loaded: state.characters.loaded,
        characters: state.characters.characters
    }
}

export default connect(mapStateToProps, { deleteCharacter , updateCharacter })(CharacterContainer);
```

There are a few things happening here.
First, we have a function 'mapStateToProps' which returns an object
Second, we export connect, which takes in two arguments, the first is the function above it, the second is another object.

What does mapStateToProps do? Well, it accesses our redux store, and allows me to take things stored there and make them props for my component that connects to them. So now I can access the currentUser via 'this.props.currentUser'

What does that second argument do? It makes my actions that were imported in the file and makes them props as well, that way I could pass those down to other components and trigger the action.

## Conclusion

Overall, my appreciation for redux grew while creating this application. At first it seemed like a whole lot of setting up just to do something very small, and it honestly is. The reason we use redux is because it is easier to have one giant store that contains all our apps data and can be access from any component, rather than passing down props all the way from a great grandparent component to a great grandchild component. While it does serve this purpose, if your app is not large, it is not worth setting up... BUT for an application the size of this one or bigger I would definitely say its worth it.


