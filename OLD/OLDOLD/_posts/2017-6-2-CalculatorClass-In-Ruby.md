---
layout: post
title: RPNCalculator Class!
---

Alright, so today I did a prep work problem regarding Ruby classes. The problem set for me to build a calculator class but instead of taking in straightforward equations (ie., " 1 + 2 + 3 * 5 "), instead it was in this format (" 1 2 3 4 5 + + + * ") called Reverse Polish Notation.

So I knew the first thing I had to do was initialize the class and have an empty array that we can push numbers into:

<pre>
  <code class="ruby">
    class RPNCalculator
      def initialize
        @equation_arr = []
      end
    end
  </code>
</pre>

Then we add in a push method so we can add in numbers we want to use in our calculator

<pre>
  <code class="ruby">
    class RPNCalculator
      def initialize
        @equation_arr = []
      end

      def push(num)
        @equation_arr << num
      end
    end
  </code>
</pre>

Pretty simple huh. Well turns out I couldn't finish the last problem. Stay strapped in.
Now we just have to add in methods for adding, subtracting, multiplying, and dividing:

<pre>
  <code class="ruby">
    ...
    def plus
      raise "calculator is empty" if @equation_arr.empty?
      dummy_arr = [@equation_arr.pop, @equation_arr.pop].reverse
      @value = dummy_arr.inject(:+)
      @equation_arr.push(@value)
    end

    def minus
      raise "calculator is empty" if @equation_arr.empty?
      dummy_arr = [@equation_arr.pop, @equation_arr.pop].reverse
      @value = dummy_arr.inject(:-)
      @equation_arr.push(@value)
    end

    def divide
      raise "calculator is empty" if @equation_arr.empty?
      dummy_arr = [@equation_arr.pop.to_f, @equation_arr.pop.to_f].reverse
      @value = dummy_arr.inject(:/)
      @equation_arr.push(@value)
    end

    def times
      raise "calculator is empty" if @equation_arr.empty?
      dummy_arr = [@equation_arr.pop, @equation_arr.pop].reverse
      @value = dummy_arr.inject(:* )
      @equation_arr.push(@value)
    end
    ...
  </code>
</pre>

And to finish this part off, we just have to have a way to call our final value back to us!

<pre>
  <code class="ruby">
  ...
  def value
    @value
  end
  ...
  </code>
</pre>

Easy right! Except, we also had to write in a way to take in the actual notation ("num num num add add add").
So for this we needed a tokenize method, that just took in th string and spit out an array of what we're supposed to do:

<pre>
  <code class="ruby">
  ...
  def tokens(string)
    nums = ("0".."9").to_a
    new_str = string.delete(" ").split("")
    final_arr = []
    new_str.each do |x|
      if nums.include?(x)
        final_arr << x.to_i
      else
        final_arr << x.to_sym
      end
    end
    final_arr
  end
  ...
  </code>
</pre>

This just returned an array similar to this [1,2,5,:/,:-] (note the signs are symbols)
And lastly we needed a way to evaluate this array:

<pre>
  <code class="ruby">
  ...
  def evaluate(string)
    into_array = tokens(string)
    @equation_arr = into_array - [:+,:-,:*,:/]
    into_array.each{|x|
      if x.class == Symbol
        if x == :+
          plus
        elsif x == :*
          times
        elsif x == :-
          minus
        elsif x == :/
          divide
        end
      end
    }
    value
  end
  ...
  </code>
</pre>

This is my code so far. What I have a problem with now is just evaluating a problem that's supposed to be broken up by parentheses like this: [1, 3, 5, :+, :-, 5, 4, :-, :/]. This is causing me problems either because i'm tired and can't think straight or because I need to find a way to break this into two separate equations and then combine them.
Hopefully by next post i'll have an answer!
