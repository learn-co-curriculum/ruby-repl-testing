# Manipulating Hashes

## Objectives

In this lesson, we'll be taking a closer look at multidimensional, or nested, hashes, iteration and higher level hash methods. We'll go through a few challenges together and then you'll complete the lab on your own. 

## More On Building Nested Hashes 

Let's say we have the following nested hash: 

```ruby
contacts = {
  "Jon Snow" => {
    name: "Jon",
    email: "jon_snow@thewall.we", 
    favorite_icecream_flavors: ["chocolate", "vanilla"]
  },
  "Freddy" => {
    name: "Freddy",
    email: "freddy@mercury.com",
    favorite_icecream_flavors: ["strawberry", "cookie dough", "mint chip"]
  }
}
```
### Adding Information to Nested Hashes

#### Example 1:

Your good buddy Jon Snow has just tried mint chip ice cream for the first time. He loved it a lot and now you need to add "mint chip" to his list of favorite ice creams. We already know how to access the array of ice cream flavors that constitute the value of the `:favorite_icecream_flavors` key: 


```ruby
contacts["Jon Snow"][:favorite_icecream_flavors]
#  => ["chocolate", "vanilla"]
```

How can we add a flavor to the list? Well, `:favorite_icecream_flavors` is an array, we we can use the same syntax as above to access that array along with our old friend, the `<<`, shovel method, to add an item to the array. Let's take a look: 

```ruby
contacts["Jon Snow"][:favorite_icecream_flavors] << "mint chip"

puts contacts 
#  => {
  "Jon Snow" => {
    name: "Jon",
    email: "jon_snow@thewall.we", 
    favorite_icecream_flavors: ["chocolate", "vanilla", "mint chip"]
  },
  "Freddy" => {
    name: "Freddy",
    email: "freddy@mercury.com",
    favorite_icecream_flavors: ["strawberry", "cookie dough", "mint chip"]
  }
}
```

#### Example 2: 

Now let's say that you want to add a whole new kay/value pair to your Jon Snow contact––his address. In a simpler hash with just one level of key value pairs, we've already learned how to add new key/value pairs. Here's a reminder: 

```ruby
hash = {first: "first value!", second: "second value!"}
hash[:third] = "third value!"

puts hash
#  => {first: "first value!", second: "second value!", third: "third value!"}
```

In a nested hash, we can add brand new key/value pairs very similarly. We just need to first access the level of the hash to which we want to add a key value pair. We want the `"Jon Snow"` key to include a new key/value pair of his address. Take a look at the implementation below: 

```ruby
contacts["Jon Snow"][:address] = "The Lord Commander's Rooms, The Wall, Westeros"

puts contacts
#  => 
{
"Jon Snow" => {
   :name=>"Jon", :email=>"jon_snow@thewall.we", 
   :favorite_icecream_flavors=>["chocolate", "vanilla", "mint chip"], 
   :address=>"The Lord Commander's Rooms, The Wall, Westeros"}, 
"Freddy"=> { 
   :name=>"Freddy", 
   :email=>"freddy@mercury.com", 
   :favorite_icecream_flavors=> ["cookie dough", "mint chip"]
 }
}

```

## Iterating Over Nested Hashes

So far, we've only iterated over hashes that had one level––a series of key/value pairs on a single tier. What happens when we want to iterate over a multidimensional hash like the one above? Let iterate over our nested hash, one level at a time. 

We're going to iterate over the first level of our hash like this: 

```ruby
contacts = {
  "Jon Snow" => {
    name: "Jon",
    email: "jon_snow@thewall.we", 
    favorite_icecream_flavors: ["chocolate", "vanilla", "mint chip"]
  },
  "Freddy" => {
    name: "Freddy",
    email: "freddy@mercury.com",
    favorite_icecream_flavors: ["strawberry", "cookie dough", "mint chip"]
  }
}

contacts.each do |person, data|
	puts "#{person}: #{data}"
end
```

