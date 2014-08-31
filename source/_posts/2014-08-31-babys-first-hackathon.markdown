---
layout: post
title: "Baby's First Hackathon"
date: 2014-08-31 16:18:17 -0400
comments: true
categories: hackathon, ideo, nyhackswaste
---


Over the weekend, I competed in my very first hackathon. <span>It was run by<span class="Apple-converted-space">&nbsp;</span></span>[IDEO](http://www.ideo.com/)<span>, and the theme was waste and surplus in NYC:</span>

[http://www.ideo.com/hackathon/nyc/](http://www.ideo.com/hackathon/nyc/)

I just want to reflect on my experience here since the hackathon was very different than I was expecting.

...

My friend Wenting is a UX designer at Viacom and my friend Jesse is a professional Rails developer, so they're both way more experienced than me. We agreed that I would "help" Jesse but he would do the heavy lifting of architecting the backend. Wenting had already thought of an idea and briefed us on it before we arrived at IDEO.

When we got to the venue near Canal Street, we were surprised to discover that almost nobody was a developer, and almost nobody had a ready-made team. Furthermore, the hackathon was scheduled to be very short: it was only Friday and Saturday as opposed to lasting the entire weekend. 
I have a feeling that IDEO didn't really know how to run a hackathon.

Most people got to work finding a team, but since I came with my friends we could get to work right away. We ended up picking up a fourth teammate, Hao, who is an amazing biv/dev guy and probably is the reason we were so successful. 

Hao did market research on our competitors while we started programming. Wenting sat apart from us and built the frontend while Jesse and I brainstormed what our models and migrations should look like. The lack of communication turned out to be a huge problem later because Wenting built a single-page app that couldn't be connected to our multi-page backend. We coded late into the night, went home for a few hours, and came back to IDEO at 9am the next morning.

---

On Saturday, Wenting arrived and told us that she had decided not to do photoshop mockups because there wasn't time. She was just going to build what she saw in her head. I was a little nervous but agreed that we didn't have time to build the app the usual way.

I was inordinately proud of myself a couple hours later when Jesse told me that form_for confused him so he always hard-coded the html in his forms. I insisted that we use form_for, and together we figured it out. Win for Flatiron!

---

Jesse and I inevitably ran out of time, so our app was barely functional. It was time to upload our slides, but we had almost nothing working. It could only do three things: authenticate with Google, calculate who's closest to you (the meat of the app), and send text messages (thank you, Twilio!)

---

Hao advised us that since we didn't have anything to actually _show_ the audience, we shouldn't present any tech at all. We just let him talk about how awesome the app is, and then I took questions about our plans for expansion. And we ended up taking 3rd place out of 12!

<span>As far as I can tell, there are no press releases announcing the winners. But it's all over my<span class="Apple-converted-space">&nbsp;</span></span>[twitter](https://twitter.com/IlanaSufrin)<span>.</span>

<span>I wrote a short description of our app, in case anyone is interested.

</span>

> <div>Free Stuff Fairy is a web/mobile app that connects unwanted goods--potential waste--with people who need them. It encourages the reuse of goods in New York City by making the exchange fun and easy. It dynamically generates peer-to-peer offers using a distance-based algorithm. The closest users have the first opportunities to claim an offer. Once a match has been made, Free Stuff Fairy sends a text message (through the Twilio API) to let the recipient know that they&rsquo;ve been chosen for a certain item. They can then accept or reject the offer within the given time limit. 
> &nbsp;</div>

<span><span class="Apple-converted-space"></span></span>

 My favorite code from the weekend was Jesse's distance algorithm:

<pre>class Matcher
  DELTA = 0.003

  def self.match_user_to_available_item(user)
    lat = user.latitude
    lng = user.longitude
    item = Item.available
      .where('latitude BETWEEN ? AND ?', lat-DELTA, lat+DELTA)
      .where('longitude BETWEEN ? AND ?', lng-DELTA, lng+DELTA)
      .first
    if item
      item.update(status: :unavailable)
      item.offers.create!(owner_id: item.user_id, recipient_id: user.id)
    end
  end

  def self.match_item_to_available_user(item)
    lat = item.latitude
    lng = item.longitude
    user = User.where('latitude BETWEEN ? AND ?', lat-DELTA, lat+DELTA)
      .where('longitude BETWEEN ? AND ?', lng-DELTA, lng+DELTA)
      .without_offer
      .first
    if user
      offer = item.offers.create!(owner_id: item.user_id, recipient: user)
      item.update(status: :unavailable)
      offer
    end
  end
end</pre>

<span>

Our<span class="Apple-converted-space">&nbsp;</span></span>[product](http://fennel-loves-you-too.herokuapp.com/)<span><span class="Apple-converted-space">&nbsp;</span>is definitely<span class="Apple-converted-space">&nbsp;</span></span>**NOT**<span><span class="Apple-converted-space">&nbsp;</span>functional yet but we have a plan to meet every night and finish it before BigApps.</span>

<span>---

FINAL THOUGHTS:

I would really not advise that anybody go to a hackathon until we're a bit more experienced. My friends agreed that I could approach this one as a student, and I definitely don't think I was slowing Jesse down (if anything I was helping him debug his code by asking a million questions), but I wish I had more to contribute to my team.
Also, sleep deprivation.</span>