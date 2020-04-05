---
layout: post
title:      "CLI project (technical blog)"
date:       2020-04-03 16:53:03 -0400
permalink:  cli_project_technical_blog
---

CLI class:

'call' method: 
This method calls the 'welcome' method, which prompts the user with a welcome message. The 'main_menu' method is then called, prompting the user with a small list of options to choose from.

'main_menu' method: 
This method directs the user to a small list of options to choose from, The options tell the user to either enter "1", "2", or "exit" in the terminal. If "1" is entered, the 'print_random_meal' method is called. If "2" is entered, the "show_saved_meals" method is called. If "exit" is entered, a 'goodbye' method is called, prompting the user with a goodbye message, then a 'break' is called to break the execution of the app. If any input not listed is entered, the 'wrong_input' method will be called telling the user to try a different input.

'print_random_meal' method: 
This method calls the API class method 'self.get_random_meal', which instantiates a new instance of the Meal class with a set of attributes. a variable is then created and set equal to the API class method 'self.all', which shows the class variable '@@all' which is an array containing all Meal class instances, along with their set of attributes. The user is then prompted with a message, showing two attributes (':name' and ':meal_category') of the last Meal instance in the array. After the message, The 'meal_menu' method is called, prompting the user with a new list of options to choose from, each of which guiding the user to learn more about the Meal Instance that was instantiated in the 'print_random_meal' method. 

'show_saved_meal' method: 
This method creates a variable 'saved_meals' which is set equal to the API class method 'self.all' which contains a class variable '@@all' which is an array of Meal instances along with a set of attributes of that Meal instance. If the 'size' of the '@@all' class variable array is greater than 0 (meaning it contains saved Meal instances) The user is prompted with a message showing any Meal instances saved and the all attributes that belong to that Meal instance. If the '@@all' class variable array is '.empty?' The user will be prompted with a message teling the user that they have not saved any meals yet.

'meal_menu' method:
This method prompts the user with a message, giving the user a list of options to choose from, each of which gives the user more information about the Meal instance created. If the user input is "1", the 'print_meal_instructions' method is called. If the user input is "2", the 'print_meal_image' method is called. If the user input is "3", the 'print_meal_area' method is called. If the user input is "main", the 'save_meal' method is called. If any other input is entered, the 'wrong_input' method will be called, telling the user to try something else.

'print_meal_instructions', 'print_meal_image', and 'print_meal_area' methods:
All of these methods create a variable 'meal_info' which is set equal to 'Meal.all.last' which means it is set equal to that last Meal instance in the '@@all' class variable array. The user is then prompted with a message showing either the 'meal_instructions', 'meal_image', or 'meal_area' attribute of that instance of the Meal class. 

'save_meal' method:
This method asks the user if they would like to save the Meal instance that was just shown to them, if the user input is "y", the user is then directed back to the 'main_menu' method. If the user input is "n", the last Meal instantiated that was shoveled into the '@@all' class variable will be deleted and the user will be directed back to the 'main_menu' method.

Meal class: 

In the Meal class, attr_accessor is created to provide attributes to each instance of a Meal instantiated. The Meal class has attributes of :name, :image_url, :meal_area, :meal_category, and :meal_instructions 

The Meal class also has a class variable '@@all' which is set equal to an empty array

The Meal class has the method 'save' in order to push an instance of a meal, and its attributes, to the @@all class variable array.

The Meal class also has a class method 'self.all' which is created so that the Meal instances stored in the @@all class variable array can be accessed and used in other classes, like I do in the CLI class.

The Meal class also has a 'self.destroy_last_meal' method, which deletes the last instance of the Meal class in the '@@all' class method array.

API class:

The API class contains a class method called 'self.get_random_meal'. This method is created to pull the information located in the API. In this case the API is a single meal, along with many attributes describing that meal

The 'self.get_random_meal' class method also creates a new instance of the Meal class, pulls the name, image_url, meal_category, meal_instructions, and meal_area from the instance of the Meal class instantiated and saves that information using the 'save' method mentioned in the Meal class. The instance of the meal is then printed to be used in the 'call' method mentioned earlier in order to create a new instance of a Meal.
