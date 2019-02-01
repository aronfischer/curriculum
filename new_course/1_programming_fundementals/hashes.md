### Introduction
Now that you've learned about arrays, it's time to get into the array's supercharged cousin: **the hash**. Ruby hashes are more advanced collections of data and are similar to object in JavaScript and dictionaries in Python, if you're familiar with those.

### Learning Outcomes
By the end of this lesson, you should be able to do the following:

 - Explain what a hash is.
 - Describe how to create a new hash and how to add information to it.
 - Explain what keys and values are.
 - Describe how to change and delete data inside a hash.
 - Describe how to merge two hashes.

### Arrays vs. Hashes
You're sitting in your cucible, diligently working away (because you would never dream of slacking off at work), when lunch time rolls around. You need to grab a bite to eat, but how are you going to go about requesting food? For the purposes of this parable, you have two options: a vending machine or a nice restaurant. 

If you were to go to the vending machine, you would see nice, orderly rows of food where each option is labeled with an identifier like "A7" or "C2". These labels are the indices of the vending machine array. It's important to note that the indices are not interchangable: "A2" will always come before "A3" and after "A1". You request your food by using an index to tell the vending machine what you want. It understands the index and returns whatever lives in that spot. Mmmmm, nothing like a lunch of Flamin' Hot Cheetos and Diet Coke! You are a programmer, after all.

Your other option is to sit yourself down at a table covered with a nice white tablecloth, where a pleasant waiter will see to your every need. The first thing they will do is a bring you a menu, which for those of you that have only been eating out of vending machines so far in your life, lists out all of your dining options labeled with the name of the dish, such as ["sublimated artichoke frittata" or "whole pork belly, market acorns, and activated shell bean"](http://www.brooklynbarmenus.com/). In this menu hash, the dish names are called **keys**: they are the labels that are used to identify your dining options. The food that those dish names represent are the **values** that the keys point to. To order your food, you give your waiter the key (you tell him the name of the dish you want), and he returns the value of that key (food that matches the description on the menu). Mmmmm, nothing like a lunch of free-range bison with corn and peach compote and an IPA! You are a programmer, after all.

There are two important differences to note between the vending machine array and the menu hash. First, it's far easier for us to use the names of things to find what we're looking for than to have to translate what we want into numerical indices. This is a huge advantage of using a hash: no more having to count out array elements to request what we want! Second, the items on a menu can appear in any order, and we'll still get exactly what we want as long as we use the correct name. This unordered aspect of hashes isn't true for arrays, which are highly dependent on order. 

### Creating Hashes
Let's dive in and create a hash! 

~~~ruby
my_hash = { 
  "a random word" => "ahoy", 
  "Dorothy's math test score" => 94, 
  "an array" => [1, 2, 3],
  "an empty hash within a hash" => {} 
}
~~~

This example shows the most basic way to create a hash, which is to use the hash literal of curly braces (`{}`). 

The above hash has four keys that point to four different values. For example, the first key, "a random word", points to the value "ahoy". As you can see, the values of a hash can be a number, a string, an array, or even another hash. Keys and values are associated with a special operator called a **hash rocket**: `=>`.

Just like with an array, you can also create a new hash by calling good old `#new` on the `Hash` class.

~~~ruby
my_hash = Hash.new
my_hash               #=> {}
~~~

Of course, hashes don't only take strings as keys and values. Ruby is a pretty flexible language, so you can jam any old thing in there and it'll work just fine.

~~~ruby
hash = { 9 => "nine", :six => 6 }
~~~

### Accessing Values
You can access values in a hash the same way that you access elements in an array. When you call a hash's value by key, the key goes inside a pair of brackets, just like when you're calling an array by index.

~~~ruby
shoes = {
  "summer" => "sandals",
  "winter" => "boots"
}

shoes["summer"]   #=> "sandals"
~~~

### Adding Data
You can add a key-value pair to a hash by calling the key and setting the value, just like you would with any other variable. 

~~~ruby
shoes["fall"] = "sneakers"

shoes     #=> {"summer"=>"sandals", "winter"=>"boots", "fall"=>"sneakers"}
~~~

### Removing Data
Deleting data from a hash is simple with the hash's `#delete` method, which provides the cool functionality of returning the value of the key-value pair that was deleted from the hash.

~~~ruby
shoes.delete("summer")    #=> "sandals"
shoes                     #=> {"winter"=>"boots", "fall"=>"sneakers"}
~~~

### Methods
Hashes respond to just about all the same methods as arrays do, since they're both part of Ruby's *Enumerable* class. Keep an eye out in the next lesson on the Enumerable class for the difference in the way that arrays and hashes handle Enumerable methods -- the `#each` method especially.

Another couple of useful methods for hashes are the `#keys` and `#values` methods, which do just what you think they do. Note that these methods return *arrays*.

~~~ruby
books = { "Infinite Jest" => "David Foster Wallace", "Into the Wild" => "Jon Krakauer" }
books.keys
  => ["Infinite Jest", "Into the Wild"]
books.values
  => ["David Foster Wallace", "Jon Krakauer"]
~~~

### Merging Two Hashes
It'll happen every now and again that two hashes need to come together in holy union. Luckily, there's a method for that.

~~~ruby
hash1 = { "a" => 100, "b" => 200 }
hash2 = { "b" => 254, "c" => 300 }
hash1.merge(hash2)
  => { "a" => 100, "b" => 254, "c" => 300 }
~~~

Note that the hash getting merged in (in this case, `hash2`), has precedence over the hash getting... uh, merged *at*, when both hashes share a key.

For a list of methods that work on hashes, check out the [Ruby Docs](http://ruby-doc.org/core-2.1.1/Hash.html).

### Best Practices
In this explanation, we mostly used strings for hash keys, but in the real world, you'll see symbols like `:this_guy` as keys significantly more often. This is predominantly because symbols are a lot better performant than strings in Ruby, but also because they allow for a much cleaner syntax when defining hashes. Behold:

~~~ruby
# 'Rocket' syntax
american_cars = { :chevrolet => "Corvette", :ford => "Mustang", :dodge => "Ram" }
# 'Symbols' syntax
japanese_cars = { honda: "Accord", toyota: "Corolla", nissan: "Altima" }
~~~

Notice that the 'hash rocket' and the colon that represents a symbol have been mashed together. This unfortunately only works for symbols, though, so don't try { 9: "value" }, or you'll get a syntax error.

### Assignment

<div class="lesson-content__panel" markdown="1">
1. Follow along Launch School's chapter on [Hashes](https://launchschool.com/books/ruby/read/hashes), and go through the exercises using IRB or any other REPL, such as [repl.it](https://repl.it/languages/ruby).
</div>


### Additional Resources
This section contains helpful links to other content. It isn't required, so consider it supplemental for if you need to dive deeper into something.

* Read through [Ruby Explained: Hashes](http://www.eriktrautman.com/posts/ruby-explained-hashes) by Erik Trautman.

### Knowledge Check
This section contains questions for you to check your understanding of this lesson. If you're having trouble answering the questions below on your own, review the material above to find the answer.

* What's the difference between hashes and arrays?
* How can you create a new hash?
* How can you populate that hash with data?
* What are keys and values in a hash?
* How can you change existing values within a hash?
* How can you delete existing data from a hash?
* How can you merge two hashes together?