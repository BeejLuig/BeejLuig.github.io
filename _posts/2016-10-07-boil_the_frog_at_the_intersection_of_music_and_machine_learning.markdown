---
layout: post
title:  "Boil the Frog: At the Intersection of Music and Machine Learning"
date:   2016-10-07 01:43:54 +0000
---


Earlier this week, I was listening to [Episode 279](https://devchat.tv/ruby-rogues/279-rr-vets-who-code-with-jerome-hardaway) of the [Ruby Rogues](https://devchat.tv/ruby-rogues) podcast. At the end of the episode, the Rogues take turns announcing their "picks" for week. A pick can be anything from an outdoor activity to a new ruby gem recommendation. This post discusses panelist Coraline's pick: [Boil the Frog](http://static.echonest.com/BoilTheFrog/). 

**What does "boil the frog" even mean?**

Boil the Frog is a simplistic-looking web app that takes in two artists as input, then spits out a playlist that connects them through similar artists. The[ "About"](http://static.echonest.com/BoilTheFrog/) section describes it best:

> Boil the Frog lets you create a playlist of songs that gradually takes you from one music style to another. It's like the proverbial frog in the pot of water. If you heat up the pot slowly enough, the frog will never notice that he's being made into a stew and jump out of the pot. With a Boil the frog playlist you can do the same, but with music. You can generate a playlist that will take the listener from one style of music to the other, without the listener ever noticing that they are being made into a stew.

**Why use it?**

How many times have you tried to convince a friend to listen to an artist that he/she has no interest in? "Just trust me," you say. "You may not *think* you will like this band, but they are actually just like some of the other stuff you like." This is the kind of situation where Boil the Frog shines: connecting unlikely artists through degrees of musical separation. 

Some examples listed in the Boil the Frog about:
* [Miley Cyrus to Miles Davis](http://static.echonest.com/BoilTheFrog/index.html?src=%27miley%20cyrus%27&dest=%27miles%20davis%27)
* [Justin Bieber to Jimi Hendrix](http://static.echonest.com/BoilTheFrog/index.html?src=%27justin%20bieber%27&dest=%27jimi%20hendrix%27)
* [Mickey Mouse to deadmau5](http://static.echonest.com/BoilTheFrog/index.html?src=%27mickey%20mouse%27&dest=%27deadmau5%27)

Gotta love the alliteration!

**What happening under the hood?**

Boil the Frog is powered by music data aggregator [The Echo Nest](http://the.echonest.com/) which, as of this writing, has 3,519,350 known artists and a whopping 37,484,281 songs stored in their databases. Both of those numbers went up before I could finish that sentence. The web app was built by The Echo Nest's Director of Developer Platform, Paul Lemere.

According to the site, a similarity graph of about 100,000 artists is created every time you "boil the frog." Similarities between artists are calculated through an algorithm provided by The Echo Nest.  The playlist is defined by the path through the app, selected by the app. The path between one artist and the next is selected by their relative popularity. The songs are selected after the artist. The app aims to maintain the energy level from song to song, keeping the musical "boil."

**My playlist**

Here are a couple of my favorite playlists
* [NOFX to D'Angelo](http://static.echonest.com/BoilTheFrog/?src=nofx&dest=dangelo)
* [Galactic to Bruno Mars](http://static.echonest.com/BoilTheFrog/?src=galactic&dest=bruno%20mars)
* [Amy Winehouse to Donny Hathaway](http://static.echonest.com/BoilTheFrog/?src=amy%20winehouse&dest=donny%20hathaway)

I have been having an absolute blast playing around with this web application and I highly recommend it. If you are someone who is: 1. looking to check out new music 2. trying to introduce someone else to an artist you like or 3. want to bask in the glory of algorithmic awesomeness, you will enjoy Boil the Frog as much as I have. 

**Learn more**

Try Boil the Frog [here](http://static.echonest.com/BoilTheFrog/)
See the source code for Boil the Frog [here](https://github.com/plamere/boilthefrog-spotify-app)
Check out The Echo Nest [here](http://the.echonest.com/)
Check out Paul Lamere's blog [here](https://musicmachinery.com/)


