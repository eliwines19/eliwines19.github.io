---
layout: post
title:      "React App beginners guide"
date:       2021-01-05 13:49:58 +0000
permalink:  react_app_beginners_guide
---


## Intro

So far, in my recent blog posts I have covered how to set up my Ruby on Rails Backend API so that I can sent HTTP requests to the server from my frontend. We covered ActiveRecord Migrations, basic ActiveRecord functions to seed our database with dummy data, basic controller actions, and configuring CORS (Cross-Origin Resource Sharing). All of which is a necessary step in creating your own backend API. If you missed this Article, please [check it out here!](https://eliwines19.github.io/getting_rails_api_up_and_running)

Now that the backend is set up and ready to be reference from the front end, lets get started on creating our React App!

## React App Setup

If you haven't already, check out my [first Article](https://eliwines19.github.io/setting_up_a_react_frontend_w_rails_api_backend_on_github) covering Ruby on Rails and React App setup to get your application started and pushed to your github account!

Once the React app is up and running, type `npm start` in your terminal. This will start running a server on your local browser. After it loads, you'll see a screen telling you to go to `src/App.js` to edit your React App.

Go to `App.js` now, and if you've never had experience with React you'll likely be pretty baffled with what you're seeing. What is in App.js is called a **Functional Component**. React lives and breathes off of components, so you'll be getting very comfortable with them as time goes on.

Something you'll want to know is that there are two types of React Components; **Functional Components** as mentioned above, and **Class Components**, which is the type of component we'll be working with.

Both component types are capable of the same functionalities, some poeple prefer one over the other, but I believe Class Components are the easiest to get the hang of at first as they are pretty straightforward.

## Class Components

Now, I know there's a lot going on in the `App.js` file that may confuse you, but what I want you to do is delete everything in the file. Thats right, just highlight it all, then delete it, because we're going to create our own first Class Component from scratch!

At the top of the file, you'll want to import React to gain the component functionalities required to write code in our React Component

```
import React from 'react';
```

Now, we'll want to create our Class Component called `App` which is where we will write the majority of our code.

```
class App extends React.Component{

  render(){
    return(
      <div>
        First line of JSX!
      </div>
    )
  }
}
```

You may be wondering what's going on here, and that's fine! I'll explain...

Our `App` class is extending the functionalities of our React.Component which was imported into our `App.js` file at the top. With these functionalities added to our class, we are able to write JSX code. JSX stands for JavaScript XML, the best way to explain what it is is to pretty much tell you it's a beautiful combination of HTML and JavaScript that combines the two languages in a clean, and easily readable way.

the `render()` that you see is actually a built in React functionality. It's a function that returns JSX, which is then rendered into the window in your browser.

### A Note on importing and exporting classes

In order for this Class Component to be recognized by the React App, you'll have to export the class itself.

```
export default App;
```

This code goes at the bottom of your file. We need to export our components so they can be reference in other files. Take a look at `index.js`. At the top of the file you will see `import App from './App'`. The `App` component in the `App.js` file is now accessible in the `index.js` file. In the code below, you can see `<App />`. This is how React knows which component to render to the screen!

### Back to App.js

As you can see it returns a <div>, inside of which you can write anything you want. If you already have your app up and running in the browser, what you type in there will be rendered into your browser window. If you inspect the text on the screen, you'll find it is wrapped in a <div>, just like it is written here.

You may be wondering, where does the 'J' in JSX come into play? I'll give a quick example.

In you App class, lets created a constructor, in the constructor we will define a `name`. Once defined in the constructor, this name variable will belong to the App class itself. Take a look.

```
  constructor(){
    super();
    this.name = "Eli"
  }
```

### Note

We call `super()` in the constructor to inherit functionality from it's parent component, which is actually `React.Component`

In the constructor, we defined `this.name = "Eli"`, now in our JSX we will reference this `name` variable.

```
  render(){
    return(
      <div>
        Hello, my name is {this.name}!
      </div>
    )
  }
```

Go to the window and you will see this text with the value of the variable `name` in the browser. The JavaScript is wrapped in curly brackets, this way you can input your JavaScript code directly into your HTML code.

## Rendering a component within a component

The thing that's great about React is it is very useful in seperating concerns, meaning that each component can represent a different piece of the application. We can have a component that handle just the title of the page, we can have a component that handle just the footer, or the navbar, or some content you want to output. Seperating your concerns is a great habit when you want to write clean, readable code, and is a big reason why React is so popular.

### A new component

Let's make two new files in our `src` folder: `Title.js` and `Name.js`.

In `Title.js` you'll want to write a new React Component. A barebones React  Class Component looks like this:

```
import React from 'react';

class Title extends React.Component{

    render(){
        return(
            <div>
                Title Component
            </div>
        )
    }
}

export default Title;
```

Do the same thing in your `Name.js` file.

Now how do we render this component onto the screen?

Go to `App.js` and import the two files like so:

```
import Title from './Title'
import Name from './Name'
```

Now, both components can be referenced in your App.js file, but how do we do that?

In your App Component, delete the constructor, we don't need that anymore. Now, in the render method, we want to render both the Title Component and the Name Component.

```
class App extends React.Component{

  render(){
    return(
      <div>
        <Title />
        <Name />
      </div>
    )
  }
}
```

Now go to your browser and you'll see whatever you have written in both the Title Component and Name Component displayed on your browser.

## Passing Props

Using props is a useful way to make components dynamic and resuable, lets say you have multiple pages where you want to display different titles for each. You could simply pass some props to each Title Component with the title of your choosing.

```
class App extends React.Component{

  render(){
    return(
      <div>
        <Title title={"My First React App"}/>
        <Name />
      </div>
    )
  }
}
```

Inside the tags of the Title Component we defined what we want our title to be equal to, now, this information can be seen inside of our title component.

```
class Title extends React.Component{

    render(){
        return(
            <div>
                <h1>{this.props.title}</h1>
            </div>
        )
    }
}
```

Now the title we defined in our App Component, is now displayed on our screen through the Title Component.

### But what is props..?

Props is actually an object that is passed down from one component to another. If you go to your Title Component and you `console.log(this.props)` you can actually see the object itself.

```
    render(){

        console.log(this.props)

        return(
            <div>
                <h1>{this.props.title}</h1>
            </div>
        )
    }
```

Go into your browser console and you will see the props object: 

```
{title: "My first React App"}
```

You can pass down multiple props to the same component, all of which can be reference the same way shown above. This may all seem confusing, but when you get to creating a full scale React Application you begin to see the usefulness in all of this.

## That's all for now

I know I didn't really touch base at all with the application I'm building in this Article, but I wanted to give a beginner's guide to React Applications to strengthen my own skills, as well as teach anyone these necessary React Developer skills to anyone who is interested. I hope you learned something from this article. I'll be sure to go more into depth on the application I'm building in the next post, as well as communicating with the backend! Happy Coding!
