---
layout: post
title: "Map vs Collect, and Other Ruby Synonyms"
date: 2014-08-31 16:03:49 -0400
comments: true
categories: ruby, map, collect
---

Ruby has a ton of high-level iterators. So how are you supposed to know which one to use when? More specifically, what's the difference between `map` and `collect`? They each produce a new array&nbsp;containing the results of the block applied to each element of the receiver. Both `map!` and `collect!` are offered to modify the array in place.

Some sources assert that the two methods are [the same](http://rubyinrails.com/2014/01/ruby-difference-between-collect-and-map/). Both are array methods that can work with anything that is [Enumerable](http://ruby-doc.org/core-2.1.2/Enumerable.htm). Ruby is written in C, and the C-level implementation is identical. Here is that implementation:

![image](https://31.media.tumblr.com/58cacab14980d9cb6fcdf6a4fc410c2f/tumblr_inline_n7ba2wiZs51si9gc8.png)

But then why have both?&nbsp;

Wikipedia [explains](http://en.wikipedia.org/wiki/Map_%28higher-order_function%29) that Ruby provides both to make programmers feel more comfortable. `Map` is provided by many programming languages, and `collect` came from a language called [Smalltalk](http://en.wikipedia.org/wiki/Smalltalk).&nbsp;

Other Ruby synonyms include the shovel operator `<<` and `concat`. The shovel is different than `+=` because `+=` creates a new String in memory, but &lt;&lt; is faster because it j[ust modifies the original one](http://stackoverflow.com/questions/4684446/why-is-the-shovel-operator-preferred-over-plus-equals-when-building-a).&nbsp;

The for loop vs each appear to be synonyms but there is a scoping difference. The local variable used in for loops [still exists after the loop](http://graysoftinc.com/early-steps/the-evils-of-the-for-loop), which might have some unintended consequences for your program.

![image](https://31.media.tumblr.com/199f291ec2b4895473c8eab50733e141/tumblr_inline_n7bb4tg2Kn1si9gc8.png)

On a different note, `.size` is an alias for `.length`. But `count`, which seems similar, [is actually different](http://stackoverflow.com/questions/4550770/count-size-length-too-many-choices-in-ruby).&nbsp;

![image](https://31.media.tumblr.com/ac5d9a927e601ecc93cb2b432d578819/tumblr_inline_n7bf5o2LtP1si9gc8.png)

When the inventor of Ruby, Matz himself, was [asked to explain](http://www.artima.com/intv/rubyP.html) why he provides so many ways to do the same thing, he said:

> <span>Ruby inherited the Perl philosophy of having more than one way to do the same thing. I inherited that philosophy from Larry Wall, who is my hero actually. I want to make Ruby users free. I want to give them the freedom to choose. People are different. People choose different criteria. But if there is a better way among many alternatives, I want to encourage that way by making it comfortable. So that's what I've tried to do. Maybe Python code is a bit more readable. Everyone can write the same style of Python code, so it can be easier to read, maybe. But the difference from one person to the next is so big, providing only one way is little help even if you're using Python, I think. I'd rather provide many ways if it's possible, but encourage or guide users to choose a better way if it's possible.</span>

So that's the answer. When presented with two or three or twelve different ways to do the same thing in Ruby, you should choose whichever way makes you the most comfortable.