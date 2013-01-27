---
layout: post
title: "Kata: FizzBuzz. 6th try."
date: 2013-01-26 18:05
comments: true
categories:
  - ruby
  - coding kata 
---

I like the FizzBuzz kata a lot. This is my 6th try.

## Code

{% codeblock fizz_buzz_game.rb %}
class FizzBuzzGame
  attr_reader :range

  def initialize(range)
    @range = range
  end

  def play
    range.map { |round| answer_for_number(round) }.join("\n")
  end

  def answer_for_number(number)
    if (number % 15).zero?
      'fizzbuzz'
    elsif (number % 3).zero?
      'fizz'
    elsif (number % 5).zero?
      'buzz'
    else
      number.to_s
    end
  end
end
{% endcodeblock %}

## Tests

{% codeblock fizz_buzz_game_spec.rb %}
describe FizzBuzzGame do
  it 'answers 1 in the first round' do
    answer_for(1..1).should == '1'
  end

  it 'answers fizz in the third round' do
    answer_for(3..3).should == 'fizz'
  end

  it 'answers buzz in the fith round' do
    answer_for(5..5).should == 'buzz'
  end

  it 'answers fizz for every number divisible by 3' do
    answer_for(9..9).should == 'fizz'
  end

  it 'answers buzz for every number divisible by 5' do
    answer_for(25..25).should == 'buzz'
  end

  it 'answers fizzbuzz for every number divisible by 5 & 3' do
    answer_for(90..90).should == 'fizzbuzz'
  end

  it 'prints every answer in a new line' do
    answer_for(98..100).should == '98\nfizz\nbuzz'
  end

  def answer_for(range)
    FizzBuzzGame.new(range).play
  end
end
{% endcodeblock %}

## Documentation

    FizzBuzzGame
      answers 1 in the first round
      answers fizz in the third round
      answers buzz in the fith round
      answers fizz for every number divisible by 3
      answers buzz for every number divisible by 5
      answers fizzbuzz for every number divisible by 5 & 3
      prints every answer in a new line


## Thoughts

I like this implementation a lot. Using a range makes for a nice
interface and it's [nearer on the specifications][1]:

> Write a program that prints the numbers from 1 to 100

    1
    2
    Fizz
    4
    Buzz
    Fizz
    7
    8
    Fizz
    Buzz
    11
    ..

So I would argue that it's definitely better then to start with only one
function, that you just pass a number.

[1]: http://codingdojo.org/cgi-bin/wiki.pl?KataFizzBuzz
