---
layout: post
title: "About Ruby Proxy Class"
date: 2015-03-05 14:12:04 -0500
comments: true
categories: meta-programming, ruby, class, object
---
It's not so amazing how life always seems to keep just slightly away from writing, but not recently! So here is a post about Proxy Classes in Ruby.

I came accross the topic while going through the [Ruby Koans](http://rubykoans.com/) this morning and since I had never heard of this pattern before I thought, "Let's get writing!"

###What is a Proxy Class?
Proxy Class is a code pattern in Ruby and is simply using a class to interface with something else(It is completely important to note that this is different from [Java Interfaces](http://docs.oracle.com/javase/tutorial/java/concepts/interface.html) which I will not talk about here). In Ruby, it helps me to think of the proxy class pattern as a technique to create wrappers for other classes that can add new functionality and/or change normal functionality.

###Why Use This Pattern?
If you read the above description, and thought, "Sure" then you can just skip to the examples. If not, you probably want to know why you might do this. You can use this pattern to add your own functionality to a class you created to give it different functions in certain circumstances or more likely to change a standard Ruby class, like array, without [monkey patching](http://en.wikipedia.org/wiki/Monkey_patch) the class and creating potential problems with your code. If you are looking for examples you you don't need to get very obscure. [Active Record](https://github.com/rails/rails/tree/master/activerecord) use this pattern to design their associations! 

If you would like a more simple example however In the below code we set up the a Proxy Class for an array of names. 

```ruby
class NamesProxyArray
  def initialize(arr)
    @arr = arr
  end

  private
    def method_missing(method, *args, &block)
      @arr.send(method, *args, &block)
    end
end

special_array = NamesProxyArray.new(%w[Alex Birtha Christopher Mark Thomas Sally Jessica Steve])

special_array.size #=> 8
special_array.shuffle #=> ["Thomas", "Christopher", "Steve", "Sally", "Jessica", "Alex", "Birtha", "Mark"] 
``` 

Above, we recreate the functionality of Ruby Array by redefining method_missing. We use the `send` method to take any method unknown to the NamesProxyArray in this case `size` and `shuffle` and call them on the array object that is in this instance of Proxy Class we made. This is the cornerstone for setting up our proxy. After hijacking into method_missing, any method that we don't define or that is not part of the default Ruby Class Object will be called on Array instead of NamesProxyArray.

Now that we have access to the regular Array methods we can start to add new functionality. In the below code we will redefine the shuffle method and add a new method called get_first_letter. 

```ruby
class NamesProxyArray
  def initialize(arr)
    @arr = arr
  end

  def shuffle
    @arr.map do |item| 
      item.chars.shuffle.join
    end
  end

  def get_first_letter
    @arr.map do |item|
      item.chars[0]
    end
  end

  private
    def method_missing(method, *args, &block)
      @arr.send(method, *args, &block)
    end
end

special_array = NamesProxyArray.new(%w[Alex Birtha Christopher Mark Thomas Sally Jessica Steve])

special_array.size #=> 8
special_array.shuffle #=> ["Axel", "hiartB", "iCsrtrhpoeh", "Mrka", "osmaTh", "lylaS", "eJssaci", "etvSe"]
special_array.get_first_letter #=> ["A", "B", "C", "M", "T", "S", "J", "S"]
``` 
Now we have two new methods! A redefined shuffle and a new `get_first_letter`. By defining shuffle in NamesProxyArray `method_missing` is not longer called and the method is evaluated in our Proxy Class instance and we don't have to worry about breaking `shuffle ` any where else in our code. `get_first_letter` works like any other instance method and any standard array method like `size`, as long as they are undefined in ProxyClass still get evaluated on the regualar Array class. All this happens without having to modify the class Array at all!

Ruby's Proxy Class pattern can be used to really make your code your own. You can modify any of the standard ruby objects to work your way without actually changing them! Additionally it helped me to get more comfortable using patterns in my day to day code. 