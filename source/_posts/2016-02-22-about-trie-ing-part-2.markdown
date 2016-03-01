---
layout: post
title: "About Trie-ing (part 2)"
date: 2016-02-22 14:36:01 -0500
comments: true
categories: 
---
In my last post I talked about benefits, reasons, and potential negatives about using a trie, a data type that is great to use for matching string patterns. For this post I will talk about how to implement a trie in Ruby.

The first thing I want to do when I'm going to build my trie is set up a `Trie` class that will initialize with a hash containing the nested hash structure I want my trie to have. Since this will be what contains my my strings I will call it `tree`. 

```ruby
class Trie
   
   attr_accessor :tree

   def initialize 
      self.tree = Hash.new{|h,k| h[k]=Hash.new(&h.default_proc) }
   end
  #...
end
```

The above code will initialze new instances of `Trie` with a default hash called `tree`. The line of code that we use above to build our nested hash can be a little confusing so see this explination:

`Hash.new{|h,k| h[k]=Hash.new(&h.default_proc) }` will create a hash that initializes a new hash as a value in place when we try to access any uninitialized keys in our `tree`. This gives us a new level of hash in our tree structure instead of returning `nil` like it usually would. The most confusing part of this syntax for me is the `&h.default_proc` that we pass as a parameter to `Hash.new`. What this does is pass the default block parameter for our hash to any new hash created when we try to access uninitialized keys in our `tree` essentially calling `Hash.new{|h,k| h[k]=Hash.new}` for every hash that is generated on access in the `tree`. 

The way this works is important for building the tree like strucutre in our trie and will be used extensively when adding strings to our trie. It works like below:

```ruby
tree = Hash.new{|h,k| h[k]=Hash.new(&h.default_proc) }
tree[:a] = "ABBA" #=>"ABBA"
tree[:a]          #=> "ABBA"
tree[:b]          #=> {}
tree              #=> {:a=>"ABBA", :b=>{}} 
tree[:b][:b1]     #=> {}
tree              #=> {:a=>"ABBA", :b=>{:b1=>{}}} 
```