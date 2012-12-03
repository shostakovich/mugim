---
layout: post
title: "Set.rb - some random facts about sets in Ruby."
date: 2012-10-13 18:06
comments: true
categories: Ruby, Code, Reading
---

I like to read code. Often I take notes and it's a shame that I did never share them. But it's never to late to start.

So here are some notes on Ruby's set.rb. I hope you in turn share an interesting read with the world ;)

## How to initialize a set	

{% codeblock initialize.rb %}
def initialize(enum = nil, &block)
  @hash ||= Hash.new

  enum.nil? and return

  if block
    do_with_enum(enum) { |o| add(block[o]) }
  else
    merge(enum)
  end
end
{% endcodeblock %}	

    
Ok much to notice here: 

 * A Set uses a hash internally to avoid duplicates
 * Per default the merge method will just add() to the hash
 * Interestingly enough you can pass a block to prepare the data 
 
Some examples what this means:

	Set.new
	 => #<Set: {}> 
	 
	 Set.new([1,2,2,3])
	 => #<Set: {1, 2, 3}> 
	 
	 Set.new(["ape", "APE", "aPe"]) { |val| val.downcase  }
	 #<Set: {"ape"}> 
	 
A second possibility is hidden at the bottom of the file:

{% codeblock enumerable.rb %}
module Enumerable
  def to_set(klass = Set, *args, &block)
    klass.new(self, *args, &block)
  end
end
{% endcodeblock %}

    
 Some examples:
 
    %w{hello, hello, you}.to_set
    => #<Set: {"hello,", "you"}>
 	
	 %w{hello, hello, world, "!"}.to_set {|val| val.upcase }
	 => #<Set: {"HELLO,", "WORLD,", "\"!\""}> 
    
	 
Here is a third way:

{% codeblock self.rb %}
def self.[](*ary)
  new(ary)
end
{% endcodeblock %}


This is pretty nice trick. It produces a nice shortcut to create a set.
	
	Set[1,2,3,4]
	#<Set: {1, 2, 3, 4}> 	 
 	
## Set arithmetic in set.rb

A  nice feature of sets is, that they support set arithmetic. The implementation is pretty straightforward, this is one of the instances where Ruby's syntax really shines :)

{% codeblock aritmethics.rb %}
  # given enumerable object.
  def |(enum)
    dup.merge(enum)
  end
  alias + |             ##
  alias union |         ##

  # Returns a new set built by duplicating the set, removing every
  # element that appears in the given enumerable object.
  def -(enum)
    dup.subtract(enum)
  end
  alias difference -    ##

  # Returns a new set containing elements common to the set and the
  # given enumerable object.
  def &(enum)
    n = self.class.new
    do_with_enum(enum) { |o| n.add(o) if include?(o) }
    n
  end
  alias intersection &  ##

  # Returns a new set containing elements exclusive between the set
  # and the given enumerable object.  (set ^ enum) is equivalent to
  # ((set | enum) - (set & enum)).
  def ^(enum)
    n = Set.new(enum)
    each { |o| if n.include?(o) then n.delete(o) else n.add(o) end }
    n
  end
{% endcodeblock %}
	 
Some examples:

### Merging
	
	a = Set["a", "b", "c"]
 	=> #<Set: {"a", "b", "c"}>
 	 
	b = Set["d", "e"]
	 => #<Set: {"d", "e"}> 
	
	a | b
 	=> <Set: {"a", "b", "c", "d", "e"}> 
 	
### Substracting another enumerable

	a = Set[1, 2, 3]
	=> #<Set: {1, 2, 3}>
	
	a - [3]
	=> #<Set: {1, 3}> 
	
### Intersection between a set and an array

	a = Set[2,3,4]
	 => #<Set: {2, 3, 4}>
	 
	 b & [1,2,3]
	 => #<Set: {2, 3}>
	 
I think you get the idea. For some problems, this kind of arithmethic is ideal. 

## Set is just another enum

Of course it might be obvious to you, if you are not new to Ruby :) Set is just another enumerable. It is interchangable with an array for example.

Another nice trait of course is, that you have all the nice methods that come with enumerables at your disposal, wen working with a set.

I highly recommend you reading the [enumerable documentation](http://ruby-doc.org/core-1.9.3/Enumerable.html), since this is something out of scope for this little article.

## Random stuff about set.rb

* A SortedSet exists as well
* There is a RestrictedSet, that is commented - wtf?
* There are many tests for set within the same file. So scroll down for more examples

## Bottom line

Sets are awesome. Sometimes it makes sense to write this kind of arithmetical functions.

This class is really big, but it still makes sense this way. The functions are very small and focused and its quite simple to read.

Set uses some nice tricks. I really love the ***def self.[]*** trick for example.

One of the biggest suprises for me was, that you can pass a block to the constructor. I'm not sure if I am a fan of that.

I loved the implementation of set arithmetics - its simple and beautiful.

Reading this file was fun, you [should read it as well](https://github.com/ruby/ruby/blob/ruby_1_9_3/lib/set.rb
)!