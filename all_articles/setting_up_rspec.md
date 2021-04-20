# Setting up RSpec

So, I'm relatively new to programming/coding etc. One thing I've found to be infinitely interesting and helpful has been working with RSpec. But I've found that it was a topic that was shrouded in mystery.  

I want to change that.  

Not necessarily for other people, but mostly for myself. Because knowing how to test, and write tests, or build using tests, or reverse engineer has been super helpful. I've been able to write a test suite for some repo on GitHub, and I learned so much about logic, and project development.  

This article is about how I set up RSpec initially, and then how to configure/format RSpec so it looks better. 

So let's go!

## Assumptions before getting too deep 
Remember high school Geometry class? Each program had "givens"...which were like little hints or clues or facts about the "reality" of the problem. 

Well, before setting up and configuring RSpec to make it work and look pretty there need to be some givens about how I'm approaching this. 

1. This is in the context of a Gem file directory (YMMV based on how your root directory is structured)
2. There is at least one class created that you'd like to test, with in a `Module` (if I can remove this assumption i will; but initially i'm leaving this; I supposed I could test an existing object....)
3. You are a complete noob! You've created a Gem file directory, but don't really know what's going on in the file directory
4. You actually have rspec installed on your machine! if you run `rspec --version` you should see something that tells you what version of RSpec you have. Like `RSpec 3.10`...and maybe some other things (like versions of rspec-core, rspec-expectations, rspec-mocks and rspec-support most likely)

And that's it! If I think of any other necessary assumptions i'll add them in retroactively. 

Ok, so now let's move on to getting things started. First step is going to be adding a class to test. 

## Add a class

Create a new file in the `<your root directory>/lib/<your gem name>/` sub-directory (aka folder). My current project is a mastermind project, so my filepath will look like `mastermind/lib/mastermind/cell.rb`...where the first "mastermind" is the root directory, "lib" is a sub-directory, and then the second "mastermind" is a sub-directory of the "lib" sub-directory. 

Whew. That's a lot. The new file I created is `cell.rb`. This is where I'll add the following code: 

```ruby
module Mastermind
  class Cell
    attr_accessor :value

    def initialize(value = "hello")
      @value = value
    end
  end
end
```

Obviously this doesn't do much, but it's enough to start setting up RSpec, and writing a test. 

On to the next thing! Which takes us to a file with a lot of scary looking text: the `.gemspec` file!  

## Adding RSpec to the project of a Gem
The reason I'm writing this article from the context of a Gem is because the initial command of starting a gem (that is `bundle gem <name_of_your_gem_here>`) creates a directory and files that are important to setting up RSpec. For instance, this `.gemspec` file. 

Now, on my local machine i have to do a couple things to this `.gemspec` file to make it run. You may have to do something different, but here's how I have to add RSpec to my project: 

1. I open the `.gemspec` file
2. I delete all instance of the "TODO:" text; nothing else...just the exact text of "TODO:" and where ever it appears. 
3. I also comment out the line that says "spec.homepage" and all the instances of "spec.metadata" (as of the writing of this, there are like 4 different `spec.metadata` that I comment out)
4. Add this line right before the final `end` : `spec.add_development_dependency "rspec"`


Now at this point I have to plead a little ignorance, because I'm doing this from memory. But for the most part you're done with the `.gemspec` file. 

Now comes the next part: adding the `spec` folder and the `spec_helper` files

## adding the `spec` directory
It's ok to close the `.gemspec` file now. You won't be using it anymore for any thing. 

Let's do 2 easy things now: 

1. Add a new folder to the `root` directory (for me, it's the main "mastermind" folder), and call it "spec" (like specifications, but I still pronounce it "speck")
2. inside the `spec/` directory add a file called `spec_helper.rb`

that was easy! 

Ok, now that that's done.....what the heck is that stuff? 

### what is the `spec` directory and the `spec_helper.rb` file? 

the `spec/` directory is where all the tests will be written. As I go on in this article, we'll see that each object will get it's on test file.  

the `spec_helper.rb` file is like Grand Central Station. Or a train depot. Or like a hub. It's a file (one of a couple) that help to bring together all the separate files that need to be looked for the program to work...for the tests to run properly. 

This may be a little confusing, and this is where things begin to get confusing. 

So now is a good time to start talking about connecting all these different files to each other!

## Connecting files to each other
Look at `../lib/mastermind.rb`. What do you see in there? (and remember, "mastermind" might not be the name of your gem or file...)

```ruby
require "mastermind/version"

module Mastermind
  class Error < StandardError; end
  # Your code goes here...
end
```

You probably see something similar to this. 

Now I want to point out: none of that code is code that I typed! It was all there already! As soon as I generated the Gem file structure that stuff was automatically made!  Pretty cool, huh?!

So why are we looking at this? Well, because we're going to update it. but first, I want to point something out...

It's possible for me to include that new class I created (remember I created something called the `Cell` class at the beginning of this?) without actually  typing it inside that `module Mastermind` code.  

Seriously.  

Since I already created a `Cell.rb` file...I can "bring in" that class into the actual `module Mastermind` but adding the following line: 

```ruby
require_relative "./mastermind/cell.rb"
```

so I'm going to add that line into the `lib/mastermind.rb` code...so your finished product will look like this: 

```ruby

require "mastermind/version"

module Mastermind
  class Error < StandardError; end
  # Your code goes here...
end

require_relative "./mastermind/cell.rb"

```

Nice! Now my `module` is growing!! 

but wait, this is supposed to be about RSpec. So, how do I test this code for the `Cell` class? 

## Writing a test for the `Cell` class

in the `spec/` directory (same place as `spec_helper.rb`) create a file called: `cell_spec.rb`

add this code, or something similar: 

```ruby
require_relative "spec_helper.rb"

module Mastermind
  RSpec.describe Cell do
    context "#initialize" do
      it "does something kinda cool" do
        cell = Cell.new
        expect(cell.value).to eq "hello"
      end
    end
  end
end
```

Again, we see `require_relative` but this time it's referencing `spec_helper.rb`. 

Time to go over there!

## adding stuff to `spec_helper.rb`

Ok, now `spec_helper.rb` should be a completely blank file. 

Let's change that. add the following: 

```ruby
require '../lib/mastermind.rb'
```

Of course, it may be titled something different for you, but it'll be the main folder that has your module in it. 

the idea is: when you call `cell_spec.rb` in the command line, the first thing that'll happen is the computer will go grab the `spec_helper.rb` file...so it can help itself. Next, it will look at the `spec_helper.rb` file and it'll see requirement to grab the `lib/mastermind.rb` file. So then the computer will then go to the `lib/mastermind.rb` file and it will grab the hard code there. The computer will see the code for the `module Mastermind`...and it will also see the `require_relative 'mastermind/cell.rb`. So then the computer will go to the `cell.rb` file and look at the code that was written for the `Cell` class. 

At that point, the computer will then move forward with running the test and evaluating it for correctness. 

## Configuring RSpec to be prettier
Assuming you got a successful test, you most likely see a green dot. That means you were successful...or at least the computer successfully evaluated the test you wrote, using the code you wrote. 

But that output is so bland. 

In your root directory (that should make sense to you at this point) add a file called `.rspec`. 

in that file add the following text: 

```
--format documentation
--color
--require spec_helper
```

Run the tests again, and you'll see the results formatted in a much cooler way, that are actually helpful. 

## Conclusion

I sort of rushed through the last part, but that's how to set it up!

Now go forth and start hacking the heck out of code!