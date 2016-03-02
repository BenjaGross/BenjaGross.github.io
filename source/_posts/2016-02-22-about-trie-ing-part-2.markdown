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
      self.tree = Hash.new{ |h,k| h[k]=Hash.new(&h.default_proc) }
   end
  #...
end
```

The above code will initialze new instances of `Trie` with a default hash called `tree`. We build our nested hash above so that when we try to access a key in our hash, if the key is present we will get the value of that key, but if the key is not present then a new key-value pair will be added to the hash. When a new pair is added, the key will be the value we tried to access and the value will be an empty hash. This would normally only work on the first level of our hash, but since we want it to work no matter how deep our hashes get nested we can call [#default_proc](http://ruby-doc.org/core-1.9.3/Hash.html#method-i-default_proc) (which returns any block passed to `Hash.new` for a specific instance of `Hash`) as a proc on each new hash we create on access so that we keep getting new hashes all the way down. 

Now, when we add characters to our custom nested hash in a specific way we will get a trie-like structure with each key pointing to a value that is a hash of the next possible characters. Below is an example for the strings "cat" and "car".  

```ruby
tree = Hash.new{ |h,k| h[k]=Hash.new(&h.default_proc) }

tree["c"]             #=> {}
tree                  #=> {"c"=>{}}
tree["c"]["a"]        #=> {} 
tree                  #=> {"c"=>{"a"=>{}}}
tree["c"]["a"]["t"]   #=> {}
tree                  #=> {"c"=>{"a"=>{"t"=>{}}}} 
tree["c"]["a"]["r"]   #=> {}
tree                  #=> {"c"=>{"a"=>{"t"=>{}, "r"=>{}}}} 
```
Since we now have a nested hash functioning like we want the next step is to build a method that can add a string to our `Trie`. I'll call this method `#insert` and it must take a string like `"cat"` and call `tree["c"]["a"]["t"]` just like above. 