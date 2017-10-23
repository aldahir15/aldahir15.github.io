---
layout: post
title: Towers Of Hanoi!
---

Good Saturday to everyone! Today I'll be talking about the Towers of Hanoi class I built.
I finally think i'm starting to get used to Object-Oriented-Programming, it's a really powerful way to program.
I think initially I didn't know how to really understand that everything is an object in Ruby.
But anyways, let's get into
**Towers Of Hanoi**
So we create a class called TowersOfHanoi
We initialize our starting point which is:
* 1 0 0
* 2 0 0
* 3 0 0
* A B C

Where the numbers represent discs and how big they are
~~~~
class TowersOfHanoi
  attr_reader :towers

  def initialize
    @towers = [[3,2,1],[],[]]
  end
end
~~~~

So we make towers an attr_reader because we're going to be using it and referencing it ALOT!
Now we have to think about what kind of methods we need to play this game and when does it actually end?
Let's break down the methods we're going to use:
* move : this method will take (from, to) signifying which towers they are coming and going from. move will pop a disc from 'from' and move it to 'to'
* valid_move? : it will check if the actual move is valid since we cant do things like move a disc from somewhere were there are none or move it to a place where its last piece is smaller than the moving piece.
* won? : Now, this will check if B or C have all the 3 spots taken up, meaning that the game is over. Pretty simple.

Let's start with move :
~~~~
class TowersOfHanoi
  attr_reader :towers

  def initialize
    @towers = [[3,2,1],[],[]]
  end

  def move(from, to)
    disc = towers[from].pop #this will take the last piece from the tower we want to move from
    towers[to] << disc #now we're moving the disc to the tower we want
  end
end
~~~~

Now let's do valid_move?
~~~~
class TowersOfHanoi
  attr_reader :towers

  def initialize
    @towers = [[3,2,1],[],[]]
  end

  def move(from, to)
    disc = towers[from].pop #this will take the last piece from the tower we want to move from
    towers[to] << disc #now we're moving the disc to the tower we want
  end

  def valid_move?(from, to)
    from_tow = towers[from] #just making this easier for us to reference
    to_tow = towers[to]
    return false if from_to.empty? #Just checks that we aren't taking a piece from somewhere empty
    return false if !to.empty? && from_tow.last > to_tow.last #Now this checks that if the to_tow isnt empty, that the from_tow last disc is bigger than the one we want to move
    true
  end
end
~~~~

And lastly won?

~~~~
class TowersOfHanoi
  attr_reader :towers

  def initialize
    @towers = [[3,2,1],[],[]]
  end

  def move(from, to)
    disc = towers[from].pop #this will take the last piece from the tower we want to move from
    towers[to] << disc #now we're moving the disc to the tower we want
  end

  def valid_move?(from, to)
    from_tow = towers[from] #just making this easier for us to reference
    to_tow = towers[to]
    return false if from_to.empty? #Just checks that we aren't taking a piece from somewhere empty
    return false if !to.empty? && from_tow.last > to_tow.last #Now this checks that if the to_tow isnt empty, that the from_tow last disc is bigger than the one we want to move
    true
  end

  def won?
    return true if (towers[1].length == 3 || towers[2].length == 3) #This just checks if either B or C are full and if so return true for being done.
    false
  end
end
~~~~
