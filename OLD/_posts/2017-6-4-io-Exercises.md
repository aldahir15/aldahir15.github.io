---
layout: post
title: io-Exercises
---

Today I learned more about user input and output. This post will mostly just be about the guessing game project I made.
This project required a couple things:
* Obviously a random number to guess to
* User input for the person guessing
* Methods to check the various stages of checking

~~~~
class NoMoreInput < StandardError

end

def guessing_game
  our_number = 1 + Random.rand(99)
  input = ask_for_input
  puts input
  number_of_guesses = 1
  while check_if_equal(input, our_number) != true
    input = ask_for_input
    number_of_guesses += 1
  end
  "#{our_number} is right, you used #{number_of_guesses} guesses"
end

def ask_for_input
  puts("guess a number: ")
  input_here = gets.chomp
  raise NoMoreInput.new() if (input_here.to_i < 1 || input_here.to_i > 100)
  input_here.to_i
end

def check_if_equal(num, ans)
  return true if num == ans
  if num > ans
    puts "/too high/"
  elsif num < ans
    puts "/too low/"
  end
end
~~~~

This is pretty simple. guessing_game starts the game which calls ask_for_input which asks the user to guess a number between 1 and 100 if not an error is raised. Then afterwards it calls check_if_equal which as the name suggests, checks if the number is equal to the random number. This is iterating until it reaches the number then afterwards it returns a string stating the number and the number of guesses used.
