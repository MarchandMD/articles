As a newbie a lot of my methods are simple. Like very simple. And since I don't know how to really organize the information well, or how to separate things effectively or optimally, my methods may be bloated. And they may "stink" as some programmers may say. If any "real" programmers were to ever look at my code. 

but I do know how to write methods. And they may be a little bloated; but I still want to write tests for them. And I still want to learn how to write software from tests first. So this will include writing some methods that have to send output to the user, as well as get output from the user. 

In the little experience I have, i'm finding it may be optimal to write even simpler methods that extract the output to the user...I mean think about this for a second: If I have to send a message to the user, wouldn't it be easier to simply have a method that contains that output? And then simply call that method from within the engine when I need to prompt the user? If I do that, I can then isolate and/or repeat the output if i need to at an appropriate time.

But that's for future programs. The program I'm writing now has methods that contain both output and input. Both `#puts` and `#gets`. And I want to know how to write tests for those. 

Enough introduction. Let's look at things: 

I have a method with `#puts` and with `#gets`, that looks like this: 

```ruby
def user_set_solution
  puts "Please select 4 of the following colors, first letters only, no duplicates: [r]ed [g]reen [y]ellow [b]lue [p]urple [c]yan"
  input = gets.chomp.downcase
  until input.length == 4
    puts "Please select 4 of the following colors, first letters only, no duplicates: [r]ed [g]reen [y]ellow [b]lue [p]urple [c]yan"
    input = gets.chomp.downcase
  end
  input
end
```

So how the heck am I supposed to test this thing? 

The way I found to test...was to do the following things: 
  
  - add instance variables to the `#initialize` method for standard input/standard output (called `@input` and `@output` for the sake of ease)
  - require `stringIO` from the standard library in the spec file
  - write a method in the spec file that sort of acts as a placeholder for actual method (more on this later)
  - Update the original method to use the `@input` and `@output` instance variables
  - write tests for the placeholder method that monitor the actual output
  - red-light until you green-light

None of that probably makes sense LOL. Totally ok. Walk through it. This is how: 

## Add instance variables to the `#initialize` method for whichever object you're working with

I happen to be working with an object called `Board` that I'm creating. 

So I go up to the `#initialize` method, and i add the following parameters to it: 

```ruby
def initialize(input: $stdin, output: $stdout)
```

honestly, I don't really know what `$stdin` does and what `$stdout` does. It does have something to do with the input and output streams for Ruby. But that's all I know. 

After I've added those two parameters, I need to know create those instance variables: 

```ruby
  @input = input
  @output = output
```

When i first did this, I was worried I was going to completely break my program. Nope. Didn't happen. Just because these instance variables are available doesn't mean the program will assign anything to them when it runs. It just means they're available. IN fact, it might actually be pretty dope to create a branch that is used specifically for testing...but that night not actually be easy to maintain. So nevermind about that idea. 

## require `stringIO` in the spec file

This is a pretty basic step. 

If you're looking at this, then you're probably comfortable with libraries. If not, briefly here's what's going on:

You know how you have Arrays and Integers and Strings? Well those are classes, right? 

You also have things like Enumerable and Comparable. Which are kind of the same as classes, but more "general" in nature. 

And all those things are just "parts" of Ruby, right? Like, the language is loaded in your computer and you know how to interact with it. 

Well now, what if I told you there were "secret" things you could do with the language? Like, things you didn't need all the time, but things you could pull in to your program when you needed them? Kind of like you can pull in extra information into your brain by going to the public library, Ruby has a library itself. And you can check out it's "books" by saying "require" whatever. 

Well the "book" we're checking out from Ruby's standard library is something called `stringIO`. And we need to use that "library book" in the spec file. So in the spec file we're going to do this: 

```ruby
../spec/board.spec

require 'stringIO'

```

Literally that's all you need to do to require `stringIO`. When the program or test suite runs it will see that line and it will go grab the things it needs to work. 

....ok. But what things are we using from `stringIO`? 

That's a great question. And here's the answer.

## write a method in the spec file that sort of acts as a placeholder for the actual method

This is the part I said i'd write more about later. This is the later i was talking about. 

