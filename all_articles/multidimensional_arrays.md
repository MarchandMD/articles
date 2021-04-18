# Multi-dimensional Arrays

Here's a quick look at multi-dimensional Arrays. 

Don't be intimidated by the name.

If I know what an `Array` is, then I know what a multi-dimensional Array is. 

> A multi-dimensional Array is: An Array with other Arrays inside of it

Now, it could be as simple as an Array inside of another Array... like this: 

```ruby
[[1, 2, 3, 4, 5]]
```

But that doesn't really illustrate the idea well. Before I look at a more practical example just know that a multi-dimensional Array is an Array with other Arrays inside of it. 

A more practical example would be an `Array` with several `Arrays` inside of it, like this: 

```ruby
[
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]
```

Formatting it like I did above is a really helpful way to visualize a multi-dimensional Array. I can do the same thing in another visually ugly way: 

```ruby
[[1, 2, 3], [4, 5, 6],[7, 8, 9]]
```

But I don't think that looks as good. 

so then let's say I wanted to "access" this multi-dimensional array. Like, for example, I had a need to grab the number 1 from the multi-dimensional array. How could I grab that number? 

First I'll show you how. Then I'll explain it. Here's how: 

```ruby
# I need a variable that references the multi-dimensional array; I called it my_array
my_array = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]

# this is how I'd grab the number 1 from within the multi-dimensional array
my_array[0][0]
=> 1
```

And this is how that works. 

A multi-dimensional Array is simply an Array with other Arrays in it. 

So, without data, it's this: 
`[[]]`

Of course, it could be `Arrays` in `Arrays` in `Arrays` in `Arrays`....etc. It's rare that it would be that complicated, but it's possible. Doesn't really change anything. 

if I wanted to grab the value of 3, i'd need to do this: 
```ruby
my_array[0][2]
=> 3
```

If I were to call `my_array[0]` what would the response be? 

```ruby
my_array = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]

my_array[0]
=> [1, 2, 3]
```

And I could also call `my_array[1]` and/or `my_array[2]`. 

But to get inside of the nested Array (that is, the Array that's inside the array), I can do so by giving another number. `my_array[0][1]`. 

Ok. That makes sense. 

Not really that complicated. But it's a start. Maybe someone else will see this and it'll answer some questions for them. 