This should return: 

```ruby
Jon Snow    	
{ :name=>"Jon", 
  :email=>"jon_snow@thewall.we", 
  :favorite_icecream_flavors=>["chocolate", "vanilla", "mint chip"]
}

Freddy: 
{ :name=>"Freddy", 
:email=>"freddy@mercury.com", 
:favorite_icecream_flavors=>["strawberry", "cookie dough", "mint chip"]
}
```

On the first level, the keys are our contacts' names: Jon Snow and Freddy, and our values are the hashes that contain a series of key/value pairs describing them. 

Let's iterate over the second level our our `contacts` hash. In order to access the key/value pairs of the second tier (i.e. the name, email and other data about each person), we need to iterate *down into* that level. So, we pick up where we left off with the previous iteration and we keep going: 

```ruby
contacts.each do |person, data|
	#at this level, "person" is Jon Snow or Freddy and "data" is a hash of key/value pairs
	#to iterate over the "data" hash, we can use the following line: 
	
	data.each do |attribute, value|
		puts "#{attribute}: #{value}
	end
end
```

That should output the following: 

```ruby

name: Jon
email: jon_snow@thewall.we
favorite_icecream_flavors: ["chocolate", "vanilla", "mint chip"]

name: Freddy
email: freddy@mercury.com
favorite_icecream_flavors: ["strawberry", "cookie dough", "mint chip"]

```

Now it's your turn. In the challenge below, you'll be *removing* and item from an array in multidimensional hash. 

Let's take is one step further and print out *just the favorite ice cream flavors*. Once again, we'll need to iterate down into that level of the hash, then we can access the favorite ice cream array and print out the flavors: 

```ruby
contacts.each do |person, data|
	#at this level, "person" is Jon Snow or Freddy and "data" is a hash of key/value pairs
	#to iterate over the "data" hash, we can use the following line: 
	
	data.each do |attribute, value|
		#at this level, "attribute" is describes the key of :name, :email or fav. flavor
		#we need to first check and see if the key is :favorite_icecream_flavors. 
		#if it is, that means the VALUE is an array that we can iterate over to print out each element
		
		if attribute == :favorite_icecream_flavors
			value.each do |flavor|
				# here, each index element in an ice cream flavor string
				puts "#{flavor}"
			end
		end
	end
end
```

This should output: 

```ruby
chocolate
vanilla
mint chip
strawberry
cookie dough
mint chip
```

Now it's your turn! You're going to iterate through the levels of this hash to operate on one of the ice cream flavor arrays. 

