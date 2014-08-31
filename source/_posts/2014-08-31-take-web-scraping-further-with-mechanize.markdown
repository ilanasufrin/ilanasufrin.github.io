---
layout: post
title: "Take Web Scraping Further with Mechanize"
date: 2014-08-31 16:09:57 -0400
comments: true
categories: ruby, gem, rubygem, subreddit, mechanize, nokogiri, random, code, programming
---

Recently, the class learned how to use Nokogiri to scrape webpages. A quick Google search reveals that this is indeed the most popular web scraping gem. But many people combine Nokogiri with another gem, [Mechanize](https://rubygems.org/gems/mechanize), to extend its functionality by automating some of the work of clicking the &ldquo;next&rdquo; button or creating profiles. This post is a quick writeup of my experiences with Mechanize. Please note that I ended up using Mechanize&nbsp;_instead_ of Nokogiri, not in conjunction with it. This made parsing my html way more complicated than it had to be.

I followed a few different tutorials to do it, including&nbsp;[this](http://www.icicletech.com/blog/web-scraping-with-ruby-using-mechanize-and-nokogiri-gems)&nbsp;and [this](http://readysteadycode.com/howto-scrape-websites-with-ruby-and-mechanize).

I created an example file, `"mechanize_example.rb"`, to run from the command line. The ReadySteadyCode tutorial provides the following code snippet for getting a random wikipedia article and printing it to the terminal:

<pre>require 'mechanize'

mechanize = Mechanize.new

page = mechanize.get('http://en.wikipedia.org/wiki/Main_Page')

link = page.link_with(text: 'Random article')

page = link.click

puts page.uri

</pre>
> <span>The #link_with method is provided by mechanize, and makes it easy to pull out the random article link. The #click method instructs mechanize to follow the link, and the #uri method returns the address of the page. Notice that mechanize follows redirects automatically, so this example makes three HTTP requests in&nbsp;total.</span>

I decided to see how far I could take this example by modifying the code to generate interesting data from other websites with &ldquo;random&rdquo; functions.

First, a simple change. I swapped out Wikipedia's info for Reddit's (I spend entirely too much time on Reddit anyway.) I was able to print random subreddits to my command line.

    require 'mechanize'

    mechanize = Mechanize.new

    page = mechanize.get('http://www.reddit.com/')

    link = page.link_with(text: 'random subreddit')

    page = link.click

    puts page.uri

This code yields such glorious random subreddits as&nbsp;http://www.reddit.com/r/CrossStitch/ and&nbsp;http://www.reddit.com/r/AntiAntiJokes/.

But what else could I do besides print random urls to the command line? Could I also use it to give me the title of the top link on each page? Hint: yes.

<pre>require 'mechanize'

mechanize = Mechanize.new

page = mechanize.get('http://www.reddit.com/')

link = page.link_with(text: 'random subreddit')

page = link.click

first_link = page.search '/html/body/div/div/div/div/p/a'

puts page.uri
puts first_link.first.text.strip
</pre>

Here's where not using Nokogiri came back to haunt me: look at how complicated parsing the html had become. I was also getting error messages whenever the subreddit was empty (that means it contains no links.) But my command line printouts now magically included results like:

`http://www.reddit.com/r/cincinnati/ 
 Just framed this 1902 map up for my dad`

and

`http://www.reddit.com/r/Images/ 
 Returned to my friends car to find this letter from 7 year old Justin`

This idea can be taken so much further! I plan to extend it in the future by taking to the web and creating a game where people can vote on randomly generated subreddits that are pitted against each other. We'll see which ones are the most popular by name alone.

I might also be able to use Mechanize to create a reddit bot that responds to regular expressions, but we'll leave that for a theoretical time in the future when I understand regular expressions.

**Edit:**

Due to popular demand, I've turned the subreddit generator into a gem! See:
[https://rubygems.org/gems/subreddit](https://rubygems.org/gems/subreddit)