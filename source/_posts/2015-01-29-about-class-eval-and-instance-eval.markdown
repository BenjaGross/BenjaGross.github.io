---
layout: post
title: "About class_eval and instance_eval"
date: 2015-01-29 11:40:19 -0500
comments: true
categories: "metaprogramming ruby class_eval instance_eval eval"
---
This afternoon I took the time to reread my old blog...

I then took less time to delete the whole thing and restart it since if something is worth doing it is worth doing well.

For the first post in my new blog I will write about the Ruby methods: class_eval and instance_eval. I can think of no way more metaphorically flavorful of getting into a good headspace to write!

These methods are meta and a bit odd so to start, in plain English, class_eval lets a programmer evaluate code within the scope of a class and instance_eval lets a programmer evaluate code on a class object. 

Here is how they look:
###class_eval
```ruby
class Meal
end

breakfast = Meal.new
breakfast.eat_pancakes # NoMethodError

Meal.class_eval do 
  def eat_pancakes
    "Mmmmm Pancakes!"
  end
end

breakfast.eat_pancakes # "Mmmmm Pancakes!"
```
The first time we call `breakfast.eat_pancakes` we get a NoMethodError. `class Meal` is empty and this is what we expect. The second time it is called after we use class_eval to pass a block to the meal class defining the eat_pancakes method. We then get a return value of "Mmmmm Pancakes!". We get this without changing the original code for the Meal class. We can use class_eval to add the an instance method to a class at any point in a program runtime!

###instance_eval
```ruby
class Meal
  def eat_pancakes
    "Mmmmm Pancakes!"
  end
end

Meal.instance_eval do 
  def breakfast?
    true
  end
end

Meal.breakfast? # true
```
In the above code `Meal.breakfast?` returns true after we use instance_eval to add the appropriate class method. This is where things get confusing. In the above example of class_eval we added the instance method `eat_pancakes` and in the example for instance_eval we added the class method `breakfast?`. So why are they not named the other way around?

* class_eval is a method from [Ruby's Module Class](http://www.ruby-doc.org/core-2.2.0/Module.html) and thus acts on a module or class and evaluates the block it is pased in the scope of that class. Thus when we define a method in that scope with `def eat_pancakes` we create an instance method the same way we would had it been within the original class syntax. 
* instance_eval however is a method from [Ruby's Object Class](http://ruby-doc.org/core-2.2.0/Object.html) and the block we pass to it is evaluated on the class object. Then when we write `def breakfast?` in the block it is evaluated on the class object and runs as a class method rather than in the class as an instance method!

So you may look at these methods and think, "Cool! But who cares?" and that's probably fine, I only used instance_eval once and it was to change an initialize method to give class instances a different name depending on how they were created. While I quickly refactore this away I still think it was a fun way to solve the problem. 

Moving forward will you use this method? If you are really into metaprogramming then yes, you probably will, but if not? Who cares, they fun methods to play with and can help you think about the ruby object model in a new way.