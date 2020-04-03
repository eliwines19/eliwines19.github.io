---
layout: post
title:      "CLI project (technical blog)"
date:       2020-04-03 20:53:02 +0000
permalink:  cli_project_technical_blog
---

CLI class:

My CLI project starts off by calling a 'call' method I have in my CLI class, this method runs another method in the CLI class 'welcome' which welcomes the user, then calls an instance method in my API class that pulls data from the API I used which gives you a random meal along with a bunch of information about the meal. Lastly, the call method prints the 'main_menu' method.

The 'main_menu' method really only has one option, which tells the user to enter '1' in the terminal, which will take the user to a screen with the random meal name and category. The other options for this 'main_menu' method will be to exit the app by typing 'exit' in the terminal. If any other input is given, the 'wrong_input method will be printed on the screen telling the user to try a different input.

When '1' is entered in the main_menu, it calls another method in the CLI class called 'print_random_meal'. This method will call the class method 'self.all' located in the Meal class. The 'self.all' method in the meal class has all of the attributes for the random meal instance that has been generated. The 'print_random_meal' class will iterate through the array of these Meal attributes and prints a message displaying the name of the meal instance and the category of the meal instance. 

After the instance of the Meal name and category has been displayed, the 'meal_menu' method is called. Similar to the 'main_menu' method explained earlier, this 'meal_menu' gives the user options for what they can do next. These options include printing the instructions for making the meal, printing a url link to an image of the meal, and printing the areas where this meal is most common. Like the 'main_menu' method, if the user input is anything other than the options given, the 'wrong_input' message will again pop up, other than that, the user has the option to input 'main' which will guide the user back to the main menu to generate another random meal.

The methods that the 'meal_menu' directs the user to includes the 'print_meal_image' method, the 'print_meal_instructions' method, and the 'print_meal_area' method. Like the 'print_random_meal' method mentioned earlier, these methods calls the method 'self.all' in the Meal class, giving them the instance of the Meal class. Once this is called, the methods will iterate through the array of attributes describing this particular instance of the Meal, and each prints exactly what is mentioned in th method name. 

Meal class: 

In the Meal class, attr_accessor are created to provide attributes to each instance of a Meal created

The Meal class also has a class variable '@@all' which is set equal to an empty array

The Meal class has the method 'save' in order to push an instance of a meal, and its attributes, to the @@all class variable.

The Meal class also has a class method 'self.all' which is created so that the information stored in the @@all class variable array can be accessed and used in other classes, like I did in the CLI class

API class:

The API class contains a class method called 'self.get_random_meal'. This method is created to pull the information located in the API. In this case the API is a single meal, along with many attributes describing that meal

The 'self.get_random_meal' class method also creates a new instance of the Meal class, pulls the name, image_url, meal_category, meal_instructions, and meal_area from the instance of the Meal class and saves that information using the 'save' method mentioned in the Meal class. The instance of the meal is then printed to be used in the 'call' method mentioned earlier in order to create a new instance of a Meal.