Up to this point, all the code that's being written is being written in the classes. 

well, what if I told you it's possible to write a method in the spec file? 

You'd probably throw your hands up in the air and just say: "ACK! i'll never learn this!". But if you've come this far, you'll probably be able to go just a little further. 

So let's go just a little further. 

We're going to write a method in the spec file that's sort of similar to our original method we're trying to test. Remember that one? if not, that's ok...here it is again: 

```ruby
def user_set_solution
  puts "Please select 4 of the following colors, first letters only, no duplicates: [r]ed [g]reen [y]ellow [b]lue [p]urple [c]yan"
  input = gets.chomp.downcase
  until input.length == 4
    puts "Please select 4 of the following colors, first letters only, no duplicates: [r]ed [g]reen [y]ellow [b]lue [p]urple [c]yan"
    input = gets.chomp.downcase
  end
  input
end
```

But there's gonna be a few key differences. Before I point those out, first let me show the new method: 

```ruby
def user_set_solution_with_input(*input_strings)
  input = StringIO.new(input_strings.join("\n") + "\n")
  output = StringIO.new

  board = Board.new(input: input, output: output)
  expect(board.user_set_solution).to eq("rgby")

  output.string
end
```

Ok, I lied. That's like...nothing like the original method. But there are some similarities. Such as: 
- the method name
- that's really it. That's really the only similarity. lol

But this method is really where a lot of the work is being done. So let's take a look at the different parts of it. 

### 1. The name
The name is important because this is going to be a placeholder for the original method. The only real difference is...this method is "with_input". lol. It also has a splat operator for a parameter. At least I think that's what `(*input_strings)` is called: splat. 

### 2. input = StringIO.new(input_strings.join("\n") + "\n")

`input` is a local variable...local to the method.  
`StringIO.new` is using the `stringIO` library required. Apparently it's possible to fake an input stream. 
`(input_strings.join("\n") + "\n")` is the parameter that was passed to the method; Since the parameter was a splat operator (remember it was `(*input_strings)`) the parameter is an Array object. `#join` is an `Array` method, and it's how the elements of an Array are joined to be turned into a `String` object. The "\n" symbols are "newline" symbols, and they don't usually get printed to the screen, but they are interpreted by the Terminal/console when writing strings. Those little "\n" symbols are the reason the cursor ends up on a new line when ever you run some code. 
`output = StringIO.new` So this is a second local variable. Again, it's another `StringIO` object. 

### 3. create a local instance of the `Board` object, with parameters `input` and `output`
earlier in this process I updated the `#initialize` method for `Board` to allow the creating of `@input` and `@output` instance variables. 

Whatever object was created with the `StringIO` constructor command is being passed to the board. 

### 4. an expect statement
This is the interesting part of this method; or another interesting part. This method is using an "expect" statement. 

```ruby
expect(board.user_set_solution).to eq("rgby")
```

What's interesting about this is...it's a call to the `#user_set_solution` method. Which is precisely the method I'm hoping to test! 

So this idea demands some more words...

I'm currently in the process of explaining how to define the method `user_set_solution_with_input`. The reason I'm defining this method is so I can text `#user_set_solution`. I'm defining the `#user_set_solution_with_input` method inside the spec file; it will have a call to `#user_set_solution` inside of it. I'll be "faking" the actual responses to `#user_set_solution` by passing arguments/parameters to `#user_set_solution_with_input`. 

### 5. output.string
The last line of the `#user_set_solution_with_input` method is `output.string`. So `#string` is apparently a method available to a `StringIO` object, and it returns something. Thankfully, through testing, will be able to see what is being returned from `output.string` because the nature of RSpec tests is to show what it was expecting to return. 

Ok, now that this "dummy" method (or this placeholder method) has been written in the spec file, I need to move onto the next process of making this work. 

##   Update the original method to use the `@input` and `@output` instance variables

In the previous step (creating the `#user_set_solution_with_input` method) I instantiated a `Board` object inside the method, passing some parameters to the `#initialize` method for `input` and `output`. 

