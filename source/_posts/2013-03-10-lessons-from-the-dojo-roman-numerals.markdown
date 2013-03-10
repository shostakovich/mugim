---
layout: post
title: "Lessons from the Dojo: Roman Numerals"
date: 2013-03-10 10:41
comments: true
categories: 
  - coding dojo
  - roman numerals
  - ruby
---

A few days ago we had one of our Coding Dojo's at
Gutefrage.net. This time we tried to implement the ["Roman Numerals kata"][1].

I always learn a ton, solving such seemingly simple problems.

## Have a look at our (unfinished) code

{% codeblock roman_numerals_converter_spec.rb %}
describe RomanNumeralsConverter do
  let(:converter) { RomanNumeralsConverter.new }

  def convert(number)
    converter.convert(number)
  end

  it 'returns an empty string for 0' do
    convert(0).should == ''
  end

  it 'returns I for 1' do
    convert(1).should == 'I'
  end

  it 'returns III for 3' do
    convert(3).should == 'III'
  end

  it 'returns IV for 4' do
    convert(4).should == 'IV'
  end

  it 'returns V for 5' do
    convert(5).should == 'V'
  end

  it 'returns VI for 8' do
    convert(8).should == 'VIII'
  end

  it 'returns X for 10' do
    convert(10).should == 'X'
  end

  it 'returns IX for 9' do
    convert(9).should == 'IX'
  end

  it 'returns XIV for 14' do
    convert(14).should == 'XIV'
  end
end
{% endcodeblock %}

{% codeblock roman_numerals_converter.rb %}
class RomanNumeralsConverter
  SPECIAL_VALUES = {0 => '', 5 => 'V', 10 => 'X'}

  def convert(number)
     result = ''

     while number > 3
       if SPECIAL_VALUES.include?(number+1)
         result += 'I'
         number += 1
       end

       nearest_boundary_under_number =
nearest_boundary_under_number(number)

       if nearest_boundary_under_number >= 0
         result += SPECIAL_VALUES[nearest_boundary_under_number]
         number -= nearest_boundary_under_number
       end
     end
     number.times do
       result += 'I'
     end
     result
  end

  def nearest_boundary_under_number(number)
    SPECIAL_VALUES.keys.delete_if {|i| i > number }.max
  end
end
{% endcodeblock %}

### Duplication in tests

We tried to remove duplication from the tests. I think we failed. 
One of my teammates pointed out that PHPUnit has
DataProviders for this kind of test data.

I didn't think that this was a good idea - but looking at the tests now I have to admit that
there is virtually no value in writing the tests spec style for this kind of problem.

* Is there something similar to a DataProvider for RSpec? 
* [I should give it 5 minutes][2]

### Write the simplest tests first

We had quite a hard time with the substraction rule (4 => IV). 
We wrote a test for the 4 very early - I think we could have made it
easier for us by first ignoring the substraction rule and focusing on
all cases where it does not apply.

* Write the easy tests first
* If you hang try another test

### Accept weired code

We "cheated" quite a bit using an array for the special cases. It just
didn't look right. Supprisingly it came together at the end of the session anyway.

* Accept weired code

### When to abstract

Right at the beginning one of my colleagues pointed out, that we just
could create a big array with a mapping between all the arabic number to
the roman numerals and use that. I think he had a point.. 

But since we wanted to create an algorithm that was no option. We
started with a big  if / elsif / else statement. That was fine for 2 or
3 tests, but then we were moving sideways. No algorithm was going to emerge.

That's when you need to stop and think about how to abstract a little bit further.

* Identify when you are moving sideways - then start abstracting

## The end

No world moving lessons - but valuable non the less. Thats why I like coding dojos - 
they are a good place to reason about the nature of programming, without
having to write production code.

[1]: http://codingdojo.org/cgi-bin/wiki.pl?KataRomanNumerals
[2]: http://37signals.com/svn/posts/3124-give-it-five-minutes
