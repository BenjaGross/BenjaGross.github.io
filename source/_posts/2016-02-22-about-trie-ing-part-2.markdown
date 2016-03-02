---
layout: post
title: "About Trie-ing (part 2)"
date: 2016-02-22 14:36:01 -0500
comments: true
categories: 
---
##Implementing a Trie in Ruby

In my last post I talked about benefits, reasons, and potential negatives about using a trie, a data type that is great to use for matching string patterns. For this post I will talk about how to implement a trie in Ruby.

#### Setting Up
The first thing I want to do when I'm going to build my trie is set up a `Trie` class that will initialize with a hash containing the type of nested hash structure I want my trie to have. Since this is the actual data structure of my trie I will call this instance variable `tree` 

```ruby
class Trie
   
   attr_accessor :tree

   def initialize 
      self.tree = Hash.new{ |h,k| h[k]=Hash.new(&h.default_proc) }
   end
  #...
end
```

The above code will initialze new instances of `Trie` with a default hash called `tree`. We build our nested hash above so that when we try to access a key in our hash, if the key is present we will get the value of that key, but if the key is not present then a new key-value pair will be added to the hash. When a new pair is added, the key will be the value we tried to access and the value will be an empty hash. This would normally only work on the first level of our hash, but since we want it to work no matter how deep our hashes get nested we can call [default_proc](http://ruby-doc.org/core-1.9.3/Hash.html#method-i-default_proc) (which returns any block passed to `Hash.new` for a specific instance of `Hash`) as a proc on each new hash we create on access so that each new hash created on access is created with the same functionality.

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

#### Inserting Strings

Since we now have a nested hash functioning like we want the next step is to build a method that can add a string to our `tree`. I'll call this method `#insert` and it must take a string like `"cat"` and call `tree["c"]["a"]["t"]` just like above. 

```ruby
def insert(word)
  characters = word.split("")         
  pointer = self.tree
  characters[0..-2].each{ |char| pointer = pointer[char] }
  pointer = pointer[characters.last][true]
end
```

The first thing we do after isolating the characters in the argument string is to set up a variable `pointer`. `pointer` starts off set to the current state of `self.tree` (starting with `{}`). Then in the next line we loop through all but the last character in our characters array. For each character we set our pointer to the return value when we try to access that character of our pointer. If that character is in our `tree` in that position, the pointer will move to the the value of that key and if that character is not present at that level of the tree then pointer will be moved to the empty array that is created and we can move on to the next value. 

After all but the last characters are added we add the last character followed by a bool `true`. We will later use this `true` to signify that this is the end of a word in our `trie`

#### Finding Matches

Once we add all the words to `trie` we need check what words have been added so we can build a method called `match` which will take a string and return `true` or `false` depending on weather the word is in our trie.

```ruby
def match(word)
  characters = word.split("")           
  pointer = self.tree
  characters.each { |char| pointer = pointer.fetch(char, nil) rescue nil }
  result = pointer.fetch(true, nil) rescue nil
  result == {} ? true : false 
end
```

`match` works similarly `insert` but in reverse. First we split the argument into characters (like in `insert`), then we set a variable called `pointer` to the current state of our tree (also like in `insert`), and then we loop through all the characters setting `pointer` to the return value of calling `fetch` on `pointer` for each character. We also `rescue nil` for this line and the next so that we will not get errors in instances where characters are not found in `tree`.

We use `fetch` here so we can set a default return of `nil` instead of getting an error like we would had the key had not been in the hash, or creating a new empty hash which would have happened if we used the `tree["c"]` syntax and there was no key. After the loop terminates `pionter` is now pointing to `tree.fetch(characters.last, nil)` so we call `fetch(true, nil)` on the last character to make sure that the word ends at that point in the trie. If it does we get `{}` and if not we get `nil`. We then compare our result to `{}` and return the proper `true`/`false` value. 

####Next Steps
After building this simple Trie implemenation in Ruby I want to end by listing a few next steps for this code.

* Partial match support - Partial match support would be a very useful tool for a trie. A method like `partial_match(substring)` that could return all words in the trie containing the provided substring would be useful for finding similarity instead of indenticlal matches
* Wildcard Support - A method that could take a string containing designated wildcard characters and return true or false for matches would be useful for finding pattern segments
* Initialize with strings - Creating a trie that could initialize with an array of strings would make adding strings and could be accomplished with an `add_multiple_strings(array)` method that leveradges the `*` operator or just takes an array argument. 
* Toggle options - Toggling case sensitivity
* Javascript - Implementing a trie in Javascript would be very useful and could be used as a look up for custom predictive text in searches and many other features. 