**Reminder:** Iterating through nested hashes is hard, and (I'm pretty sure) you are not psychic. Meaning, you can't necessarily predict with perfect clarity what the key/value pair is at a certain level of the hash. **Using binding.pry** when you are iterating in upcoming labs to make sure you understand what the key/value pair is that you are iterating over. 

%%%

### Code Challenge I: Manipulating Nested Hashes

Your good buddy Freddy Mercury has recently developed a strawberry allergy. You need to delete "strawberry" from his list of favorite icecreams. Iterate over the below array and when you reach the key of `:favorite_icecream_flavors`, check to see if the array contains strawberry, if it does, remove it. **Hint:** use the `delete_if` method to eliminate strawberry from the appropriate array.

~~~ruby

contacts = {
  "Jon Snow": {
    name: "Jon",
    email: "jon_snow@thewall.we", 
    favorite_icecream_flavors: ["chocolate", "vanilla", "mint chip"]
  },
  "Freddy": {
    name: "Freddy",
    email: "freddy@mercury.com",
    favorite_icecream_flavors: ["strawberry", "cookie dough", "mint chip"]
  }
}

#your code here


#do NOT touch the following line!

contacts

~~~solution

contacts.each do |person, data|
  data.each do |attribute, value|
    if value.class == Array && value.include?("strawberry")
      value.delete_if {|flavor| flavor == "strawberry"}
    end
  end
end

~~~validation

assert_equal(response, {"Jon Snow": {name: "Jon", email: "jon_snow@thewall.we", favorite_icecream_flavors: ["chocolate", "vanilla", "mint chip"]}, "Freddy": {name: "Freddy", email: "freddy@mercury.com", favorite_icecream_flavors: ["cookie dough", "mint chip"]}})

~~~

%%%

## Higher Level Hash Methods

In a previous lab, you were asked to iterate over a hash and collect the key that pointed to the lowest value. We asked you not to use some of the higher level hash methods there. Now, we're going to learn a few tricks that make a task like that much easier. 

### `.values`

You can collect all of the values in a hash with the `.values` method: 

```ruby
family_members = {mom: "Victoria", dad: "Richard", sister: "Zoe"}

family_members.values
#  => ["Victoria", "Richard", "Zoe"]
```

We can see that the `.values` method returns an array of the values of the keys in the hash. 

### `.keys`

This method, not surprisingly, returns an array of the keys in the hash that you call it on: 

```ruby
family_members = {mom: "Victoria", dad: "Richard", sister: "Zoe"}

family_members.keys
#  => [:mom, :dad, :sister]
```

### `.min`

You can use the `.min` method on a hash to return the key/value pair that contains that lowest value: 

```ruby
food_items = {apples: 45, pears: 12}

food_items.min
#  => [:apples, 45] 
```

These are only a few of the many helpful methods out there. Be there to check out the [Ruby Docs on Hashes](http://ruby-doc.org/core-2.2.0/Hash.html) to learn more. 

Let's practice before you move on to the next lab: 

%%% 

### Code Challenge II: Manipulating Nested Hashes

Below we have a nested hash of grocery items. Use the `.values` method to collect all of the values of the grocery type keys (:dairy, :meat, etc). The end return should be *one-dimensional* array of groceries that *only includes* actual foods ("milk", "carrots"). 

**Hint:** What happens when you call `.values` on a nested hash? What is the return value? How can you *flatten* an array of arrays? Try out your code in IRB to help you solve this one. 

~~~ruby

groceries = {
   dairy: ["milk", "yogurt", "cheese"],
   vegetable: ["carrots", "broccoli", "cucumbers"],
   meat: ["chicken", "steak", "salmon"],
   grains: ["rice", "pasta"]
}

#code you solution here

~~~solution 

groceries = {
   dairy: ["milk", "yogurt", "cheese"],
   vegetable: ["carrots", "broccoli", "cucumbers"],
   meat: ["chicken", "steak", "salmon"],
   grains: ["rice", "pasta"]
}

groceries.values.flatten

~~~validation

assert_equal(response, ["milk", "yogurt", "cheese", "carrots", "broccoli", "cucumbers", "chicken", "steak", "salmon", "rice", "pasta"])

~~~

%%%

%%%

### This is the Quiz Title - It is required.

Here, you can write any necessary directions for the repl.  It will be parsed as markdown, and even accepts codeblocks!

Write an array containing three strings, each saying "taylors gonna tay".


The space delineated by the tildes below is used to set any initial values for the repl, and to declare the repl langauge.  The second tilde demarcated section is for the solution.  It contains the content you would like displayed when a user clicks "See Solution".  The final tilde demarcated section is for validations in the style of the Chai.js assertion library - http://chaijs.com/guide/styles/
The `assert` and `expect` methods are both supported.  `should` is NOT supported.

~~~javascript

// Code your solution here

~~~solution

['taylors gonna tay', 'taylors gonna tay', 'taylors gonna tay']

~~~validation

// Validation go in this block - VALIDATIONS MAY CONTAIN DOUBLE QUOTES ONLY -- NO SINGLE QUOTES!!

assert.equal(response.length, 3);
expect(response).to.be.a("array");

~~~

%%%

## Resources: 

* [Ruby Docs on Hashes](http://ruby-doc.org/core-2.2.0/Hash.html)