So now what needs to happen are some minor changes to the actual `#user_set_solution` method. These shouldn't affect the actual performance of the program, but will greatly influence the ability to pass the tests. 

First let's take a look at what the original `#user_set_solution` method looks like

```ruby
def user_set_solution
  puts "Please select 4 of the following colors, first letters only, no duplicates: [r]ed [g]reen [y]ellow [b]lue [p]urple [c]yan"
  input = gets.chomp.downcase
  until input.length == 4
    puts "Please select 4 of the following colors, first letters only, no duplicates: [r]ed [g]reen [y]ellow [b]lue [p]urple [c]yan"
    input = gets.chomp.downcase
  end
  input
end
```

To be able to test this method, any `#puts` or `#gets` methods need to be updated to reference the instance variables of `@input` or `@output`

```ruby
def user_set_solution
  @output.puts "Please select 4 of the following colors, first letters only, no duplicates: [r]ed [g]reen [y]ellow [b]lue [p]urple [c]yan"
  input = @iput.gets.chomp.downcase
  until input.length == 4
    @output.puts "Please select 4 of the following colors, first letters only, no duplicates: [r]ed [g]reen [y]ellow [b]lue [p]urple [c]yan"
    input = @input.gets.chomp.downcase
  end
  input
end
```

It happened in 4 places there. 

Depending on the method, this may vary. 

### So what's happening here? 
This is really where the "control" is happening. It might be a good idea to explore the idea of standard input and standard output for a stream. 

For now, what I'll say is: everything that is done in a program is a file. You may be asking: How can a "file" be a verb? Because a "file" is a noun...it's a thing. 

so since this is a concept i'm not very well-versed on - since I don't know it that well - I'm going to say some more things. 

A program is maybe binary. It's either doing, or receiving. It's either giving or getting. It's always active. Or it's always passive. 

The entire genesis for this article is about learning how to test a method that uses some user input. So it's about testing for when the program is waiting for something to be passed into it. Whatever gets typed onto the keyboard gets put into a file and then after the enter key is pressed is passed into the program so it can start doing things again. 

So that's why the original method had to be updated in the way it was. And since the instance variables are using `stringIO` objects, the runtime behavior of the program isn't affected. It may be necessasry to read more about `StringIO` objects. Which will happen in the next section of the Odin Project. For now, that's why the existing method had to be updated. 

##   write tests for the placeholder method that monitor the actual output

Alright, finally, it's time to start writing some tests. But this is done in a unique way. 

Setup the test with "context" and/or "describe" and the "it" block

```ruby
context "#user_set_solution" do
  it "does something cool" do
    # test code goes here
  end
end
```
Now it's time to write the unit test. Add the following by replacing `# test code goes here` with this: 

```ruby
output = user_set_solution_with_input('rgby')
```

So just by reading this, i'm setting output to be equal to the `user_set_solution_with_input` method. and I'm passin the string of `rgby`. 

It's important to look back at the "expect" clause in the `#user_set_solution_with_input` method. 

```ruby
expect(board.user_set_solution).to eq("rgby")
```

This is what RSpec is epxecting the `user_set_solution` to return. Which is exactly the same as the string being passed to `user_set_solution_with_input`. 

The reason I made the two strings the same, is because I'm expecting the user to pass in the `rgby` string when the method stops and waits for user input. 

So i'm "pretending" the user will input `rgby` on the first try, and the method will simply return what they input. Which is exactly what I wanted to have happen. 

Next part of the test should look like this (just put it on a new line in the unit test, directly below the previous code)

```ruby
expect(output).to eq <<~OUTPUT
          Please select 4 of the following colors, first letters only, no duplicates: [r]ed [g]reen [y]ellow [b]lue [p]urple [c]yan
          OUTPUT
```

which is the same as the first line in the `#user_set_solution` method. 

Just for the sake of testing, I'd do something silly here, like adding an exclamation point somewhere between the two `OUTPUT` keywords. So when the test runs it fails, so I can see what went wrong. 

Ok, that was a lot. For now I'm going to leave that as is, because I want to get back to the actual programming of the program, as opposed to the test. Because I'm sure I'll come across some other thing that needs to be explored in a matter of minutes. 


