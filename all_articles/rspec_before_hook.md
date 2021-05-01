# RSpec `before` hooks
Learning to use RSpec is tough stuff. 

But it's not impossible. 

Being genuinely interested in it has really helped.  

And the more I've learned about TDD, writing tests, and RSpec itself, the more I'm fascinated by what it is and how it works. 

And there's a lot to learn about it. 

What's fascinating about RSpec today is the `before` hooks. 

But before I spend any time talking about that, I want to say a couple of common sense things. 

When I'm writing tests, i'm doing a couple of things. One of the things I'm doing is writing the "context" as well as the "it" statements. 

And after I've written those....then I'm writing the actual code inside of the unit test. 

A lot of what I'm doing when I'm writing the actual code is repeating some things. Often i'm instantiating an object, so I can call the new methods I'm working on. 

What if there was a way to run a small snippet of code for the entire suite of tests, that RSpec used for all the unit tests? 

For example, what if I had a way to instantiate a single object, and tell RSpec to use that single object for every test? 

Well, that might make things a little easier. And that's where the `before` hook comes in. 

## What situation am I trying to fix, with `before`? 

Here's some code, to help illustrate: 

```ruby
RSpec.describe SomeObject do
  context "#initialize" do
    it "does something when initialized" do
      an_instance = SomeObject.new # the `before` hook will remove this
      expect(an_instance).to be_an_instance_of(SomeObject)
    end
  end

  context "#some_new_method" do
    it "does something" do
      an_instance = SomeObject.new # the `before` hook will remove this
      expect(an_instance.some_new_method).to eq ('some output')
    end
  end
end
```

So this is obviously a simplified test suite. Theoretically it works. 

However, the `before` hook could be used to simply this process. Here's how. 

```ruby
RSpec.describe SomeObject do

  before(:each) do
    @an_instance = SomeObject.new
  end

  context "#initialize" do
    it "does something when initialized" do
      expect(@an_instance).to be_an_instance_of(SomeObject)
    end
  end

  context "#some_new_method" do
    it "does something" do
      expect(@an_instance.some_new_method).to eq ('some output')
    end
  end
end
```

## So what changed? 

If you can't tell by looking at it, here's what happened:

  - the `before(:each)` hook and the `do/end` block was added
  - each unit test removed the instantiation of `SomeObject.new`

If you don't write a lot of tests this might not seem like it'd be a game changer; However, if you write a lot of tests this could make a big difference. 

## So what? 
So if I understand the `before` hook, I can start to think about what it's doing. 

It looks like it's creating an instance variable, called `@an_instance`. And if I were writing a program that's probably exactly what I'd be writing. But this is in the context of RSpec; so what appears to be happening is i'm creating an instance of the object to be used in RSpec. 

This begs the question: is the instance of the object that was created during the `before` hook the same object being used in each unit test? That's a very good question and something worth exploring. 