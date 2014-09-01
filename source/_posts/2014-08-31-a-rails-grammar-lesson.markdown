---
layout: post
title: "A Rails Grammar Lesson"
date: 2014-07-30 16:33:41 -0400
comments: true
categories: ruby, ruby on rails, rails, grammar
---

Since I first started learning Rails, one thing that struck me was how incredibly smart the framework is. Not just in anticipating paths and RESTful patterns, but also how class `Person` magically becomes table `People` in the database. How in the world does Rails know that it's not the `Persons` controller/table? Has somebody hardcoded every possible grammar edge case? I made it my mission to find out. 

As [this](http://underpop.free.fr/r/ruby-on-rails/cookbook/I_0596527314_CHP_2_SECT_6.html) handy blog post points out, if you want to see how Rails _would_ pluralize a word, just run a generator command with the `--pretend` or `-p` option:

![image](https://31.media.tumblr.com/5de321ac423d0c7b39be8bacf15dcce6/tumblr_inline_n9icpdrYV71si9gc8.png)

This prints out a list of all the files Rails would create if you ran a generator command without the option. This is also useful if you forget the difference between scaffolds and resources.

Convention is part of what makes Rails so powerful. The framework can anticipate what you need and generate your file structure for you-- but at the end of the day, rules were meant to be broken. You can turn off pluralization completely by adding this line of code to your config/environment.rb file:

<pre>ActiveRecord::Base.pluralize_table_names = false</pre>

But keep in mind that your whole project will break.

You can also experiment with existing conventions by testing pluralization patterns in your Rails console. Use the `pluralize` and `singularize` methods:

![image](https://31.media.tumblr.com/ecd1066dfe6cc67eaeb37f1396053f36/tumblr_inline_n9idb92Vff1si9gc8.png)

Where do these methods come from? Glad you asked! Rails depends on a gem called [ActiveSupport](http://api.rubyonrails.org/) that provides string pluralization patterns (and MANY other things.) Many of the files in the "[inflector](http://api.rubyonrails.org/classes/ActiveSupport/Inflector.html)" directory deal specifically with how and when to pluralize words.

If you love reading source code, the [inflector/methods.rb](https://github.com/rails/rails/blob/36019329b015e570a58d9f34027c6e73edc36b77/activesupport/lib/active_support/inflector/methods.rb) and [string/inflections.rb](https://github.com/rails/rails/blob/aa0e1fac3e2988710f799795eaae2a06202540c6/activesupport/lib/active_support/core_ext/string/inflections.rb) files use higher-level methods like `tableize`, which depends on the lower-level methods like `plural` and `singular` in the [inflector/inflections.rb](https://github.com/rails/rails/blob/efff6c1fd4b9e2e4c9f705a45879373cb34a5b0e/activesupport/lib/active_support/inflector/inflections.rb) file. **The rules are defined in the first place in the lowest-level file, [active_support/inflections.rb](https://github.com/rails/rails/blob/92f567ab30f240a1de152061a6eee76ca6c4da86/activesupport/lib/active_support/inflections.rb).** It handles both normal and irregular patterns.

![image](https://31.media.tumblr.com/d04b350306707a1627bb69594ee39adc/tumblr_inline_n9if77EKRY1si9gc8.jpg)

The [Ruby on Rails Cookbook](http://underpop.free.fr/r/ruby-on-rails/cookbook/I_0596527314_CHP_2_SECT_6.html) explains how to override the rules provided in the [inflector](http://api.rubyonrails.org/classes/ActiveSupport/Inflector.html) files:

<pre>_`$ ruby script/console
>> "foo".pluralize
=> "foos"`

Rails calls words that are the same in both plural and singular form uncountable. To add the word foo to a list of all uncountable words, add the following to the bottom of environment.rb:

config/environment.rb:

`...
Inflector.inflections do |inflect|
 inflect.uncountable "foo"
end`

Reload script/console, pluralize foo again, and you'll find that your new inflection rule has been correctly applied.

`$ ruby script/console
>> "foo".pluralize
=> "foo"`

Other inflection rules can be added to the block passed to Inflector.inflections. Here are a few examples:

`Inflector.inflections do |inflect|
 inflect.plural /^(ox)$/i, '\1\2en'
 inflect.singular /^(ox)en/i, '\1'
 inflect.irregular 'octopus', 'octopi'
 inflect.uncountable "equipment"
end`

These rules are applied before the rules defined in inflections.rb. _</pre>
<pre>_Because of this, you can override existing rules defined by the framework._</pre>

So there you have it, fellow grammar enthusiasts. The Rubyists who came before us took the time to painstakingly define pluralization rules in [this](https://github.com/rails/rails/blob/92f567ab30f240a1de152061a6eee76ca6c4da86/activesupport/lib/active_support/inflections.rb) file. You can override them if you'd like. In fact, you might _have_ to override them if your models are made-up or non-English words.

![image](https://31.media.tumblr.com/bbe7cd18619444794e45c490042d8cce/tumblr_inline_n9ifetNwqO1si9gc8.png)