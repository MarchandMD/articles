# Stubbing a method
So, I'm new to coding. Like, I'm teaching myself. And I have to figure things out on my own. 

And it isn't easy! At least, some of it isn't easy. Some things make sense, and other things don't really make sense. 

For example: I'm learning how to write a program using Ruby; which is also called building a Gem; which is also called writing code. 

And one of the ways (actually a really big way, I think) of writing code/building a program, is by writing tests. In RSpec. 

  *But what the hell is writing a test? And how does it help me write code?*

Well, that's a larger topic. And I'll write more about that in a different article. 

For now, I just learned something in RSpec and I want to write it down so I have it published somewhere. Because this thing that I just learned, stubbing a method, feels like it could be helpful for writing future things. I'm just not sure how, yet. Maybe it will be really helpful. 

Hopefully you're just like me and you were doing some work involving testing with RSpec stubs or doubles and need to know what the heck stubbing is and how to do it. And hopefully this makes sense to you. 

If you go through this and it still doesn't make sense, please contact me! I'd love to be able to try to explain it! The more I explain hopefully the better I get at it!

Ok, enough about that. 

First, a little context so the rest of what I say makes sense.

I'm following a tutorial about building a TicTacToe Gem in Ruby. The tutorial is old. And it hasn't been updated. Maybe it's old by design. Maybe it's outdated on purpose. I don't know. But one of the tests in the tutorial shows this: 

```ruby
  board.stub(:winner?) { true }
```

I'm not going to explain the context of this until I have to. I'm just going to continue hoping you're here because you see a similar piece of outdated code and just want to fix it. 

Spoiler alert: that code above is old. The `#stub` method doesn't really work in RSpec. At least not without changing some things in the "guts" of your RSpec. And I'm not your resource for that yet. 

I ran my test in RSpec with that old code, and I got a Failure/Error: NoMethodError: undefined method 'stub'... etc

So I had to look for some way to fix this piece of code; some way to fix this test. 

The best I could find was this: 

```ruby
allow(board).to receive(:winner?) { true }
```

Ok, so I see some similarities between the old way and the new way:  

1. `board`
2. `:winner?`
3. `true`

But that's where the similarities end. 

So from that, I had to figure out what the hell was going on. 

To understand that requires a little more context about my specific "problem". 

I'm attempting to test an instance method for an object. The instance method I'm testing is called `#game_over`. Now the `#game_over` method has been written, but it's been written using some other methods that haven't been written yet. 

So this is a strange concept to me, and is sort of the reason why I'm writing this article. 

> # How can I write a method for one object using methods that don't exist yet? 

The best answer I have now is: if I can sort of envision what those other methods will do, or what I need the other methods to at least return...then I should be able to sort of plan to make those other methods at some time in the future. The most important thing being: I know what the method will return.

Another important thing to remember is: just because I write some code...it doesn't have to work perfectly yet. Not until the complete program is called/ran do I need it to work perfectly. 

And that's the beauty of building via Testing; I can control or manipulate the testing "environment" to control how a piece of the program should work. As long as any code that is "mimiced" in the test environment is eventually actually written into the program, then there won't be an issue. 

If i know what return value I need from a non-existent method, then I can write a method that uses the "not yet existing method".  

Then, I can test the method that now exists that uses the non-existent method, and I can FAKE the non-existent method. 

*Wait...what?* 

*How can you FAKE a method?* 

Because the only thing I need to fake a method is a fake name and a real result. Or a real return value. 

I don't know if that's making sense, so here's another way to look at it. And bare with me, because this example also explains where the name "double" comes from. (Double is another name for what we're doing.)

# An allegory to help explain stubbing

Pretend writing code is like making a movie.  

And now pretend a method is like a movie actor.   

And a test is like an out-take. 

Now in the out-take, there's a stunt the actor has to do. But I don't want the actor to actually do the stunt. I want the stunt-double to do the stunt. 

That's how I think of the double. It's a movie stunt-double. 

It's standing in for something else that doesn't exist yet. 

And it's only standing in for something else that doesn't exist in the out-take. Not in the actual movie. 

Does that make more sense? 

Since I'm writing tests, not the actual program, it's possible to "fake" the operation of a program. And the way to "fake" the operation of a program is with a double. 

Ok, well now that I've explained the idea of a "double" in a couple of ways, how do I get from: 

```ruby
  board.stub(:winner?) { true }
```

to  

```ruby
allow(board).to receive(:winner?) { true }
```
. 

