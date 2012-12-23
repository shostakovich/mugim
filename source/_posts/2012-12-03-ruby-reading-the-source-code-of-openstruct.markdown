---
layout: post
title: "Ruby: Reading the source code of OpenStruct."
date: 2012-12-01 08:16
comments: true
categories:
  - ruby
  - read

---

***Ruby version: 1.9.3-p327***

Here is another read of a Ruby core class: OpenStruct. 

![Open Struct](http://farm3.staticflickr.com/2780/5786753272_04ed149fa4_z.jpg "Openstruct")

## Initializing an OpenStruct

You can create an OpenStuct using hash - or without any attributes.

{% codeblock .initialize.rb %}
def initialize(hash=nil)
  @table = {}
  if hash
    hash.each_pair do |k, v|
    k = k.to_sym
    @table[k] = v
    new_ostruct_member(k)
  end
 end
{% endcodeblock %}

If you use a hash openstruct will go ahead and store all the key/value pairs in an internal hash called table.

{% codeblock .new_ostruct_member.rb %}
def new_ostruct_member(name)
  name = name.to_sym
  unless respond_to?(name)
    define_singleton_method(name) { @table[name] }
    define_singleton_method("#{name}=") { |x| modifiable[name] = x }
  end
  name
end
protected :new_ostruct_member
{% endcodeblock %}

It will also create getters / setters using [define_singleton_method](http://apidock.com/ruby/Object/define_singleton_method).

    require 'ostruct'
    coordinate = OpenStruct.new(:x => 1, :y => 2)
    #<OpenStruct x=1, y=2>
    	
	OpenStruct.new
	#<OpenStruct>
	
## Modifiable

But wait.. What does this modifiable thing in the dynamic setter do? Lets have a look at the code.

{% codeblock .modifiable.rb %}
def modifiable
  begin
    @modifiable = true
   rescue
     raise TypeError, "can't modify frozen #{self.class}", caller(3)
   end
   @table
end
protected :modifiable
{% endcodeblock %}

Huh? It sets a attribute modifiable and if it does not succeed it throws an exception? If it is modifiable it returns the @table?

Well to be honest this is not clear at all. The text of the exception states that this is about frozen objects. Hm lets validate this:

	foo = OpenStruct.new
 	#<OpenStruct> 
	foo.send(:modifiable)
	# {} 
	foo.freeze
	# nil 
	foo.send(:modifiable)
  # TypeError: can't modify frozen OpenStruct

Ah! - It acts as kind of a gatekeeper to the table, by preventing you from changing the OpenStruct if it's frozen. I'm not sure if I like this - it not to clear to me - on the other hand it is tell don't ask. I guess I have to think about it.  
  
## Method missing

There is another feature of OpenStruct. It has setters for attributes that do not yet exist.

    foo = OpenStruct.new
    #<OpenStruct> 
    foo.x = 1 
    foo.y = 2
    foo
    #<OpenStruct x=1, y=2>
     
In Ruby you can achieve this using ***method_missing***.

{% codeblock .method_missing.rb %}
def method_missing(mid, *args) # :nodoc:
  mname = mid.id2name
  len = args.length
  if mname.chomp!('=') && mid != :[]=
    if len != 1
      raise ArgumentError, "wrong number of arguments (#{len} for 1)", caller(1)
    end
    modifiable[new_ostruct_member(mname)] = args[0]
  elsif len == 0 && mid != :[]
    @table[mid]
  else
    raise NoMethodError, "undefined method `#{mid}' for #{self}", caller(1)
  end
end
{% endcodeblock %}

Hm… Wow there is some stuff I have never seen before. Lets try to read it line by line.

***mid.id2name*** converts the method's id (which is a symbol). Into a string.

If the method name has a ***=*** and only one argument was used, then a new entry in the table is created using ***new_ostruct_member*** after that the value if assigned.

The elseif block handles reads on attributes that were not defined. Getter method have no arguments of course.

	foo = OpenStruct.new
	#<OpenStruct> 
	foo.something_random
	# nil

It looks the ***mid*** up in the ***@table***. I have no idea why it does not return nil in the first place here. The effect is the same, because ach time we add a new attributes ***new_ostruct_member*** creates dynamic methods. So if the attribute is defined, we do not end up in method missing.

For all other functions except:
* These that have an "=" and one argument
* And these that have no argument
* And are not :[], :[]=

It raises an exception. If you wondering what this caller(1) thing is:

>Returns the current execution stack—an array containing strings in the form “file:line” or “file:line: in method’”. The optional start parameter determines the number of initial stack entries to omit from the result.
[Link to the Documentation](http://www.ruby-doc.org/core-1.9.3/Kernel.html#method-i-caller)

## Duplicating an OpenStruct

I had quite a hard time of figuring out what the following function was doing. I tried really hard to call it from the console.. :/

{% codeblock .initialize_copy.rb %}
 def initialize_copy(orig)
    super
    @table = @table.dup
    @table.each_key{|key| new_ostruct_member(key)}
  end
{% endcodeblock %}

It turns out ***initialize_copy*** is used, whenever .clone or .dup are called, to duplicate the object. To get the whole picture read this [great article about initialize_dup, initialize_clone and initialize_copy](http://jonathanleighton.com/articles/2011/initialize_clone-initialize_dup-and-initialize_copy-in-ruby/)!

## Inspect

{% codeblock .inspect.rb %}
InspectKey = :__inspect_key__ # :nodoc:

def inspect
  str = "#<#{self.class}"

  ids = (Thread.current[InspectKey] ||= [])
  if ids.include?(object_id)
    return str << ' ...>'
  end

  ids << object_id
  begin
    first = true
    for k,v in @table
      str << "," unless first
      first = false
      str << " #{k}=#{v.inspect}"
    end
    return str << '>'
  ensure
    ids.pop
  end
end
alias :to_s :inspect
{% endcodeblock %}

Ok. The last part of this is straightforward. Every attribute is printed in like ***lenght=21***. All attributes are separated by a space and a comma. The attribute info is appended to the class name.

	#<OpenStruct name="Brussel Sprouts">
	
What this inspect_key stuff is all about? Have a look at this experiment.

  	puts :__inspect_key__
	  # nil
    Thread.new { puts :__inspect_key__ }
    #<Thread:0x007fe2b487b650 sleep>
    
 You see? Every thread has its own key.
 
    Thread.new do
	    a = OpenStruct.new(:hello => "foo", :world => "bar")
  	  puts a.inspect
	  end
    #<OpenStruct hello="foo", world="bar"> => #<Thread:0x007fe2b4817d80 run> 
 
## Marshalling support

>The marshaling library converts collections of Ruby objects into a byte stream, allowing them to be stored outside the currently active script. This data may subsequently be read and the original objects reconstituted. [Link to the Documentation](http://www.ruby-doc.org/core-1.9.3/Marshal.html)

{% codeblock marshal.rb %}
def marshal_dump
  @table
end

def marshal_load(x)
  @table = x
  @table.each_key{|key| new_ostruct_member(key)}
end

{% endcodeblock %}

I would have called it serialization / deserialization ;) There are 2 function that OpenStruct implements to achieve this.

***marshal_dump** just returns the table - it's already a hash, so Marshal can handle it.

**marshal_load** just assign this same hash and creates all the required method to access the attributes.

Lets have a look at this in action.

	point = OpenStruct.new(:x => 1, :y => 2)
	#<OpenStruct x=1, y=2> 

	serialized_point = Marshal.dump(a)
	"\x04\bU:\x0FOpenStruct{\a:\x06xi\x06:\x06yi\a" 

	Marshal.load(serialized_point)
	#<OpenStruct x=1, y=2> 

## Some changes in trunk

The version in trunk is slightly different from the version disscussed here. It contains the functions ***[]*** and ***[]=***.

{% codeblock .hash_style_accessors.rb %}
 def [](name)
  @table[name.to_sym]
end

def []=(name, value)
  modifiable[new_ostruct_member(name)] = value
end
{% endcodeblock %}

## Last words

OpenStruct is quite nice. It comes in handy when you want to replace a hash with a value object. The addition in trunk will make this even easier in the future because it's a drop in replacement for simple cases and the Tests will stay green ;)

Many people find that the attr_reader part for non-specified attributes is a big problem, when you use OpenStruct. I agree. You have to be aware of this or it will bite you.

It was fun reading this class. I did't like the ***modifiable*** method. It's not really as clear as it could be.

* There's a method called .id2name
* Some random stuff about Threads
* Serialization is called marshalling in Ruby
* A weired way to determine if a object is frozen
* Ways to hook into the dup and clone methods

Go read some code as well! Its a highly educating activity.

Picture of cup licensed under: [(CC BY-NC-SA 2.0)](http://creativecommons.org/licenses/by-nc-sa/2.0/) Licensor: [rebeccaâˆžmahoney](http://www.flickr.com/photos/from_my_d40/)
