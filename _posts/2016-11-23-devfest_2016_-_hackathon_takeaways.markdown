---
layout: post
title:  "DevFest 2016 - Hackathon Takeaways"
date:   2016-11-23 22:25:28 +0000
---


This weekend I had the opportunity to attend [DevFest 2016](https://devfestnyc.com/) in NYC -- and participate in my very first hackathon! Even better, the event was hosted at the Flatiron School's Manhattan campus! As an online student of the Flatiron School, I was thrilled at the opportunity to see the school itself. It was a fantastic experience overall and, at least for now, my programmer imposter syndrome has been put to rest.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Me: I just registered for my first hackathon<br>Co-worker: you play hacky sack??<br>Me: 😂😂😂</p>&mdash; BJ Cantlupe (@BeejLuig) <a href="https://twitter.com/BeejLuig/status/797197271767072769">November 11, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

I was extremely nervous going into this event and had thoughts of backing out right up to the beginning of the hackathon. In retrospect, I am so glad that I fought off that anxiety. Hopefully sharing my experience will help other budding developers shake any feelings of uncertainty they may have about putting themselves out there. *Just do it*. You'll thank yourself later.

This post is particularly long. If you want to skip to the actual takeaways, scroll down or click <a href="#takeaways">here</a>.

**Before the event**

I first heard about DevFestNYC from one of the Learn instructors at Flatiron. She told me that there was a team in the hackathon representing Flatiron School's online program. Great! All I had to do was get in touch with the team and I'd be all set.

Except the team was full. 

I was back to square one. The Flatiron instructors who were involved with the devfest seemed certain that I would be able to find another team, even if I was placed the day of. Considering this would be my first programming-related event *ever*, much less my first hackathon, the idea of joining a team the day of wasn't super enticing. So I kept reaching out. I found another Learn student who agreed to pair up. At least having a partner wouldn't be nearly as bad as being a straggler. This could work out after all. 

Except he had a last minute work thing.

With less than a week to go before the event, I was freaking out. What was I supposed to do without a team? Sure, I could be placed the day of, but it wouldn't be terribly helpful if I ended up with a team on a different development track. I mean, I breezed through the beginner level of [Swift Playgrounds](http://www.apple.com/swift/playgrounds/), but that doesn't qualify me to build an iOS app! What if all of the teams were full by the time I arrvied? I started to convince myself that I would be better off checking out the talks and labs and mingling with other attendees. I wasn't prepared to jump into this situation, and I didn't want to hold a team back. The stakes weren't terribly high, but it was still a competition! 


**Day 1 - the morning of**

About 30 minutes before the hackathon began, I was quietly hanging around the organizers, hoping they would help me out. One of them finally came up to me and queried "Are you a team leader?" I said "no, I don't have a team." He asked if I would be willing to be a team leader. I said "maybe next time." He asked then if I could wait for a few minutes because at the moment was only looking for team leaders. I obliged, continuing to feel like an apple at an orange party. 

<div style="text-align: center; width: 100%; background: white;">
  <img src="http://www.supergraphictees.com/wp-content/uploads/2013/05/retro-apple-orange.jpg" width="300px" />
	<p style="margin-top: -10px; padding-bottom: 10px;"><em>get it?</em></p>
</div>

Once all of the teams had been organized, I noticed a small group of people kind of standing around in the corner. One of the organizers ushered them towards an open desk in the back of the room. I had a feeling these were the other stragglers. This was my one shot to get a team and do the thing I came to do. I walked up to them and asked if they had just formed and if I could join them. Their answers were yes and yes. YES. I was finally on a team, and they were all web developers. My people! All of the anxiety washed away. Then...

**The app idea**

Our team leader, Eric, polled everyone to figure out what kind of app we want to make. We collectively liked the idea of creating a productivity-based web application that would support educators or education in some way. A few pitches came out and were quickly shot down. My first idea was to create a cloud-based clipboard for code snippets. I had thought of it on the subway ride over to the event and thought it was pretty clever, that is, until Eric pointed out that it has [already been done](https://gist.github.com/). Woops. I quickly recooped from my first flub and suggested an app idea I had come up with in abstraction a while ago: an online console to help private teachers and tutors schedule and manage their student-base. They liked it! Turns out not all of my ideas are copyrighted by Github. 

<div style="text-align: center; width: 100%;">
  <img src="https://media.giphy.com/media/xmQh8NQ78y8QU/giphy.gif" width="300px" />
</div>

Now that we agreed on an idea, we had to figure out the design and implementation. For that, the team looked to me. The anxiety came back. I thought I remembered saying that I *didn't* want to be in charge of anything. Too late now!

**Building it out**

I did my best to lay out my vision for what the app should look like. The main feature of the app would be an automated scheduler. As a teacher, you could sync you Google Calendar into the app and mark your blocks of weekly availability. Your students would then do the same, and the app would propose a schedule that you could then track daily. The teacher could update the app when they complete a lesson and indicate whether they were paid, which would be used later to track earnings and possibly create invoices. The feature possibilities for this app are endless, and we went down the rabbit hole for a while before we really got started. Ultimately, we decided we would build out the console itself and focus on the Google calendar integration. 

The only requirement for this hackathon was that our app had to use Google's backend infrastructure API, [Firebase](https://firebase.google.com/). We opted to use the real-time database and Google OAuth features for ours. Half of our team worked on connecting the Firebase API to the backend of our app while I paired with another team member, Samantha, to work on the frontend. Samantha is near graduating from another NYC coding bootcamp. She is fluent in [React](https://facebook.github.io/react/), the frontend Javascript library created by Facebook. I am not. Luckily I had spent the previous week going through the [Codecademy](https://www.codecademy.com/) courses on React. With Samantha's guidance I could (sort of) keep up. 

The rest of day one was a scramble to get *anything* going. We spent a little too much time trying to figure out the logic of the app's main function -- automated scheduling -- and it cut into precious coding time. About an hour before the day was through, I took a stroll through the Flatiron school and struck up a conversation with one of the Google Dev Group volunteers. We discussed my problem with the scheduling logic and he gave me an invaluable piece of advice: in a hackathon setting, your app just has to *look* like it works. By the end of the day, we had all agreed to keep working at home. It didn't feel like we had gotten very far, but there was still hope. 

**Day 2 - moar code**

By day two of the hackathon, we had a name (Teacherly), a logo, OAuth integration and a shell of the user console. We decided to focus on the parts of the app that would be easily demonstrated during our presentation to the hackathon judges (and everyone else). We worked on building out the teacher console including an interactive calendar view, list of active students, and completed lesson prompt. Working out the automated scheduler logic would have been too complicated and time-consuming, so we decided it would just have to be explained in the presentation. Oh, by the way, I was volunteered to do the presentation.

<div style="text-align: center; width: 100%;">
  <img src="https://media.giphy.com/media/26BRq84rhISRcFVUQ/giphy.gif" width="300px" />
</div>

I think we really got into a flow during day two. There wasn't much discussion, just coding. I spent a few hours working on the CSS files, fixing style and positioning issues. When all was said and done, we had finished our prototype with an hour to spare! I practiced my demo speech in front of the team a few times. 4pm crept up on us, and it was time to present!

**Presentations**

There were about 15 teams participating in the hackathon. Each team had four minutes to present their apps and give a demonstration, then there was a one-minute Q&A with the judges. Yeah, it was quick. Apps were judged on their idea, earning potential and user interface. Based on the judging, there was a grand prize and three runner-ups. There was also a prize for the crowd favorite. 

I was really nervous going into the presentation, but I'm not exactly a newbie to public speaking. I felt pretty good about my presentation, even though we ran out of time before making it through all of our slides. The most important part of the presentation is how you handle the Q&A. The judges had good questions and fortunately I had prepared answers for them. 

Considering we were the "straggler" team, started without a plan, and had to scramble to get a half-decent web app together, I frequented the kegerator right after presenting. There was no way we would win; just making it to the finish line was a win in itself. I figured I'd hang around to find out who won out of interest and respect. There were a ton of great ideas put out there, and all of the prototypes were really impressive. 

I was standing in the back of the room, near the kegerator, congratulating my teammate Jack on a job well done when we hear "2nd runner up, TEAM SUCCESS!" Jack and I shared a confused look for a moment, then we realized...we won! Not first place, of course, but we won something! The most unlikely team from day one somehow got it together and earned a spot on the podium!

<img src="https://photos.google.com/photo/AF1QipOKQDzl0Avhuf-hTdXWO0D3ua-FWV_ko2ZG71Hk" alt="We did it!" />

`🏆🏆🏆`

**<a name="takeaways">Takeaways</a>**

If you skipped the long story, you ended up here. Congratulations, your attention span is as short as mine! Without further ado, here are my takeaways from the DevFest2016 hackathon, in no particular order:

* *Just do it*: If you want to participate in a hackathon but aren't sure if you are ready, guess what: you are. Experience level matters very little. Everyone is there to learn a ton, make something cool and be awesome to each other

* *Come prepared*: If you have an idea for an app, make some notes and come up with details. You'll never know when it'll come in handy. 

* *Study up*: If a hackathon has some sort of framework or language requirement, give yourself ample time to research. Anything helps

* *Stay positive*: In the middle of a hackathon, it's easy to feel like things aren't coming together or you're not gonna make it. You will. 

* *Ask for help*: When you get stuck, there are usually people around you who are happy to share their expertise with you. A hackathon is meant to be a learning experience. Even your competitors may be helpful

* *Remember why you are there*: Hackathons are draining and can be emotional. No matter what happens though, you will meet new people, learn new things, work hard and ultimately succeed in putting together a new idea! That's awesome. 

* *Believe in yourself*: It's sappy, I know. But it's true! You are awesome. If you try your best, others will see the best in you. You are awesome! 

Thanks for reading! If you want to chat with me about my hackathon experience, feel free to find me on [Twitter](https://twitter.com/beejluig). Flatiron students can find me on Slack!

Below is a homepage screenshot of my team's web app, Teacherly

<img src="/teacherly-screenshot.png" width="500" />





