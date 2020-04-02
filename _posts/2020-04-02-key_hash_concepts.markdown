---
layout: post
title:      "Key Hash Concepts "
date:       2020-04-02 15:15:28 +0000
permalink:  key_hash_concepts
---


As I explained in my last blog, hashes are extremely useful tools to use in Ruby for containing large amounts of information. Hashes are basically like a dictionary, you have the word, then the definition next to it, but what if you don't understand a word IN the definition? Well just flip a couple hundred pages and you'll find that word too! 

Hashes have one name that represents the large picture of what the hash will contain, then sub-categories that contain more detailed information of the hash, then more sub-categories etc. etc!!

Lets use an example of a hash that we will call "friends", well that hash will, of course, contain your friends! But friends have attributes that you might want to know about them (I mean they are your friend of course)...lets just make an example of this:

friends = {

   "Lebron James" => {
	 
	    nickname: "King James", 
			
			accomplishments: ["3 rings", "6 finals appearances"], 
			
			children: "3"
			
		}	
			
	 "Johnny Knoxville" => {
	 
	   nickname: "Johnny",
		 
		 accomplishments: ["3 jackass movies", "37 broken bones"],
		 
		 children: "3"
		 
		 }
		 
	}	 
	
	This hash contains friends of mine! Yes I am totally friends with Lebron James and Johnny Knoxville... Anyways, the hash friends has 2 values, Lebron James and Johnny Knoxville, these values also have values like nickname, accomplishments and children, these values ALSO have values that contain data about the last 3 values... so as you can see, hashes can get pretty huge and complicated, but its all connected! 
