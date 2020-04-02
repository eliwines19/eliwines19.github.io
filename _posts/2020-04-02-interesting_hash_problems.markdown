---
layout: post
title:      "Interesting Hash Problems"
date:       2020-04-02 14:59:46 +0000
permalink:  interesting_hash_problems
---


Hashes are basically giant groups of data. They're like an array, but a little more complicated because they have keys and values, whereas arrays just have values. Theyre more efficient when it comes to storing data inside of data! But of course, with more power comes more responsibility!

An interesting problem I have solved while dealing with hashes was probably the "simple nesting" lesson in OO Ruby. This lesson was pretty easy, but certinly taught me a lot on how to access information on hashes! The lesson was basically learning how to manipulate keys and values located inside of a hash.

Heres an example of one of the methods: 

def adding_to_dennis
	programmer_hash =
 		{
        :grace_hopper => {
          :known_for => "COBOL",
          :languages => ["COBOL", "FORTRAN"]
        },
        :alan_kay => {
          :known_for => "Object Orientation",
          :languages => ["Smalltalk", "LISP"]
        },
        :dennis_ritchie => {
          :known_for => "Unix",
          :languages => ["C"]
        }
     }

     programmer_hash[:dennis_ritchie][:languages] << "Assembly"
     programmer_hash
end

What was required of me for this method was that I had to push "Assembly" into the array of the key "languages" that was inside of the key "dennis_ritchie" and then return the programmer hash to show that "Assembly" was added. Seems complicated (and it is at first) but over time you start to see how simple and self-explanatory it really is! 

Hashes are certainly a valuable tool for Ruby, and I look forward to working with them more often! One step at a time!
