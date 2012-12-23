---
layout: post
title: "Reading prime.rb - The build in support for prime numbers in Ruby"
date: 2012-10-20 09:16
comments: true
categories:
  - ruby
  - primes
  - read
  - code
---

In one of our coding katas one of the pairs asked the question how to
find out, if a number is prime. I knew a simple algorithm, because I
solved the first few challenges on [Project
Euler](http://projecteuler.net/) - but I did not know that Ruby has
prime support out of the box.

This week I want to share my notes on my read of
[prime.rb](https://github.com/ruby/ruby/blob/trunk/lib/prime.rb). Part
of Ruby 1.9.3 ..

## Some tweaks to Integer

{% codeblock integer.rb %}
class Integer
  def Integer.from_prime_division(pd)
    Prime.int_from_prime_division(pd)
  end

  def prime_division(generator = Prime::Generator23.new)
    Prime.prime_division(self, generator)
  end

  def prime?
    Prime.prime?(self)
  end

  def Integer.each_prime(ubound, &block) # :yields: prime
    Prime.each(ubound, &block)
  end
end
{% endcodeblock %}

If you require 'prime' you get some extra functions on integers.

You can check if a number is prime, can generate all prime numbers up to
a certain upper bound or execute a prime factor decomposition.

### Examples

21.prime? #=> false

100.prime_division # => [[2, 2], [5, 2]]

Integer.each_prime(20) { |primee| puts prime } # Prints all primes up to
20
 
    Integer.from_prime_division(21.prime_division) # Reverses a
prime_division
    
The prime devision returns nice pairs - the prime and how often its a
divisor of the number.

For example: 4 is 2 * 2 and would return [[2,2]], wheras 12 is 2 * 2 * 3
and returns [[2, 2], [3, 1]]

## The generators

Prime.rb contains several different generators for prime numbers. 

They inherit from the same abstract base class.

{% codeblock pseude_prime_generator.rb %}
class PseudoPrimeGenerator
  include Enumerable

  def initialize(ubound = nil)
    @ubound = ubound
  end

  def upper_bound=(ubound)
    @ubound = ubound
  end
  def upper_bound
    @ubound
  end

  def succ
    raise NotImplementedError, "need to define `succ'"
  end

  def next
    raise NotImplementedError, "need to define `next'"
  end
  
  def rewind
  raise NotImplementedError, "need to define `rewind'"
  end

  def each(&block)
    return self.dup unless block
    if @ubound
      last_value = nil
      loop do
        prime = succ
        break last_value if prime > @ubound
        last_value = block.call(prime)
      end
    else
      loop do
        block.call(succ)
      end
    end
  end

  alias with_index each_with_index

  def with_object(obj)
    return enum_for(:with_object) unless block_given?
    each do |prime|
      yield prime, obj
    end
  end
end
{% endcodeblock %}

Every generator has an upper bound and has to be able to supply the next
prime number or start over from scratch. 

Intersting is also the each method, that stops executing if the upper
bound is reached. Its kind of obvious after you have read it - but it
never occured to me, that you can (or might want to) modify each in this
way if necessary - but for primes it makes sense of course, unless you
are really patient..

There are 3 generators that are implemented in prime.rb:

### EratosthenesGenerator

{% codeblock eratosthenes_generator.rb %}
class EratosthenesGenerator < PseudoPrimeGenerator
  def initialize
    @last_prime = nil
    super
  end

  def succ
    @last_prime = @last_prime ? EratosthenesSieve.instance.next_to(@last_prime) : 2
  end
    
  def rewind
    initialize
  end
  alias next succ
end
{% endcodeblock %}

This generator is based on the [Sieve of
Eratosthenes](http://en.wikipedia.org/wiki/Sieve_of_Eratosthenes). 

If the first prime number is found you start to cross out all its
multiples and repeat this process for the next prime etc. (see below).
Of course this only works with an upper bound - otherwise you would
cross out numbers for a very long time..

{% img /images/uploads/sieve_of_eratosthenes_animation.gif %}
Picture created by Skoop (License: [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/))

Sieving numbers is faster than pure brute-forcing but you have to keep track of the number that are not prime, which is memory-intensive.

This generator is also the default generator in prime.rb, if you want to use Integer.each_prime() to iterate over all primes.

### Trial Division

{% codeblock trial_division_generator.rb %}
 class TrialDivisionGenerator<PseudoPrimeGenerator
   def initialize
     @index = -1
     super
   end

   def succ
     TrialDivision.instance[@index += 1]
   end
   def rewind
     initialize
   end
   alias next succ
 end
{% endcodeblock %}

The TrialDivisionGenerator is a brute force generator. It divides the integer through all integers smaller to its square root. The TrialDivision class itself has some more tricks in it, to make it faster - but thats pretty much it.

Interesting is how its used. Have a close look at the succ function. It calls the trial divsion like this:

	TrialDivision.instance[prime_number]
    	
TrialDivision calcuates all prime numbesr up to this index, saves them in its @primes variable and then returns the result.

{% codeblock trial_division.rb %}
def [](index)
  while index >= @primes.length
    #calculate the number and push it to primes
  end
  return @primes[index]
end
{% endcodeblock %}
	
To be honest I was quite supprised - and I am not sure, if I like that syntax. Its not really clear what this will do from the outside.

Another aspect is that the next time you iterate over a set of primes they are cached already. On the other hand this means that the highest prime you can calculate is limited by the amount of your RAM and not of the time you invest to brute-force. (I guess that is no big concern in most cases, since brute forcing is so slow after a few minutes, that it makes no big difference)

The TrialDivision generator is not used in the rest of the file.

### Generator 23

This is should be the fastest generator of them all - it also does not consume memory. But - its a fake.. You can try to figure out why: If you can't you can read the original file - it's commented in there of course ;)

{% codeblock generator_23.rb %}
class Generator23 < PseudoPrimeGenerator
  def initialize
    @prime = 1
    @step = nil
    super
  end

  def succ
    loop do
      if (@step)
        @prime += @step
        @step = 6 - @step
      else
        case @prime
          when 1; @prime = 2
          when 2; @prime = 3
          when 3; @prime = 5; @step = 2
        end
      end
      return @prime
    end
  end
  alias next succ
  def rewind
    initialize
  end
end
{% endcodeblock %}

This generator is used to do the prime division. And to check if a number is prime. Its just a speed improvement - you will see why in a second.

## Putting it all together

{% codeblock prime.rb %}
class Prime
  include Enumerable
  @the_instance = Prime.new

  def initialize
    @generator = EratosthenesGenerator.new
    warn "Prime::new is obsolete. use Prime::instance or class methods of Prime."
  end

  class << self
    extend Forwardable
    include Enumerable
    # Returns the default instance of Prime.
    def instance; @the_instance end

    def method_added(method) # :nodoc:
      (class<< self;self;end).def_delegator :instance, method
    end
  end

  def each(ubound = nil, generator = EratosthenesGenerator.new, &block)
    generator.upper_bound = ubound
    generator.each(&block)
  end

  def prime?(value, generator = Prime::Generator23.new)
    value = -value if value < 0
    return false if value < 2
    for num in generator
      q,r = value.divmod num
      return true if q < num
      return false if r == 0
    end
  end

  def int_from_prime_division(pd)
    pd.inject(1){|value, (prime, index)|
      value *= prime**index
    }
  end

  def prime_division(value, generator= Prime::Generator23.new)
    raise ZeroDivisionError if value == 0
    if value < 0
      value = -value
      pv = [[-1, 1]]
    else
      pv = []
    end
    for prime in generator
      count = 0
      while (value1, mod = value.divmod(prime)
             mod) == 0
        value = value1
        count += 1
      end
      if count != 0
        pv.push [prime, count]
      end
      break if value1 <= prime
    end
    if value > 1
      pv.push [value, 1]
    end
    return pv
  end
end
{% endcodeblock %}

The Prime class puts this all together. As you can see the each() method employs the EratosthenesGenerator - it has to delive proper primes and sieving is faster, then mere brute force.

The prime? method on the other hand, uses Generator23 - because it does trial division from the smallest number up. Even if some of the numbers the Generator are no primes, the result will not change - in fact you could even use all natural numbers..

The prime_division function also uses trial division from the lowest generated number up. This way a bigger number will not divide without rest again - unless its a real prime and a prime factor.

So using Generator23 - that only returns numbers that are not divisible by 2 or 3 - is a speed improvement.

## Bottom line

Some questions remain after reading this file:

* Why is there a TrialDivision generator, that is not really in use?
* Do tests exist?

I like how this file reads. It has functions that are quite short and focused. It's really awesome that Ruby has some built-in tools for playing around with prime numbers.

This file seems to be a good foundation, to build other prime number generators upon. For example the [Sieve of Atkins](http://en.wikipedia.org/wiki/Sieve_of_Atkin) comes to mind (even to me as a prime number noob ;)..

As always: I invite you to [read the code](https://github.com/ruby/ruby/blob/trunk/lib/prime.rb) - especially if you are interested in prime numbers.
