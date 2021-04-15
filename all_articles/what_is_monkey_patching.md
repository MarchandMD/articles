# Monkey Patching

At this point of my coding/learning process I'm comfortable with the idea of `Objects` in Ruby: I understand there are `Arrays` and `Strings` and other data types.

I'm also familiar with the idea that instances of these `Objects` have methods.

The academic/student in me wants to learn all the objects and all the methods that exist. It's just a natural extension of my learning process: If I'm learning things on my own and I'm teaching myself, why not teach myself all the different Objects and **ALL** the different methods that exist for them?

While that's a noble idea, it's also a bad idea.  

Many times I've heard it said/written that programming is NOT about memorization. (though it could be about MEMOIZATION, lol get it? Ok that's a bad programming joke)

I mean, everything in Ruby is an `Object`. And then there's the idea of inheritance. And there's all these increasing levels of abstraction that would eventually make sense...but it'd be a really long time before I'd understand everything, if that would ever happen.

What I've found is that it's a little more natural to actually be building programs and then find which Objects and methods I need specifically.

It's the idea of learning by doing, instead of by memorization.

Eventually more Objects and more methods will become apparent to me.

However, there is one really neat feature of coding that is the purpose of this article:

> # It's possible to create methods that I can add to the language at the global level (at least for my program)

That's a strange idea to me as a noob. 

 Throughout my entire journey there's always been this idea of some "source" code that just shouldn't be messed with. Whether it's on `SUDO` on the command line, or a  configurations within the machine, there's always an aversion to making changes to the "source" code.

However, knowing that I can "extend" an of the `Objects` that already exist in Ruby shouldn't be something to be afraid of.

Being able to add to the actual language feels like a powerful thing, and the point isn't to be afraid of it; Rather, see it for what it is: just writing a `#method` for an `Object`.

## Extending the Language

So there's something that programmers call "monkey patching". I don't really know where it came from, but i think the point is, Monkey-patching is just like writing code, and then adding it to the main code without it actually being an official update or change. Just something added for the time being to make it work.

so I'm going to extend the `Array` class.

In my project (or any project you might be working on) just create a file called `core_extensions.rb`.

Now, since in my example I'm extending the `Array` class, I need to start writing code that allows me to update it.

`Array` is just a class. I've already written classes in my project. So I'm just doing this:

```ruby
class Array
end
```

and then I'm adding additional methods within that class:

```ruby
class Array
  # my methods will go in here
end
```

The reasoning for wanting to "monkey patch" the Ruby Language is a little confusing to me. I'm not really sure "when" it would be the right thing to do.

But as I reflect a little more about it and use my brain, I tell myself the following:

> If I can monkey patch the Ruby language for my program, why wouldn't I try to put everything into the Ruby langauge? Like, why wouldn't I put all my objects into Ruby?

And I guess there's a couple of answers to that:

1. that's sort of already being done, by creating new objects
2. that'd be sort of a dumb thing to do, because some new object I name and create wouldn't necessarily be the same as someone elses

I mean, one of the main idea of OOP is Encapsulation. That is, something like an Array is finite. But it can be extended. More on this later, maybe.

For now, I think it's best to think that Monkey patching may work when I need or want to do something to an existing Ruby Object, like an Array. And that I actually am putting everything into the Ruby program by writing my little program, or Gem, or whatever thing I might be building.

But I think being able to identify when I can Monkey Patch something will come in time.

So for now, here's what my tutorial is doing:

```ruby
class Array
  def all_empty?
    self.all? { |element| element.to_s.empty? }
  end
end
```

So this `#all_empty?` method is a new predicate method that combines `Enumerable#all?` and `Array#empty?`. 

# Conclusion
And that's all there really is to Monkey-Patching!  

At least for now.  

As I continue my journey in programming and coding I may learn more about monkey patching, and it may become more second nature to me. 

But for now, if I were to walk away and forget everything I think this article would help me in some small way.  

Thanks future me!