The `board` object is an instance of an `Object` that's specific to my program. I'm making a TicTacToe game, and so the `board` object is an instance of the TicTacToe `Board` class. 

And the situation that I'm testing on the `board` object is when the game is over. So the method I'm testing is `#game_over`. One thing I need the program to be able to identify is if the player is the winner. 

So the method that i'm "stubbing"...or the method that needs a stunt-double is `:winner?`. 

The reason I need the `:winner?` method to be a stunt-double in my test code is because I haven't written the `:winner?` method yet in the `Board` class, but I'm using the `:winner?` method in the `#game_over` method, which has been written.

The simpleton in me at this point wants to say: just write the `:winner?` method in the `Board` class and be done with it. But there's a couple of reasons why that won't necessarily work. Or why it doesn't need to work right now. 

One possible reason is, maybe I know generally how a game of TicTacToe is won. I mean, that's pretty easy to explain in plain English; But identifying that logic from the eyes of a program is a little more involved. 

So it's just easier for me to say, for now, the `#game_over` method is defined like: 

```ruby
def game_over
  return :winner if winner?
  return :draw if draw?
  false
end
```
This is such a straight-forward "dumb" sort of method. But it's also really elegant. 

Now this is probably getting further away from helping someone else specifically, but the point still remains. There may be a situation when needing to use a method that doesn't exist yet. 

Looking in the definition of the `#game_over` method, the first line shows that the :winner symbol will be returned if `winner?` is true. 

The second line goes on in the same vein. 

Now, I don't really need to know what the `:winner` symbol does. Not at this point. But what I do need to do is I need to force the `#winner?` method to return `true` within the context of my test for `#game_over`, so that `game_over` method will return `true`...because that's what I'm expecting the current test to return.

But as I mentioned before, I haven't written the `#winner?` method. So if I run the RSpec test with `#game_over`...I'll most likely get a NoMethodError, since `#winner?` doesn't exist. 

This is the work around. 

The `#stub` method is no longer used in RSpec. At least, not in the most recent version. 

it's been replaced with this new version: 

```ruby
allow(board).to receive(:winner?) { true }
```

And what this is saying is: allow the `board` instance of the `Board` class object to receive a method called `winner?`...and when it does, return the value to be `true`. 

That's really it.

So when I run the test, the test for this example will see the call to `#game_over` in `expect(board.game_over)` and it will see a call to `#winner?` and it won't even look for a `#winner?` method; instead RSpec will automatically just say   
>*"oh, i know the answer to this: it's supposed to be `true`, so just keep moving forward".*

More magic happens though, when writing the test. LIke in the code example. 

When writing tests in RSpec, it's like a way to manipulate the run-time environment of a program. What I mean by that is: when I write an RSpec test, I provide all the basic things I need for my code to run successfully. Then, after I provide all that stuff, I also write the expectation of what all the basic things should do when they run. Here, look at this: 

```ruby
context "#game_over" do
    it "returns :winner if winner? is true" do
    board = Board.new
    allow(board).to receive(:winner?) { true }
    expect(board.game_over).to eq :winner
  end
end
```
Alright, so hopefully at least some of this looks familiar to you. If it doesn't go back a step and look at another article about how to setup up RSpec tests. 

For my example, what's happening here is:   

1. I'm describing the context of `#game_over`
2. and I'm saying it "returns :winner if the `#winner?` method is true
3. Now that I know what I should be expecting, I create a new `Board` object to work with
4. I'll be testing the `#game_over` method, which I know contains `#winner?` so I tell the test environment to allow the `board` instance of the `Board` class to be able to receive the `#winner?` method
5. On the same line, I also tell the test environment what to do after it allows the `board` instance of the `Board` class to receive the `#winner?` method....make it return `true`
6. Now that I have everything the way it needs to be, I write the expect statement. This says I expect `board.game_over` to equal `:winner`. If you look back at the `#game_over` method, you'll see that this test should pass. 

Spoiler: the test does pass. If the test didn't pass, please contact me! Maybe your testing environment isn't setup correctly. 

For my code, I get two additional chances to use the `allow().to receive() { } ` format. But safe to say, after getting the first one to run, I feel pretty confident about the success of the other stubs. 

So that's it. That's how the RSpec `#stub` has been chagned to `allow().to receive() {}`.

There are some other options to `allow().to receive() {}`...but you're more than welcome to explore those other options on your own at the RSpec documentation pages available for free on the internet. 

Thanks for reading! and of course feel free to contact me if something is just plain wrong. 