---
layout: post
title: "Lessons from the prime factorization dojo"
date: 2012-09-29 08:38
comments: true
categories: Ruby, Coding Dojo
---
Last week some of my colleagues at Gutefrage.net and me had our second
Coding Dojo. I learned a important lessons - even though I prepared the
Dojo and thought I mastered the kata.

## The kata

The task was to create a class that decomposes natural numbers into its
[prime factors][1].

Additionally the class was supposed to sort the factors ascending.

A few examples:

    1  => [] # Since 1 is no prime ;)
    5  => [5]
    10 =>       [2,5]
    75 => [3,5,5]
    
You get the idea ;)

This sounds relatively simple - and like a perfect candidate for a
introduction to TDD right?

It looks so - but this kata is a bit tricky.

## Performing the kata

The first few tests are straightforward:

{% codeblock prime_factor_decomposer.rb %}
module PrimeFactorDecomposer 
  def decompose(number)
    if number < 4
      number
    else
      [2,2]
    end
  end
end
{% endcodeblock %}

{% codeblock prime_factor_decomposer_spec.rb %}
describe PrimeFactorDecomposer do
    include PrimeFactorDecomposer

     it "returns no prime factors for 1" do
    decompose(1).should be_empty
  end

  it "decomposes 2 into 2" do
    decompose(2).should be == [2]
  end

  it "decomposes 3 into 3" do
    decompose(3).should be == [3]
  end

  it "decomposes 4 into 2,2" do
    decompose(4).should be == [2,2]
  end
end
{% endcodeblock %}

## Now the trouble begins

Now decomposing 5 would be a nice test? Right? Since 5 is a prime it
would only return 5. But how can we find out if its a prime?

So we could write a function that finds out if 5 is prime. Hm that seems
hard.

What about 6? But then we have to develop a algorithm that works -
otherwise we would just move sidewards and add conditionals.

## Whats the problem?

You do not know how to solve the problem. Not everything is as simple as
the [FizzBuzz][2] Kata.

Adding another test alone does not suffice. Its a common hurdle for
newcomers to TDD. 

## How to solve this?

Of course the solution might be to think about the algorithm. Maybe on a
sheet of paper. Or do a little spike until you fully understand the
problem.

Then come back and continue. Sometimes its just not true, that a design
"magically" emerges.

## Learnings

You need a sound knowledge about where to go - otherwise you will have
problems with TDD along the way. Then you have to lean back and think. 

TDD is cool, but it can not solve every problem. Especially not
complicated algorithms (and this is a really easy one of course).

Another personal learning for me was to watch myself better, when I do a
kata the first time.

When the design does not emerge from the tests - I should listen to
them. Maybe they want to tell me that I have absolutely no idea what I
am doing and that I need to think ;) 

[1]: http://en.wikipedia.org/wiki/Prime_factor
[2]: http://www.codingdojo.org/cgi-bin/wiki.pl?KataFizzBuzz
